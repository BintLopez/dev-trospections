<!-- ---
layout: post
title:  "Resque Me, rspec! And lessons learned in testing"
date:   2016-01-19
categories: testing resque rspec mailers
--- -->

When you embark on writing a unit test, it's really easy to approach the test with the correct mentality. The mentality that no, you won't rely on Factory Girl data, of course you'll stub and mock out everything you need, and yes, the test will definitely be a true unit test. However, when the setup of your test involves ActiveRecord relations, callbacks, and tons of associated data the task becomes more daunting. When you throw in the additional complication of testing logic in background job processors and mailers, it's easy to throw your hands up in frustration and succumb to Factory Girl's siren song. Don't succumb to the sirens. Tempting as they are, they only lead to shipwrecks.

First, let's cover the basics. Unit tests vs acceptance or integration tests. What is a true unit test? A true unit test only tests one unit of logic. A true unit test has no external dependencies... on the database, on http, etc. So lets say you have a job processor that runs a scoped query and iterates through each returned object to then determine whether or not to kick off another job that ultimately sends an email. Let's call this job processor class `MyJobProcessor`. True unit tests for the `MyJobProcessor` class would not test whether the email is actually delivered. It would test the logic specific to that class, i.e. whether it makes the correct decision to kick off or not kick off the email sending job. True unit tests for `MyJobProcessor` would also not test that the correct set of objects were returned from the scope. The scope should be separately unit tested where it is defined. In contrast, an integration or acceptance test would test the entire process, end-to-end. It would test that the scope returns the correct set of data and then emails are triggered and delivered for the desired subset of email-eligible objects.

I've recently had a few run-ins with issues in testing, ranging from rookie rspec mistakes to some trickier gotchas. And so I give you these tips so that you may avoid the blood, sweat, tears, and general suffering that I experienced while trying to fit square pegs in round holes to get some solid test coverage.

Let's work through the following example. Let's say I have the following code that needs unit test coverage.

```ruby
class AccountActivationReminderJobProcessor
	
	def send_acct_activation_reminder
		Customer.new_without_active_account.each do |cust|
			ReminderMailer.validate_account(customer: cust, second_reminder: is_second_reminder?)
		end
	end

	private

	def is_second_reminder?
		...
	end

end
```

I want to write unit tests for this. Now I could do something like this...

```ruby
Rspec.describe AccountActivationReminderJobProcessor do
	
	let(:customer) { FactoryGirl.create(:customer) }
	let(:run_job)  { Resque.run_for!(AccountActivationReminderJobProcessor.queue) }

	context "New customer needing account activation reminder email" do
		before { customer.account.update_attribute(:status, "inactive") }

		it "sends the email" do
			AccountActivationReminderJobProcessor.create job: "send_acct_activation_reminder"
			run_job
			refute ActionMailer::Base.deliveries.empty?
		end
	end

end
```

Awesome. Done. Now that was easy, right? Not so fast! There are more than a few problems with this. For starters, this isn't a unit test; this is an integration test. What are we actually testing here? The scope? The job processor? The mailer? Also, this test relies on FactoryGirl data. Sure, it's quick and easy to type out `let(:customer) { FactoryGirl.create(:customer) }`, but what do you actually get with all of that? Perhaps the default setting for your customer factory doesn't just create a customer. Perhaps it creates a customer object, their related person object, their address, and their customer application. All of that extra data creation adds precious time to your test run and adds a potential source of mysterious failures. For example, what if the address samples from an array of different locations and triggers some location-based logic that causes inconsistent behavior?

Let's take a second pass at this. First, do I really need to create a customer to test the job processor? No, I don't, so I can use an instance double instead with `let(:customer) { instance_double(Customer) }`. But what about my `before` block? 
```
before { customer.account.update_attribute(:status, "inactive") }
```

The instance double of customer obviously doesn't have an account. But we can fix that with the following mocks and stubs:

```
let(:inactive_account) { instance_double(Account, status: "inactive") }
before do
	allow(customer).to receive(:account).and_return(inactive_account)
end
```

