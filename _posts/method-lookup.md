---
layout: post
title:  Method Lookup
date:   2015-11-18 22:43:00
categories: rubyconf method-lookup
---

use the ancestors method on a class!

a class method is just a singleton method on the class
methods outside of a class (top-level) end up being private methods on the object class
top level methods can be called from within any class

you can override a method from a module with a method from a class, but not vice-versa... not if you use include

prepend allows the module to override the class method

Refinements make monkeypatching safe!

```ruby
module MyOverrideModule
	refine String do
		def capitalize
			...
		end
	end
end

class TargetClass
	using MyOverrideModule
	...
end
```