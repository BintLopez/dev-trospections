---
layout: post
title:  "Windy City Rails - Day 2"
date:   2015-09-16 22:43:00
categories: jekyll update
---


##The Journey of an HTTP request

rubygems.org. A IN
. after org b/c it needs to know when to stop
A for address


DNS
------
Local Cache
Router
ISP
Registry

rubygems.org(query)
TTL (cache length)
IN (classification)
A (record -- the address)
IP ADDRESS (value)

UDP -- user datagram protocol
-- really really fast but no guarantee of delivery
(videogames and DNS queries)


TCP -- Transmission Control Protocol
-- slower, but guarantees delivery

Internet
-----
Browser opens up a socket to chat w/ server
BGP -- border gateway protocol
-- lets ISPs/networks share their network routes
-- lets router figure out the next destination to hand off the packet
Router knows quickest path to get to the destination server

Server
------
Then server has to respond
It's listening on the following ports (or perhaps others) 
80 http
443 https

Request & request headers
Accept: text/html
Accept-Encoding: gzip, deflate, sdch
Accept-Language: en-US
Connection: keep-alive
Host: rubygems.org
User-Agent: (what browser's sending the request)

Response & response headers
server: nginx
Date: [date]
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connnection: close
status: 200 OK
Strict-Transport-Security: [where to communicate in future for secure connection?]


Terms to look up
* Strict-Transport-Security


##How-To Performance
benchmarking vs profiling -- need both of these to up performance
Benchmarking -- how slow our code is
Profiling tells us what parts of the code are slow

1. First -- need a baseline. Need something to MEASURE
* she built an app to measure the speed of integration tests

Controller tests and integration tests are quite similar

TIME NOT A GOOD BASELINE for measuring performance
- it varies a lot

What's a good baseline?

benchmark-ips <-- good gem for finding your baseline

2. Second -- find the culprits
What's slowing stuff down???
gem --> rubyprof (profiling gem)

Callstack printer

StackProf -- ruby profiling like RubyProf
-- isolates parts of stack instead of just big pic like RubyProf

Always
Be
Benchmarking

DELEGATE was the culprit

RubyVM.stat

creating constants in tests is costly
caching is expensive

'Optimize first, cache second'

'busting a global constant'

AllocationTracer -- finding how many objs are allocated by Ruby and where they are

look up #freeze on strings --- makes your string immutable
must prove that string is a bottleneck before freezing it

These changes were ported back to rails 4.2 b/c they didn't change behavior


##HIDE COMPLEXITIES OF RUBY

* containerizing
* How to package your rails app

lobsters -- let's you pretend that you're hacker news 

should consider containers like better packages

Kubernetes is like google's borg system. Good container for production