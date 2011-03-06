# node.js #

## Overview ##

The [officicial homepage](http://nodejs.org/) explains `node.js` as

> Evented I/O for V8 JavaScript. 

[V8](http://code.google.com/p/v8/) is a JavaScript Engine by Google. It does a very good Job of increasing JavaScripts performance by compiling it into machine code. V8 itself is written in C++ and so are most parts of node.js.  

Ryan Dahl saw the value in using JavaScript as a server side scripting language. He is very opnionated on how I/O should be done and node.js is his answer to the problems developers are faced when dealing with I/O.

Currently most I/O code is blocking - meaning that a code block waits for some I/O to work finish before further execution.

	var result = db.query("select ...")

This wastes CPU cycles. We deal with this problem by writing multitreaded programs, so that other threads can take over, when one thread has to wait. Threads come with an overhead. Each thread costs memory and the context-switching between can be hard and time consuming. 

For Ryan Dahl _"threaded concurrency is a leaky abstraction"_. His propsosed solution is using an event loop and using non blocking I/O all the way done. JavaScript is a good candidate for this abstraction layer as its language and API specification has no notion of binary data and has some nice language features, such as closures. In fact JavaScript was designed for using an event loop. On the user interface level you have events such as `onClick` to which you can bind a callback function. The culture of JavaScript is already geared towards evented programming.

	var http = require('http');
	http.createServer(function (req, res) {
	  res.writeHead(200, {'Content-Type': 'text/plain'});
	  res.end('Hello World\n');
	}).listen(8124, "127.0.0.1");
	console.log('Server running at http://127.0.0.1:8124/');

`node.js` provides a _purely evented_, _non blocking infrastructure_ to script _highly concurrent_ programs. It's design goals are

- everything is asynchronous. The base environment has been built essentially from scratch. It's hard to make blocking code.
- HTTP and sockets are first class citizens. The example Hello World is over HTTP. Node keeps you focused on on dealing with the data, rather than spending all your time dealing with the sockets or protocols.
- API is both familar to client side JS developers and old school UNIX hackers

Don't underestimate the last point. [Github](https://github.com/), a source code repository lists JavaScript as the most used language with [19% of all committed code](https://github.com/languages) being JavaScript.

> The overarching point, however, is that Node is optimal for the new breed of real-time web apps. Yes, there are other means of building real-time tools, but Node lets you build real-time tools on the same platform that runs the rest of your site.
-- Cade Metz

## Core Concepts ##

### The event loop ###

The event loop is the system that JavaScript uses to deal with these incoming request from various parts of the system in a sane manner. JavaScript takes a simple approach that makes the process much more understandable but does introduce a few constraints.

On the server there isn't a user to drive a variety of interactions. Instead we have a whole range of reactions to take on many different kinds of events. Node takes the approach that all I/O activities should be non-blocking (for reasons we'll explain more later). This means that all HTTP requests, database queries, file I/O, etc. do not halt execution until they return, instead they run independantly and then emit an event when the data is available.

In every day life we are used to having all sorts of internal callbacks for dealing with events, and yet, like JavaScript, we only ever do one thing at once. JavaScript uses a single-threaded concept to deal with events.

[1](http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/)
[2](http://ofps.oreilly.com/titles/9781449398583/ch03.html)

### Event API ###

If you used JavaScript before you probably cam e across events. The events in the browser are normally originating from the DOM. In the DOM we have a user-driven event model which is based on user interaction with a set of interface elements arranged in a tree structure (HTML, XML, etc). This means when a user interacts with a particular part of the interface there is an event and a context. That context has a parent and potentially children. Since the context (the HTML/XML element interacted with) is within a tree we get the concepts of bubbling and capturing. This allows elements either up or down the tree to receive the event that was called. For example, if I have an HTML list then a click event on an `<li>` can be captured by a listening on the `<ul>`. Conversely a click on the `<ul>` can be bubbled to a listener on the `<li>`. Since JavaScript objects don't have this kind of tree structure the model in Node is much simpler.

All event functionality in Node resolves around `EventEmitter`. The `on` methods creates and event listeners for an event:

	server.on('event', function(a, b, c) {
	  //do things
	});

EventEmitter provides the on method to other classes to use. It would be unusual to call an `EventEmiter` instance directly. The `on` method takes two options: the name of the event to listen for and the function to call when that event is emitted. Since `EventEmitter` is an interface Pseudo-class the class which inherits from `EventEmitter` is expected to be innovated with the new keyword.

	var utils = require('utils');
	var EventEmitter = require('events').EventEmitter;
	var Server = function() {
	  console.log('init');
	};
	utils.inherits(Server, EventEmitter);
	var s = new Server();
	s.on('abc', function() {
	  console.log('abc');
	});

The `utils` module helps us to inherit methods. Note how EventEmitter is capitalized to show it is a Class. Right now however, the event listener we added will never be called because the abc event isn't fired. We can fix this by adding some code to emit the event.

	s.emit('abc');
	// with parameters
	s.emit('abc', a, b, c);

It's important to note that these events are instance based. There are no global events. When you call the on method you attach to a specific EventEmitter based object. Even the various instances of the Server class don't share events. s from the code sample will not share the same events as another Server instance such as `var z = new Server();`.

[1](http://ofps.oreilly.com/titles/9781449398583/ch05.html)

### HTTP ###

A simpe HTTP server:

	var http = require('http');
	var server = http.createServer();
	var handleReq = function(req,res){
	  res.writeHead(200, {});
	  res.end('hello world');
	};
	server.on('request', handleReq);
	server.listen(8125);

Making outoging requests. A simple HTTP client:

	var http = require('http');
	var opts = {
	  host: 'www.google.com'
	  port: 80,
	  path: '/',
	  method: 'GET'
	};
	var req = http.request(opts, function(res) {
	  console.log(res);
	  res.on('data', function(data) {
	    console.log(data);
	  });
	});
	req.end();
	
YOu can even make this code shorter by dropping the `method: 'GET'` property and removing the `req.end();` line because `node.js` implies the use of `GET`.

`POST`ing data:

	var options = {
	  host: 'www.example.com',
	  port: 80,
	  path: '/submit',
	  method: 'POST'
	};
	var req = http.request(options, function(res) {
	  res.setEncoding('utf8');
	  res.on('data', function (chunk) {
	    console.log('BODY: ' + chunk);
	  });
	});
	req.write("my data");
	req.write("more of my data");
	req.end();

[1](http://ofps.oreilly.com/titles/9781449398583/ch05.html)

### I/O ###

Many components in Node provide continuos output or can process continuos input. To make these components act in a consistent way the stream API provides an abstract interface for them. This API provides common methods and properties that are available in specific implementation of streams. Streams can be readable, writable or both. All streams are EventEmitter instances, this allows them to emit events.

	var fs = require('fs');
	fs.readFile('warandpeace.txt', function(e, data) {
	  console.log('War and Peace: ' + data);
	  fs.unlink('warandpeace.txt');
	});

The basic stream in the example simply reads data from a file in chunks. Every time a new chunk is made available it is exposed to a callback in the variable called data. In this example we simply log the data to the console.

[1](http://ofps.oreilly.com/titles/9781449398583/ch05.html)

## Patterns ##

_Pseudo-class factories_. Eg. `http.createServer()` method provides us with a new instance of the HTTP Server class`

_Spooling pattern_. Is used when we need an entire resource available before we deal with it. In this scenario we use a stream to get the data but then only use it when enough is available. Typically this means when the stream ends but it could be another event or condition.

	//abstract stream
	var spool = "";
	stream.on('data', function(data) {
	  spool += data;
	});
	stream.on('end', function() {
	  console.log(spool);
	});

## Usage ##

### C(++) library bindings ###

https://www.cloudkick.com/blog/2010/aug/23/writing-nodejs-native-extensions/
https://github.com/pquerna/node-extension-examples
https://github.com/pkrumins/node-png
https://github.com/pkrumins/node-async
http://howtonode.org/how-to-module

### Task queues ###

### Fun with node.js ###

- [talk to serial devices](https://github.com/voodootikigod/node-serialport)
- [talk to usb devices](https://github.com/schakko/node-usb)
- [talk to arduino](https://github.com/tobeytailor/node-arduino)	

## Appendix ##

### Compilation ###

	curl -O http://nodejs.org/dist/node-v0.4.2.tar.gz
	tar xzf node-v0.4.2.tar.gz
	cd node-v0.4.2
	./configure
	make
	sudo make install

### Installation ###

#### OSX ####

	brew install node
	# Caveats
	# Please add /usr/local/lib/node to your NODE_PATH environment variable to have node libraries picked up.
	
#### Open Suse ####

	sudo zypper ar http://download.opensuse.org/repositories/home:/SannisDev/openSUSE_11.3/ SannisDevBuildService 
	sudo zypper in nodejs nodejs-devel

#### Ubuntu ####

	sudo apt-get install python-software-properties
	sudo add-apt-repository ppa:jerome-etienne/neoip
	sudo apt-get update
	sudo apt-get install nodejs
	
### Node Package Manager ###	

	curl http://npmjs.org/install.sh | sh

### APIs/Frameworks ###

- [socket.io](http://socket.io/)  An abstraction over all the realtime transports (WebSocket, long polling, XHR multipart, etc) for the Node HTTP server
- [Connect](http://senchalabs.github.com/connect/) Connect is a middleware framework for node, shipping with over 11 bundled middleware and a rich choice of 3rd-party middleware.
- [mongooose](http://mongoosejs.com/) Mongoose is a MongoDB object modeling tool designed to work in an asychronous environment.
- [coffee-resque](https://github.com/technoweenie/coffee-resque) Node.js port of Resque
- [node-worker](https://github.com/cramforce/node-worker) An implementation of the WebWorker API for node.js

### Projects/Companies using node.js ###

- [Github](https://github.com/blog/678-meet-nodeload-the-new-download-server) Archive downloads
- [Etsy](http://www.etsy.com/) Website/Application status analysis with UDP
- [transloadit](http://transloadit.com/) Uploading and processing in one step.

## References ##

- [Ryan Dahl: node.js at jsconf.eu 2010](http://jsconfeu.blip.tv/file/2899135/)
- [Felix Geissendörfer, 27C3, node.js as a networking tool](http://www.youtube.com/watch?v=g29PemqW7lQ)
- [Tim Caswell: node.js at jsconf 2010](http://creationix.com/jsconf.pdf)
- [What it’s like building a real website in Node.js](http://www.travisglines.com/web-coding/what-its-like-building-a-real-website-in-node-js)
- [How to Node](http://howtonode.org/), Blog about node.js by Tim Caswell
- [Official node.js mailing list](http://groups.google.com/group/nodejs)
