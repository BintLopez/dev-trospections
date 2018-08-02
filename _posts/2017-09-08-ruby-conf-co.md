---
layout: post
title:  Ruby Conf Colombia
date:   2017-09-08
categories: rubyconf colombia
---

# RubyConf Colombia -- Medellin

## Palabras nuevas en espanol

One ear with the translation device and one ear escuchando en espanol!

'para tener en cuenta' -- 'to keep in mind'
escarapelas -- name tags
charlas -- talks

## An Engineering Culture Where YOU Matter
- @buritica

Engineering culture... what is it?
* not a ping pong table
* lot of companies tout ^^ as culture

"The behavior you reward or punish" -- definition of culture
* how management can intervene to cultivate a good engineering culture

"How to influence culture without authority in an industry where poor management is common"
* unpaid labor is not good but not always feasible to quit a toxic culture
* not all job markets are like silicon valley where you can have multiple offers
* Be realistic about your scope of influence and the capacity you have

1- Trust is important (need people to trust you)
'trust > authority' -- not sure if I buy this tbh
* give visibility to your work - he used a public dashboard to show what he's up to
  * show what you're doing
    * how is this different than PM software like pivotal?
    * how is this different than stand? 
    * does this supplement stand or are these processes
  * communication
* Be on time -- others will trust you
* Deliver on your promises -- stay true to your word
* Importance of support time

"If all your attention goes to the code you're writing, none of it goes to those you write code with"
* need to make time for others

2- Find allies
* identify common values & create patterns around those common values
* do the things you can do on your own w/o authority like attending meetups, creating a study group, taking an online course together, etc

D&I ideas
- share an article and discuss at lunch
- study group for career development

^^ Ways to create unofficial processes and culture
But once it's a process, it's a product that you can ship, market, and iterate on
Alternate unofficial process can 'disrupt' poor management

## Scaling a RoR app -- Breaking up the monolith into microservices
### Phase 1 -- move fast, start up
* fast > perfect
* the problems of scale
* no tests (omg)

### Phase 2
Improve the Infra
* data lake, data warehouse
* rabbit mq

Improve the Engineering
* wrote tests

Optimize the DB
* use indexes
* don't make unnecessary db calls -- cache things!!

AR specific stuff
* Use indexed columns in the where
  * `Driver.where(id: 10)` < `Driver.select('cedula, name').where(id: 10)`
* Load info in batches -- activerecord-import gem

Endpoints should be single purpose

### Phase 3
- upgrading all the things
- delete all the dead code paths and tools
Architecture Improvements
- 'a relational db + stored procedures does not scale well w/r/t concurrency'

To look up
- FreeBSD
- Tile38 -- leader/follower architecture

## Style Consistency in a 13 Year Old Code Base
- shopify is biggest rails shop in the world
- enforcing a strict style goes against the ruby way; ruby was designed to be flexible
- RubyCop didn't even work on their codebase
- Decided to only enforce style on new or modified code
- Gem: Policial -- only runs rubycop on code that's being added or changed

"Make all lines of your methods operate on a single level of abstraction"
- is the shopify guide open source?

"It usually means I need to spend more time cleaning up"
- feedback from people after policial roll out

Using rubycop autocorrect
Use a bot to autocorrect style violations in PRs

Need to have a place for discussion about style
Only follow the style conventions that make sense for your codebase

## Ruby Internals

- cadena == file ?

Tokenizer
- ingests file letter by letter
- identifies ruby keywords "Lexical Analysis"
- at this point it's not seeing if the code can run; just seeing if the file is valid in terms of the language

Finite Automates
- Lex -- can indicate a set of rules and then analyze the lexicon
- Ruby has its own Lexical Analyzer
- Why? b/c ruby syntax 

parse.y has the ruby lexer

Within ruby there's a program called Ripper
- this does a lexical analysis over ruby's analysis

After the Lexer, Ruby has to Parse
- now that we have tokens, need to group those tokens in a way that ruby can understand

Ruby uses a library called Bison for parsing
- Bison is a LALR parser (left to right)

After parsing, create AST (abstract syntax tree)
- AST is almost the endpoint to execute this code

Next is compiling
- YARV == ruby's vm

How does the code execute?
- YARV is a stack VM
- Stack is FILO
- push values to the memory stack and then pop them off the stack to run the code

insns.def has the YARV instructions

## Native Extensions in Helix (?)
@Hone02
- Discourse -- look this up
- String#blank? is slow; hotspot for discourse
- ^^ somebody wrote a faster blank in C

Rust
- like C -- compiled, statically typed, fast
- mem safety guarantee of Ruby -- if it compiles, it won't segfault

Zero-cost abstractions
- in ruby -- tension between abstractions and performance
- hash.keys.each vs hash.each_key (each_key is faster)
- array.shuffle.first vs array.sample (sample is faster)
- In rust, compiler optimizes for you

Helix Rails App
- helix-rails gem w/ generator
- use rust from inside of Rails by adding it to the Gemfile as a gem but designate the path
- for deployments, need to make sure rust can be compiled on target machine
	- heroku makes this easy but what about others?

* text_transform gem
usehelix.com

## Rails Panel
- Common theme in the rails core contributors -- all found bugs in an external library and fixed it
- Find bugs in rails by having a small personal project based off the edge branch of rails -- use master
- Add to the tests
- sprockets are going away in rails 6

