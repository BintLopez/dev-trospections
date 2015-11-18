---
layout: post
title:  "Windy City Rails"
date:   2015-09-16 22:43:00
categories: jekyll update
---


##Sarah Mei -- SOLID and design patterns

Single Responsibility Principle 	one responsibility per class
Open/close Principle 							don't edit code; add new code
Liskov Substitution principle 		inheritance -- it's a thing
Interface segregation principle 	makes more sense for compiled languages but make the api small
Dependency Inversion Principle 		makes it easier to test classes

utility levels of SOLID:

S - med
O - low
L - low
I - high
D - high

####Design Patterns

At what point does the lower cost of change outweigh the higher cost of understanding? Think of the observer pattern

####Strategy and Tactics

Trying to apply SOLID to a 1000 line class is like scaling Everest without a plan.

Smell your code
Tiny problems first
Augment tests - need tests one level of abstraction higher than your current level (for the refactor)
Back up - don't be afraid to stop going down the path if it's the wrong abstraction
Leave it better than you found it
Expect good reasons

When you tackle the smaller problems first, the solutions to the larger problems become clearer.

"Work on making your code STABLE and it will eventuall be SOLID."


##Rails for the Long-term (Braintree Guy Talk)

* As the app gets bigger, it's going to get harder to maintain. The best time to make a change is always going to be right now. When the team and codebase grow, it becomes less and less feasible for new people to know everything about the system.
* Service oriented architecture

`"You can't build a development strategy around heroes"`

`"The code must speak with one voice or else you will all go crazy"`

* Explicitly adopt patterns. 
* Early decisions are hard to change. "Developers live in the worlds we create for ourselves"
* BT 'sharded' the database. But now really hard to do cross-shard queries. 'postgres clusters'
* All decisions are early decisions.
* Follow institutional practices.


##Refactoring Rails Apps with Engines

* engines pack up things into a gem that can be easily re-used
* engines can also be used to break up app into more maintainable chunks
* engines can live in a single engines directory
* avoid circular dependencies
* the main rails app depends upon the other engines in your app
* has_many creates circular dependencies
* tests for engines live within the engines themselves
* one model and table per engine
* service engines -- engines that are the bridge of communication between existing engines
* aggregate engines -- engines inside engines

Followup questions:
* How to implement an engine?
* How do you use the magic of rails with your engine
* are all the models still active record?
cbra.info

##GO
* concurrencty is not parallism talk
* Go for rubyists -- previous windy city rails talk

##Importance of Defaults
* What is the default development, deployment, testing process
* What are the default values
* Defaults are the unknown unknowns

##Parallela 
* This is an epiphany-based processor
* Da faq is Moore's law?!
* Parallelism is the answer to making things faster -- multiple cores running simultaneously
* In the context of rails -- using diff cores for diff queries


##Communicating Code

KISS my SOLID YAGNI!
1. What are you doing?
2. Why are you doing it?

code != art
code == a_manual

s/o to Ikea manuals.

###the WHAT

How to make code more manual like
* Class names
	- Things (m, v, c... has state) - easy to name
	- Processes -- harder to name (CreditCardCharger (worse), ChargeCreditCard (better in his opinion))
* Namespacing
* Method naming

```
module Reverb
	module Checkout
		class ChargeCreditCard

			def initialize(credit_card:, cart_order:)
				@credit_card = credit_card
				@cart_order = cart_order
			end

			def charge_credit_card
				#the code that charges the code
			end

		end
	end
end
```

* Be EXPLICIT
* Better to explain *too* much rather than not enough

* Metaprogramming -- can sometimes be 'false economy'. Saved time when typing but not on the understanding part
* Use metaprogramming as salt (just a little bit) -- unless you're programming a DSL or framework

###the WHY

* Be explicit in your commit messages so ppl can fix out what you were doing and why
* Good comments -- add extra info that's not apparent from the code
* HAVE EMPATHY


##Acceptance Tests Talk
* production code is NOT more valuble than test code
* Myth:  acceptabnce tests are driven by acceptance criteria and should mirror this as much as possible
* Myth:  the more acceptance tests you have, the more stable your system of tests is

Question:
* What is the test pyramid?


##My Opinions

I hate foo bar examples. They are too abstract.
