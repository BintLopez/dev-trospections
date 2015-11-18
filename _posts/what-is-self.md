---
layout: post
title:  "What is Self?"
date:   2015-06-30 22:43:00
categories: jekyll update
---

In ruby, everything is an object, and the keyword `self` refers to the instance of the current object within that scope.

##class_eval and instance_eval
`ClassName.instance_eval` defines a class level method (instance of the class -- not to be confused with an instance of the object. These methods can’t be accessed by a specific instance of the class.

`ClassName.class_eval` defines methods on the class that can be accessed by specific instances of the class -- applies to all instances of ClassName.

##include vs extend
[include vs extend][include-v-extend]
Include is for adding instance level methods to a class. Extend is for adding class level methods to a class.

{% highlight ruby %}
def self.included(base)

end
{% endhighlight %}

When 'self' refers to the class
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