## Smashing Dense Graphs
- particular entity where you have diff connections between diff nodes
- should look up graph dbs

- Matrix can represent a graph effectively but can't scale b/c it grows quadratically
- lazy evaluation of dense graph
	- recurse to find the adjacent nodes of the nodes you care about?
	- filter the nodes w/ a simple predicate

- niveles -- levels
- medir -- measure? "to size"

- Need to look up graphs and graph dbs
- Look up his reference slides

## Building the new Rails System Test
@eileencodes

TDD is DEAD (DHH in 2014 rails conf)
- need to supplement tdd with full system tests

rails 5 includes capybara testing
- has system testing without configuration!!! it just works out of the box
- rails 5.1 includes capybara and selenium-webdriver

What's a system test?
- test the app as a whole entity; test how the pieces work together
- allows you to test your js and how it interacts with MVC

application_system_test_case.rb
- any sys test you write will inherit from ^^ file

`rails test:system` -- doesn't run this with the whole suite; most peeps run in a separate ci build

Why did this take so long
- needed to inherit from integration tests
- but integration tests were really slow

Beginner's mindset w/ Capybara
- got to experience the difficulties setting up capybara in rails first hand without bias

Guiding Principles
- use the rails doctrine
- maximize programmer happiness

Config Defaults
- Why Selenium for the driver?
	- default driver for capybara is rack test
	- rack test can't test js and can't take screenshots
		- selenium automatically takes screenshots when tests fail
	- can watch selenium run in your browser
		- poltergeist and rack test don't have the gui
	- better for beginners learning rails and capybara b/c you can watch it fail
	- but can change the default driver
		- driven_by :selenium --> driven_by :poltergeist

Why Chrome
- b/c it's widely used and has higher market share than firefox
- for a while firefox was broken w/ selenium
- driven_by :selenium, using: :firefox
- using arg is only used by selenium b/c others have no gui

Driver Options
- screen size
- options: { url: "some url" }

Database Cleaner is not required for system tests
- in past, txns didn't work w/ capybara tests b/c there were two threads -- one w/ the txn and one talking to the browser; browser was making updates, so they'd never rollback b/c weren't on the thread w/ the txn

Building Open Source Features
- public work -- lot of opinions; feels like everybody is judging you
- public debate is part of open source and the discussion is important
"Consensus is the enemy of vision"
- can merge something imperfect; because community can help you fix the probs
- you're building a foundation for others to work off of


## GC Talk

"Copy on Write" & "Compaction"

"Copy on Write" / CoW Optimizations
- we don't copy something until it gets written to

strings are CoW...
- if you copy, will not use the full memory
- once you change it, full mem will be used
arrays are CoW
hashes are not... will copy all the mem

When you `fork` a process, it's a copy of that process and the copy is your child

CoW Page Fault -- copy that page to the parent-child process

Why is CoW important?
- Unicorn is a *forking* webserver
- have a parent process w/ a whole bunch of child processes that serve the request

What causes page faults?
- Mutating shared memory
- Oftentimes, the Garbage Collector is what mutates shared memory

How does GC impact CoW?

1 - Marking phase
- Mark phase of GC would mark those objects and end up copying them
- OS copies an entire page
- When we mark one object, an entire page worth of object gets copied

Bitmap Marking (optimization)
- when we set the markbit, it marks the process in a table rather than marking the object itself

2- Object Generation
- each tie GC occurs, the object gets older
- store the age of the object on the object itself; means that each object gets copied
- workaround for ^^ is NakayoshiFork gem (look this up)
	- generations are bounded; once it gets to a certain age we stop

- object created before `fork` will get old
- can GC a few times before forking a new project

3- Object Allocation
- writing one object means that we duplicate memory to the child processes
- how can we reduce the free space?

GC Compaction
- compact before fork

GC + CoW -- How can we make GC more CoW friendly
- Bitmap Marking
- Generation Friendly Forking (Nakayoshi Fork)
X Heap Compaction -- haven't talked about this yet

What is compaction?
Why do this? 
- Reduce memory usage
- Lot of people say that this is an "impossible" to do

Compaction Algorithms
- Two Finger Compaction
- Disadvantages
	- slow
	- objects move to random places throughout mem (which may slow down the program?)
- Advantages
	- it's easy!

Two Steps in this algo
1- Object Movement
- free pointer moves to the right until it finds an empty slot, scan pointer moves to the left until it finds a full spot
- once the fingers meet, we have everything grouped together
- Need to update the reference though
- Change all the forwarding addresses back to free spots

Garbage collector lives in gc.c
- `gc_move` - swaps things and leaves forwarding address
- makes a `T_MOVED` object (hidden to user)
- `gc_compact_heap` - moves the fingers closer together
- `gc_update_object_references` - have to have a diff implementation for all the diff ruby obj types

- hash points to keys and values differently than an object points to ivars

- `pinned_bits[];`
- `rb_gc_mark` if something is marked this way, then it pins obj to pinned bits table so it doesn't get moved

Movement Problems
- objects marked with `rb_gc_mark`
- Hash Tables
- dual reference objects
- objects created with `rb_define_class` (global?)
- string literals

Improve Memory
ObjectSpace.dump_all
- dumps evrything in mem to json file

Use `ObjectSpace` to debug or inspect ruby level mem issues
Use /proc/{PIDs}/maps (?)











