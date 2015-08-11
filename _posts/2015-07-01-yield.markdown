---
layout: post
title:  "Yield"
date:   2015-07-01 22:43:00
categories: jekyll update
---

Yield allows you to return a value without escaping from the method. The method 'returns' the value but continues to execute until its end.

"yield is used to execute a block that you pass to the method, and then you do something with the result of that call." - http://stackoverflow.com/a/18995184

The method in which yield is called *must* take a block as an argument.

Examples...
{% highlight ruby %}
def some_method(arg, named_arg1: nil)
  puts "I puts my first arg here: #{arg}"
  puts "I puts the named argument, 'named_arg1', here: #{named_arg1}"
  puts "Now i'm about to yield the value 3"
  yield 3
  puts "Now i'm back in 'some_method'"
  puts "my return value will always be 4"
  4
end


some_method(1, named_arg1: 2) do |value_from_some_method|
  puts "The value I got from 'some_method' is: #{value_from_some_method}"
end
{% endhighlight %}

Another example of yield
{% highlight ruby %}
class GabeArray < Array

  def each 
    i = 0
    while (i <=self.length)
      yield self[i]
      i += 1
    end
  end

end

arr = GabeArray.new.compact
[1,2,3].each{ |e| arr << e}
(arr << [1,2,3]).flatten
arr.each do |element|
  puts "element is #{element}"
end
{% endhighlight %}



<!-- [class-instance-eval]: http://web.stanford.edu/~ouster/cgi-bin/cs142-winter15/classEval.php
[include-v-extend]: http://www.railstips.org/blog/archives/2009/05/15/include-vs-extend-in-ruby/ -->