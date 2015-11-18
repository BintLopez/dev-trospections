---
layout: post
title:  "Class vs Instance Methods & Variables"
date:   2015-06-30 22:43:00
categories: jekyll update
---

Whether or not to use a class level or instance level variable or method is a design choice. In general, we should use class level methods and variables for things that will not change from instance to instance.

{% highlight ruby %}
code sample goes here
{% endhighlight %}

Instance variables in modules refer to the Class in which that module will be used.
- http://stackoverflow.com/a/15479033

{% highlight ruby %}
Inside of a class level method, self refers to that instance of class Class.
class Something

	def self.class_level_method
		self #here self refers to class Something, which is an instance of class Class.
	end

end
{% endhighlight %}


[class-instance-eval]: http://web.stanford.edu/~ouster/cgi-bin/cs142-winter15/classEval.php
[include-v-extend]: http://www.railstips.org/blog/archives/2009/05/15/include-vs-extend-in-ruby/