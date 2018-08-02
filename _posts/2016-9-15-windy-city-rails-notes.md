---
layout: post
title:  "Windy City Rails Notes"
date:   2016-9-15
categories: conferences
---

## When webscale doesn't make sense


## Building a Better Open Struct

* ruby's javascript object
* dot and bracket notation

### Why us Open Struct

* Consume an API
	* subclass open struct
	```ruby
	class ApiCall < OpenStruct
		define your own api methods!
		make a wrapper around your response
	```
* Configuration Object
* Test Double

### How Openstruct works

* OpenStruct defines attr setters/getters on the object's singleton class
thinking of singleton as class that's just for 1 obj
`#each_pair`
`new_ostruct_member(k)`
* uses method_missing to allow for a setting w/o it being defined
* no 2 openstructs share the same methods
* 10-40 times slower than a regularly defined class
	* defining methods in ruby is slow
	* method missing is slow
	* in ruby 2.1 and up, open structs does not invalidate the global method cache

### Improvements to open struct

#### OpenFastStruct

* Hash#fetch
* doesn't pay the upfront cost of defining a method
* doesn't call new ostruct

#### PersistentOpenStruct

* defines methods on class instead of singleton methods
* good when creating objs w/ the same attrs

#### Dynamic Class

like persistent os but diff 


## Why TDD is Crap

Design by Contract (Eiffel)
Property Based Testing (Haskell)
Hammock Driven Development (Clojure)
Actual QA (Every organization ever)


## Layered Security

* design specific controls that solve specific problems
* process things at the right time and place
* 'locality of reference'

Layers w/r/t rails
Edge
* CDN
* Load Balancer
* Web Server
App
* App -- Rack & Active/ACtion
Data
* DB

Should do most security work @ the edge
* need SSL or TLS running
* need @ least B+ rating on SSL Labs
* reject extensions you don't want to accept (ie reject php requests, etc)
* reject known bad user agents -- lists exist out there
* reject know bad actors
* customize all the error pages
* basic secure headers -- force https

Can have dynamic controls @ the edge
* authentication caching -- authenticate @ the edge
* availability is a security problem (??)
* firewalls -- speaker doesn't like b/c finnicky & require maint
* load shedding / autobalance / autoscaling -- ways to shed a lot of traffic (DDOS attacks)
	* have to do this at the edge b/c if hits your app, it's game over
* Repsheet -- speaker's OS project -- look up this

Rack Controls
* Rack Attack -- whitelists, throttling, blacklists
* Rack Honeypot
* Rack DetectTo/Rack Tor Block -- should block Tor
* Warden (auth)
* Rack Throttle (he recommends handling throttling at the edge) - EngineX and CDNs better
* Rack Cylon -- for indexing / crawling
* write your own custom middleware

Rails Controls
* secure headers
* lotta built in stuff
* authorization -- should do this at the app level
* encryption -- can be elsewhere but can be in the app
* domain specific stuff -- fraud, etc
	* what would MAC be specifically vulnerable for
* secure dev lifecycle

Use Brakeman -- tool that looks for security flaws
Bundler Audit -- detect security issues w/ gems

Database Level
* never use the root user (except for setting up the other users)
* strongly and securely stored prod passwords
* separate users for runtime & migrations
* separate dbs for prod, staging, tests
* Firewalls -- db should never be open for the internet
	* only allow the apps that need access to have access
* Logs -- maintain the logs
* Back up your db

LOOK AT THIS DUDE'S RESOURCE LINKS
And look him up on twitter, etc



## TCP & SOCKETS
@sebasoga

* Line protocol

### What's a socket?

* sockets speak to other sockets using standard unix file descriptors
* everything (in unix) is a file
* sockets act like files
* Ruby has TCPSocket class

stuff you can do w/ the TCPSocket class
* create an endpoint for comm
* assign to a socket to an addy
* prepare a socket for listening
* connect to a socket in the given addy
* manage new incoming connections
* send a stream to the connected socket
* rcv incoming stream
* perform syncrhonous I/O multiplexing (!!! look this shit up)
* end connection w/ peer socket

Berkley Socket API

Client/Server Lifecycle w/ sockets
* initiater socket (client)
* server socket
`man 2 socket`

socket(2)
bind(2)
listen(2)

### 3 Way Handshake
1. SYN packet from socket to server
2. server responds w/ ACK packet
3. client sends its ACK packet to server

```
require 'socket'

# create new TCP socket
socket = Socket.new(:INET, :STREAM, 0)
# first arg could also be :UNIX, other stuff
# second arg could also be :DRAM

# Create a C struct to hold the addy for listning
addy = Socket.pack_sockaddr_in(5000, '127.0.0.1')
socket.bind(address)

# Listen for incoming connections
socket.listen(Socket::SOMAXCONN)
client.close
```

## Negotiating
@AshleyPQPQP

* Try to garner multiple job offers
* It's a numbers game
* don't 'need' a job
* Know your worth
* Beware of ranges

* Non-monetary wiggle room is not for the 1st round of negotiations
"That's a good offer. What more can you do?"
"I was really hoping for something more like X. Can you get close?"
"Do you have some room to negotiate?"


## Docker Gotchas

* Docker and UFW (uncomplicated firewall) don't play nice
* container ip addy in docker is dynamic -- not persistent -- can't rely on it to be the same
* reference containers by name
* OOM errors w/ docker -- need to specify mem restrictions, define restart policy for containers that crash
* WTF is a 'dangling image' w/r/t docker


## Mustache

* Look up mustache -- front end templating language
* only knows string, boolean, array, hashes
* keeps logic out of templates


## Rails for NonProfs

* Look into Digital Ocean for hosting (do they host fo free for non-profs?)
* Rocketjob -- sidekiq alternative


## Dash for Datasets
* can deposit data into a data repo?



## Progressive Enhancement -- does it have a place in web dev?

* What is progressive enhancement -- sandwich example
'minimum viable sandwich'
* Start with HTML -- do as much w/ that HTML as you can
* Add more ingredients -- CSS
* Building in a layered way -- starting at the simplest and then adding add'l ingredients w/o replacing the core ones

### Single page web app is the alternative to the sandwich progressive enhancement web dev approach
* more like a loaf of bread
* all pieces must exist / load / are required

### Diff b/t the two approaches...
* complexity!

Simplified overview of web request being rendered in browser
parser -> object model -> renderer -> GUI

Two types of complexity
* Inherent complexity is okay -- the complexity that comes with the problem you're trying to solve (i.e. filling out a tax application)
* Incidental complexity -- can really ruin your day
	* when the problem that's being solved doesn't justify the code that's being written to solve it
	* makes things take longer to ship
	* costs time, money, responsiveness to users
	* can hide bugs :0

With progressive enhancement, you can choose WHERE and WHEN to increase complexity


## From OO to Functional
@rusilko

division b/t data and behavior
composable functions
operating on immutable data

## The Human Utility -- Rails and Water
github.com/humanutility

* Human Utility ensures low income families who need help paying water bill can get running water
* Saved 40+ homes from being foreclosed due to water bill being missed
	* if you don't pay your water bill, it gets tacked onto your taxes and city can sell the house

FOR MAC
* Look into the griddler gem (hooked up through mandrill) -- to display email discussion within the app
	* can I use this with gmail -- google apps?
	* can use mailgun as the processor!
* Look into the refile gem.... could there be a use for this w/ MAC? YES -- REIMBURSEMENTS
	* allow volunteers to text pics of receipts
* Look into ActionCable

* Twillio
* Add Paulo to MAC repo