This would be an overall improvement, especially in respect to test run time. However, as it stands, the test will still not work, due to the `new_without_active_account` scope on Customer. As we've defined it in our test, `customer` is a double, not an instance of Customer, and thus it will not be returned from a scope on Customer. This can be fixed with some additional, trickier mocking.




```ruby
Rspec.describe AccountActivationReminderJobProcessor do
	
	let(:relation) { instance_double(ActiveRecord::Relation) }
	let(:customer) { class_double(Customer) }
	let(:run_job)  { Resque.run_for!(AccountActivationReminderJobProcessor.queue) }

	context "New customer needing account activation reminder email" do
		before { customer.account.update_attribute(:status, "inactive") }

		it "sends the email" do
			AccountActivationReminderJobProcessor.create job: "send_acct_activation_reminder"
			run_job
			refute ActionMailer::Base.deliveries.empty?
		end
	end

end
```





No external dependencies (db, http, etc)
everything but the subject is fake


First ask yourself what you are actually trying to test?

I recently struggled through some issues testing an argument passed into a mailer from within a job processor. Unit testing resque jobs and mailer classes can be difficult.

Let's say you're trying to 



```ruby
context "resend: true is passed as a param to the job processor" do
  let(:annual_statements_job_processor_with_resend) { AnnualStatementsJobProcessor.new(1234, {job: "send_annual_statement", resend: true}) }
  let(:prev_statement) { instance_double(AnnualStatementsLog, statement_data: "data") }
  let(:relation) { instance_double(ActiveRecord::Relation) }
  
  before do
    allow(loan).to receive(:annual_statements_logs).and_return(relation)
    allow(relation).to receive(:order).and_return([prev_statement])
    allow(annual_statements_job_processor_with_resend).to receive(:statements_mailer).and_return(email)
  end

  it "does not raise an error" do
    expect{ annual_statements_job_processor_with_resend.perform }.to_not raise_error
  end
end
```

```ruby
context "Testing arguments passed to the mailer" do
  let(:msg) { instance_double(Resque::Mailer::MessageDecoy, deliver!: true) }
  before { allow(AnnualStatementsMailer).to receive(:annual_statement).and_return(msg) }

  context "Given a uuid and resend: true" do
    let(:prev_statement) { instance_double(AnnualStatementsLog, statement_data: email_data) }
    let(:relation) { instance_double(ActiveRecord::Relation) }
    
    before do
      allow(loan).to receive(:annual_statements_logs).and_return(relation)
      allow(relation).to receive(:order).and_return([prev_statement])
      allow_any_instance_of(LoanAnnualStatementsPresenter).to receive(:present).and_return(email_data)   
    end

    it "sets data to the most recent annual statement log's statement_data" do
      AnnualStatementsJobProcessor.create job: "send_annual_statement", uuid: "12345", resend: true

      expect(AnnualStatementsMailer).to receive(:annual_statement).with(loan.id, data: email_data)
      run_job
    end
  end

  context "Given a uuid and correction: true" do
    before do
      allow_any_instance_of(LoanAnnualStatementsPresenter).to receive(:present).and_return(email_data)
      allow(AnnualStatementsMailer).to receive(:annual_statement).and_return(msg)
    end

    it "sets data to the most recent annual statement log's statement_data" do
      AnnualStatementsJobProcessor.create job: "send_annual_statement", uuid: "12345", correction: true
      
      expect(AnnualStatementsMailer).to receive(:annual_statement).with(loan.id, data: nil)
      run_job
    end
  end
end
```


tim:  "the primary culprit would be tests that create records in separate threads which will not run within the context of the main thread's transaction"
"yes, it permanently mutates them on the class.  You could unset/set them but that is not great either because then the implementation that gets reset by the test must stay up to date with any conditions that are set for the callback on the model"

the importance of using let vs = in tests -- "The test is setting one_year_ago when the test file is evaluated/parsed, rather than at the time the examples actually run. This causes a failure when the spec runs over midnight where the test suite starts running and evaluatesone_year_ago before midnight, but the examples themselves don't run until after midnight.""

# tricks from Kel
When you're expecting something to receive something else, it is destroyed the instant it's called!
.and_call_original
