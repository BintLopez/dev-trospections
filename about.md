---
layout: page
title: About
permalink: /about/
---

This is me...

{% highlight ruby %}
class Human

  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def loves_to_code?
    puts "Do you love to code?"
    answer = gets.chomp
    if answer == "yes" || "y"
      puts "#{name} <3 coding."
      true
    else
      false
    end
  end

end

me = Human.new('Nicole')
{% endhighlight %}

As you can see in the code above, I'm an instance of the human class. As you might guess, if I were to run `me.loves_to_code?` the return value would be `true`.

I'm currently working as a Software Engineering Intern at an awesome tech company. I don't have a formal background in computer science or tech, and this is my first experience in a professional dev environment. I'm learning so much that my brain is constantly exploding! And I mean that in the best way possible! :-D

These dev_trospections, if you will, are my effort to solidify the concepts I'm encountering. If I am understanding something incorrectly, by all means, kindly let me know.
