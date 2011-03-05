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

## The event loop ##

The event loop is the system that JavaScript uses to deal with these incoming request from various parts of the system in a sane manner. JavaScript takes a simple approach that makes the process much more understandable but does introduce a few constraints.

On the server there isn't a user to drive a variety of interactions. Instead we have a whole range of reactions to take on many different kinds of events. Node takes the approach that all I/O activities should be non-blocking (for reasons we'll explain more later). This means that all HTTP requests, database queries, file I/O, etc. do not halt execution until they return, instead they run independantly and then emit an event when the data is available.

In every day life we are used to having all sorts of internal callbacks for dealing with events, and yet, like JavaScript, we only ever do one thing at once. JavaScript uses a single-threaded concept to deal with events.

[1](http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/)
[2](http://ofps.oreilly.com/titles/9781449398583/ch03.html)

## Usage ##

### Extending node.js ###

https://www.cloudkick.com/blog/2010/aug/23/writing-nodejs-native-extensions/
https://github.com/pquerna/node-extension-examples
https://github.com/rbranson/node-ffi

### Fun with node.js ###

- [talk to serial devices](https://github.com/voodootikigod/node-serialport)
- [talk to usb devices](https://github.com/schakko/node-usb)
- [talk to arduino](https://github.com/tobeytailor/node-arduino)	

## Appendix ##

## Installation ##

### OSX ###

	brew install node
	# Caveats
	# Please add /usr/local/lib/node to your NODE_PATH environment variable to have node libraries picked up.

### Open Suse ###

	sudo zypper ar http://download.opensuse.org/repositories/home:/SannisDev/openSUSE_11.3/ SannisDevBuildService 
	sudo zypper in nodejs nodejs-devel

### Ubuntu ###

	sudo apt-get install python-software-properties
	sudo add-apt-repository ppa:jerome-etienne/neoip
	sudo apt-get update
	sudo apt-get install nodejs

### Frameworks ###

- [socket.io](http://socket.io/)  An abstraction over all the realtime transports (WebSocket, long polling, XHR multipart, etc) for the Node HTTP server
- [Connect](http://senchalabs.github.com/connect/) Connect is a middleware framework for node, shipping with over 11 bundled middleware and a rich choice of 3rd-party middleware.
- [mongooose](http://mongoosejs.com/) Mongoose is a MongoDB object modeling tool designed to work in an asychronous environment.

### Projects/Companies using node.js ###

- [Github](https://github.com/blog/678-meet-nodeload-the-new-download-server) Archive downloads
- [Etsy](http://www.etsy.com/) Website/Application status analysis with UDP
- [transloadit/](http://transloadit.com/) Uploading and processing in one step.

## References ##

- [Ryan Dahl: node.js at jsconf.eu 2010](http://jsconfeu.blip.tv/file/2899135/)
- [Felix Geissendörfer, 27C3, node.js as a networking tool](http://www.youtube.com/watch?v=g29PemqW7lQ)
- [Tim Caswell: node.js at jsconf 2010](http://creationix.com/jsconf.pdf)
- [What it’s like building a real website in Node.js](http://www.travisglines.com/web-coding/what-its-like-building-a-real-website-in-node-js)
- [How to Node](http://howtonode.org/), Blog about node.js by [Tim Caswell](http://creationix.com/)
- [Official node.js mailing list](http://groups.google.com/group/nodejs)
