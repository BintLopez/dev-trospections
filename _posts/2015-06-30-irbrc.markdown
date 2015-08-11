---
layout: post
title:  "Customizing irb with .irbrc"
date:   2015-06-30 22:43:00
categories: jekyll update
---

IRB::Inspector
inspect method

###Do you use rvm as your Ruby version manager?
If so, you should know that rvm uses a global default $IRBRC. If you want your command line interface customizations to be specific to your rails project, you'll need to take an extra step. Instead of starting rails console as you normally would -- `rails c` -- you should start it with `IRBRC= rails c`. 

Here `IRBRC=` allows you to bypass rvm's default .irbrc file. Don't be deceived -- we did not change the `$IRBRC` global variable path. The use of your project's .irbrc file instead of rvm's default is only specific to that console session. It does not alter the default load path for .irbrc that will be used in future sessions.

###How can I check the path of my default .irbrc?
Type `echo $IRBRC` into your terminal to check the path of the global variable $IRBRC.

You can also use `unset IRBRC` to remove the path for $IRBRC. However, this will only last for the duration of that terminal session.


{% highlight ruby %}
inspector = proc{ |v|
  if v.respond_to? :my_inspect
    v.my_inspect
  else
    v.inspect
  end
}

ins = IRB::Inspector.new(inspector)
IRB.CurrentContext.inspect_mode = ins
{% endhighlight %}



[class-instance-eval]: http://web.stanford.edu/~ouster/cgi-bin/cs142-winter15/classEval.php
[include-v-extend]: http://www.railstips.org/blog/archives/2009/05/15/include-vs-extend-in-ruby/
