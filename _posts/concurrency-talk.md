---
layout: post
title:  Concurrency in Ruby
date:   2015-06-30 22:43:00
categories: rubyconf concurrency
---

concurrent-ruby gem

GIL = Global Interpreter Lock
GVL - Global Virtual Machine Lock

serial vs concurrent
jruby does concurrency

concurrency vs parallelism
TL;DR --> concurrency != parallelism

C -- programming as the composition of independently executing processes
- dealing w/ lots of things at one
- can have one processor core
- concurrent programs get parallelism for free when the runtime supports it

P - the simultaneous execution of (possibly related) computations
- doing lots of things at once
- requires two processor cores
- a processor core can only execute on instruction at a time

GIL
loack in CS is a synchronization mechanism which ensures that only one thread can access a protected resource at any given time
a thread needing to access a protected resource must request the lock

What's a thread
* smallest sequence of programmed instructions that can be managed independency by a scheduler
* usually an OS will have way more threads running than there are things to do

Always need a scheduler

Ruby and java map language constructs directly to os threads
Erland and Go place an abstraction over native threads and provide their own scheduler
Regardless still need to manage OS threads

Each core can only run one thread
new term - "context switch"

no langauge can preempt an OS context switch

every language has internal logic to manage itself across OS context switches and across its own concurrency constructs

Ruby uses the GIL to protect internal state across OS context switches

Thread A starts doing some work -- ruby locks the GIL
context switch and thread B runs
- thread b tries to get the GIL but can't
- thread A resumes, finishes work and unlocks the GIL
- now thread B can have the GIL

only one unit of Ruby code can be executed at any given time

ruby is a shared memory language
every var is a reference 
- the var itself is a memory address
- the data is stored at the references memory location
- two vars may ref the same mem addy
2 thread can share data by simultaneously accessing the same mem location

read update error isn't thread safe

ruby is compiled to bytecode within the interpreter
ruby is free to optimize and reorder your code
every ruby operation is implemented in C
ruby runtime is just another program; it is under the control of the complier and the OS
- the C compiler is free to optimize and reorder instructions during compilations
- an OS context switch can occure at any point in the running C code
GIL protects Ruby -- not your code

Ruby is thread safe -- but no guarantees that your code is thread safe
- GIL preventes interleaved access to mem used by the runtime
- GIL prevents interleaved access to individual vars

Memory Model
- models threads through memory and their shared use of the data
- defines visibility, volatility, atomicity, and synchronization barriers
- some languages have a documented memory model (Java, C, C++ all have)
- Ruby has no documented memory model... 
- the GIL provides an implied memory model but no guarantees

Sometimes your program does a lot by doing nothing at all

I/O
- modern computers support both blocking and asynch (non-blocking) I/O
I/O in ruby programs is BLOCKING
I/O w/in ruby is ASYNCH

All ruby IO calls unlock the GIL, as do backticks and system calls
When ruby thread is wating on IO it doesn't block other threads from doing their stuff

'you can't spell GIL w/o IO'

IO operations are slow, which is why asynch IO was invented
GIL protects ruby runtime

What are IO actions?
read/write from files
interact w/ dbs
listen for inbounch network requests (HTTP)
connect to external HHTP APIs
send email

- often concurrency is managed by frameworks (Rails does this)
- many domains requiring highly concurrent code also needs high performance languages

ruby is good at concurrent IO; not so much for processor intensive operations
GIL prevents full parallelism
- the OS will still multiplex across multiple (threads?)

concurrency is about design

Ruby's concurrency tools:  Thread, Fiber, Mutex, ConditionVariable
`future` construct is how presenter coded concurrency in his example (perhaps that was from concurrent ruby gem?)
