

next unless does not work how I'd think it would...

It seems like it would mean 'do the next thing UNLESS whatever condition I name after the unless evaluates to true.' However, this is not the case. 

2.1.5 :046 > thing = true
 => true
2.1.5 :047 > arr.each do |item|
2.1.5 :048 >     next unless thing
2.1.5 :049?>   puts item
2.1.5 :050?>   end
cats
pandas
dogs
snakes
 => ["cats", "pandas", "dogs", "snakes"]
2.1.5 :051 > arr.each do |item|
2.1.5 :052 >     next unless thing
2.1.5 :053?>   puts item
2.1.5 :054?>   thing = false
2.1.5 :055?>   end
cats
 => ["cats", "pandas", "dogs", "snakes"]

Unless is just the reverse of an if statement -- the following block only executes if the condition is false. https://signalvnoise.com/posts/2699-making-sense-with-rubys-unless#comments

So I think what's confusing me here is understanding what next does...