---
layout: post
title:  General RubyConf Notes
date:   2015-06-30 22:43:00
categories: rubyconf soft-typing
---

#Gems - when to use them, what to assess

How to avoid using the wrong gem, or using gems recklessly

Needs Assessment
* avoiding overzealous gem use
* think of the problem from multiple angles
* why is ita problem?
* get a diversity of opinions
* how will your users be impacted? have you considered the impact on ALL stakeholders

Pitfalls of gems:
* unknown dependencies
* break on upgrade
* "why did we install that one?"
* maybe it's not the best solution to your problem
* security risks -- esp ones not on rubygems. Whoa -- apparently 66% of gems have known security vulnerabilities
* Memory risks -- some gems have mem leaks -- leaky-gems repo on GH

Why?
* start up culture -- ie SHIP IT NOWWWW
* groupthink
* poor planning -- thought out the obvi cases, but not the edge cases
* imposter syndrome -- can I do it better than the gem? 

Gem Assessment
* how well is it maintained? When was the last commit? Has it been updated since the most recent version of ruby?
* what is the community trust and commitment to it?
* are there unaddressed issues that have been festering? What kinds of issues have been addressed? Were they addressed quickly?
* is it tested? 
* If you have the time, read the sourcecode. If you don't, read the tests.
* Is it well documented? How's the code? Is it legible, well-commmented, DRY, SRP?
* How's the contribution policy?
* Does it have a code of conduct? Would you want to work with these people?
* Use www.rubysec.com/advisories to know about vulnerabilities w/ gems
code climate, gymnasium, deppbot -- services that will scan for vulnerabilities
gem called brakemen -- will scan your code base for vulnerabilities
checking style -- rubocop

Like a gem? Shoe fits? Then contribute to it! Get involved :-D

No gem fits? Need to DIY

Good process:
Overcome
Assess
Research
Build
Maintain

Define the problem concretely
* current behavior
* ideal behavior and why
* edge cases
* risks
* why haven't others solved it?

Look at gems that come close to solving the problems
* read their tests and their code
* look at their dependency list
* look at what works, what doesn't, and why

Did any of the gem authors write about what they did? Can you learn from their process? Is there a tutorial or blog post about it?

How to DIY build
* map out your solution
* model it and figure out its architecture
* use TDD or BDD
* document it as you go
* the headache test:  if you hate working on it, then something's probs not working
* HAVE FUN!

Why did you DIY?
Be able to explain your decisions to someone else

Level of Complexity
Whether using a gem or DIYing, KEEP IT SIMPLE as possible

If you use a gem...
* yay! you're not the maintainer...
* but... also can't control what the maintainer does or doesn't do