---
layout: post
title:  Concurrency in Ruby
date:   2015-06-30 22:43:00
categories: rubyconf concurrency
---

This talk was given by Jerry Antonio, writer of [the concurrent-ruby gem](http://www.concurrent-ruby.com), which is used by Rails, among other things.

TL;DR
concurrency != parallelism

###Concurrency vs. Parallelism

Concurrency: the composition of independently executing processes
* dealing with many things at once
* only needs one processor core
* concurrent programs get parallelism for free when the runtime supports it
* concurrency is about design

Paralellism: the simultaneous execution of (possibly related) computations
* doing many things at once
* requires two processor cores
* a processor core can only execute one instruction at a time

###What's the GIL?
GIL = Global Interpreter Lock
GVL = Global Virtual Machine Lock

###What's a lock?
A 'lock' is a synchronization mechanism which ensures that only one thread can access a protected resource at any given time. Think of a resource here as a piece of data persisted in memory. If two threads are both trying to access, and possibly change, the same piece of data located at the same place in memory, the results could be unpredictable. Locking prevents this from happening. A thread needing to access a protected resource must request the lock. 

###What's a thread?
* Smallest sequence of programmed instructions that can be managed independently by a scheduler, which every operating system (OS) has built in.
* Usually an OS will have far more threads running than there are things to do.

Not all languages manage threads the same way. For example, Ruby and Java map language constructs directly to OS threads, while Erlang and Go place an abstraction over native (OS) threads and provide their own scheduler. Regardless of the difference, the language still needs to manage OS threads. Each core can only run one thread.

###Context Switches
* When the OS drops one thread for another
* No langauge can preempt an OS context switch
* Every language has internal logic to manage itself across OS context switches and across its own concurrency constructs.
* Ruby uses the GIL to protect internal state across OS context switches.

###Example of how the GIL handles a context switch
* Thread A starts doing some work
* Ruby locks the GIL
* OS does a context switch, and thread B runs
* Thread B tries to get the GIL but can't (because thread A has it)
* Thread A resumes, finishes work, and unlocks the GIL
* Now thread B can have the GIL

###Memory in Ruby
* Ruby is a shared memory language
* Every variable is a reference 
* The variable itself is a memory address
* Data is stored at the reference's memory location
* Two variables may reference the same memory address
* Two threads can share data by simultaneously accessing the same memory location

###Ruby's concurrency tools
* Thread
* Fiber
* Mutex
* ConditionVariable
* Note: the speaker used `future` in his presentation. This is from the concurrent-ruby gem.
* Often concurrency is managed by frameworks (Rails does this)
* Many domains requiring highly concurrent code also need high performance languages (not Ruby)

###Ruby runtime and the GIL
* Ruby is compiled to bytecode within the interpreter and is free to optimize and reorder your code.
* Every Ruby operation is implemented in C, and only one unit of Ruby code can be executed at any given time.
* Ruby runtime is just another program (see [Aaron Patterson's talk on the Ruby Virtual Machine](LINK)) -- it is under the control of the compiler and the OS.
* The C compiler is free to optimize and reorder instructions during compilations.
* An OS context switch can occure at any point in the running C code
* The GIL prevent interleaved access to memory used by the runtime.
* The GIL prevents interleaved access to individual variables.
* The GIL protects Ruby as a language -- not your Ruby code

###Memory Models
* These exist to model threads through memory and their shared use of data.
* Defines visibility, volatility, atomicity, and synchronization barriers.
* Some languages have a documented memory model (Java, C, C++), but Ruby does not.
* That said, the GIL provides an *implied* memory model, but makes no guarantees.

###'You can't spell GIL without I/O'

Modern computers support both blocking and asynchronous (non-blocking) I/O. IO operations are slow, which is why asynchronous IO was invented.

* I/O in ruby programs is BLOCKING
* I/O within Ruby itself is ASYNCHRONOUS

All ruby I/O calls unlock the GIL, as do backticks and system calls. When a Ruby thread is waiting on I/O it doesn't block other threads from doing their jobs.

####What are I/O actions?
* Read/write from and to files
* Interactions with databases
* Listening for inbound network requests (HTTP)
* Connecting to external HTTP APIs
* Sending email

###Takeaways

Ruby is good at concurrent IO, but not so much for processor intensive operations as the GIL prevents full parallelism. Though the OS will still multiplex across multiple threads.

* Concurrency is not parallelism
* GIL protects Ruby's internal state when the operating system context switches
		* The GIL does not provide thread safety guarantees to user code
		* But it imporses an implicit memory model
* The GIL prevents true parallelism in Ruby
* But Ruby is pretty good at multiplexing threads performing blocking I/O


serial vs concurrent
jruby does concurrency

GIL
lock in CS is a synchronization mechanism which ensures that only one thread can access a protected resource at any given time
a thread needing to access a protected resource must request the lock

Always need a scheduler

read update error isn't thread safe

In other words, Ruby is thread safe, (multiple threads aren't able to simultaneously access the same individual variables or memory addresses during runtime). However, there are no guarantees that your code is thread safe.
- GIL prevents interleaved access to mem used by the runtime
- GIL prevents interleaved access to individual vars


Sometimes your program does a lot by doing nothing at all

GIL protects ruby runtime

`future` construct is how presenter coded concurrency in his example (perhaps that was from concurrent ruby gem?)