# node.js #

## Overview ##

The [official homepage](http://nodejs.org/) explains `node.js` as

> Evented I/O for V8 JavaScript. 

[V8](http://code.google.com/p/v8/) is a JavaScript Engine by Google. It does a very good Job of increasing JavaScripts performance by compiling it into machine code. V8 itself is written in C++ and so are most parts of node.js. 

[Ryan Dahl](http://tinyclouds.org/) saw the value in using JavaScript as a server side scripting language. He is very opinionated on how I/O should be done and node.js is his answer to the problems developers are faced when dealing with I/O.

Currently most I/O code is blocking - meaning that a code block waits for some I/O to work finish before further execution.

	var result = db.query("select ...")

This wastes CPU cycles. Normally developers deal with this problem by writing multithreaded programs, so that other threads can take over, when one thread has to wait. Threads come with an overhead. Each thread costs memory and the context-switching between them can be hard and time consuming. Projects like [nginx](http://nginx.org/), [Lighttp](http://www.lighttpd.net/), [Cherokee](http://www.cherokee-project.com/) are examples that the approach of a single-treaded event loop do work quite well.

For Ryan Dahl _"threaded concurrency is a leaky abstraction"_. His proposed solution is using an event loop and using non blocking I/O all the way down. JavaScript is a good candidate for this abstraction layer as its language and API specification have no notion of binary data or I/O and offers some nice language features such as closures. In fact JavaScript was designed for using an event loop. On the user interface level you have events such as `onClick` to which you can bind a callback function. The culture of JavaScript is already geared towards evented programming.

`node.js` provides a _purely evented_, _non blocking infrastructure_ to script _highly concurrent_ programs. It's design goals are

- everything is asynchronous. The base environment has been built essentially from scratch. It's hard to make blocking code.
- HTTP and sockets are first class citizens. The example Hello World is over HTTP. Node keeps you focused on on dealing with the data, rather than spending all your time dealing with the sockets or protocols.
- API is both familiar to client side JS developers and old school UNIX hackers

Don't underestimate the last point. [Github](https://github.com/), a source code repository lists JavaScript as the most used language with [19% of all committed code](https://github.com/languages) being JavaScript.

> The overarching point, however, is that Node is optimal for the new breed of real-time web apps. Yes, there are other means of building real-time tools, but Node lets you build real-time tools on the same platform that runs the rest of your site.
-- Cade Metz

## Core Concepts ##

### Modules ###

`node.js` supports the [CommonJS](http://www.commonjs.org/) [module specification](http://www.commonjs.org/specs/modules/1.0/).

The module system helps to structure your program into different files. For instance `main.js` could require the `hello` module

	var hello = require('./hello');
	hello.world(); 	

`require('./hello');` imports contents from another JavaScript file. The initial '`./`' indicates that the file is located in the same directory 'main.js'. Also note that you don't have to provide the file extension, as '`.js`' is assumed by default.

The implementation of `hello.js` could look like this:

	exports.world = function() {
	  console.log('Hello World');
	} 

We are assigning a property called '`world`' to an object called '`exports`'. Such an '`exports`' object is available in every module, and it is returned whenever the require function is used to include the module.

If you don't supply a relative path, as in

	var http = require('http');

node.js will first try to load a core module named `http`. If it can't find a core module with the supplied name node.js will start searching for a directory called `node_modules` starting in the directory of the current file and will try to load the module from there. If the directory doesn't exists it walk up the directory tree searching for the `node_modules` directory and the module. If the root directory is reached and no module has been found, node.js will give up and throw a exception.

### The event loop ###

The event loop is the system that JavaScript uses to deal with these incoming request from various parts of the system in a sane manner. JavaScript takes a simple approach that makes the process much more understandable but does introduce a few constraints.

On the server there isn't a user to drive a variety of interactions. Instead we have a whole range of reactions to take on many different kinds of events. Node takes the approach that all I/O activities should be non-blocking. This means that all HTTP requests, database queries, file I/O, etc. do not halt execution until they return, instead they run independently and then emit an event when the data is available.

In every day life we are used to having all sorts of internal callbacks for dealing with events, and yet, like JavaScript, we only ever do one thing at once. JavaScript uses a single-threaded concept to deal with events.

[1](http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/)
[2](http://ofps.oreilly.com/titles/9781449398583/ch03.html)

### Event API ###

If you used JavaScript before you probably came across events. The events in the browser are normally originating from the DOM. In the DOM we have a user-driven event model which is based on user interaction with a set of interface elements arranged in a tree structure (HTML, XML, etc). This means when a user interacts with a particular part of the interface there is an event and a context. That context has a parent and potentially children. Since the context (the HTML/XML element interacted with) is within a tree we get the concepts of bubbling and capturing. This allows elements either up or down the tree to receive the event that was called. For example, if I have an HTML list then a click event on an `<li>` can be captured by a listening on the `<ul>`. Conversely a click on the `<ul>` can be bubbled to a listener on the `<li>`. Since JavaScript objects don't have this kind of tree structure the model in Node is much simpler.

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

A simple HTTP server:

	var http = require('http');
	var server = http.createServer();
	var handleReq = function(req,res){
	  res.writeHead(200, {});
	  res.end('hello world');
	};
	server.on('request', handleReq);
	server.listen(8125);

Making outgoing requests. A simple HTTP client:

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
	
You can even make this code shorter by dropping the `method: 'GET'` property and removing the `req.end();` line because `node.js` implies the use of `GET`.

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

Many components in Node provide continuous output or can process continuous input. To make these components act in a consistent way the stream API provides an abstract interface for them. This API provides common methods and properties that are available in specific implementation of streams. Streams can be readable, writable or both. All streams are EventEmitter instances, this allows them to emit events.

	var fs = require('fs');
	fs.readFile('warandpeace.txt', function(e, data) {
	  console.log('War and Peace: ' + data);
	  fs.unlink('warandpeace.txt');
	});

The basic stream in the example simply reads data from a file in chunks. Every time a new chunk is made available it is exposed to a callback in the variable called data. In this example we simply log the data to the console.

JavaScript can't natively deal with binary data. Node.js introduces `Buffers` to compensate for that. 

	> new Buffer(10);
	<Buffer e1 43 17 05 01 00 00 00 41 90>
	>

`Buffer` allocates memory and leaves it uninitialized which does mean that it might be initialized with dirty bytes.

Writing strings to a buffer means that if you to cope with encodings. When creating a buffer with a string, it defaults to `UTF-8`

	> new Buffer('é');
	<Buffer c3 a9>

You can specify other encoding, but that does mean that characters might get truncated to one bit

	> new Buffer('é', 'ascii');
	<Buffer e9>
	
Although JavaScript supports strings as primitives, you would normally write them to a `Buffer` in nodes. You can partially write into a buffer.

	> var b = new Buffer(1);
	> b
	<Buffer 00>
	> b.write('a');
	1
	> b
	<Buffer 61>
	> b.write('é');
	0
	> b
	<Buffer 61>
	>

The `write` method will return the number of written bytes. If characters won't fit (byte wise) no character will be written. Be careful when you write with an offset though as it will insert the `null` character as a terminator.

	> var b = new Buffer(5);
	> b.write('fffff');
	5
	> b
	<Buffer 66 66 66 66 66>
	> b.write('ab', 1);
	2
	> b
	<Buffer 66 61 62 00 66>
	>

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

I built my [own hello world extension](https://github.com/oschrenk/node-hello-extension) using the [Writing Node.js Native Extensions](https://www.cloudkick.com/blog/2010/aug/23/writing-nodejs-native-extensions/) blog post.

### Debugging ###

You can set breakpoints in your script by adding `debugger;` statements;

	function foo() {
		debugger;
		return 1+2;
	}
	
You can start debugger by running `node debug script.js` and you get a debugging prompt. Its like `gdb` - try: 

	debug> backtrace
	debug> list
	debug> quit

V8 offers access to its debugger. The primitive `gdb` style debugger is only way to access it. [node-inspector](https://github.com/dannycoates/node-inspector) offers a web inspector much like the Inspector from Chrome.

## Problems ##

- spaghetti code
- usage of single thread on multi core machines

### Debugging ###

When executing the following code

	function foo() {
		console.log("foo");
		setTimeout(bar, 1000);
	}
	function bar() {
		console.log("world");
		throw new Error("hit a peoblem")
	}
	setTimeout(foo, 2000);
	console.log("hello");

an error will be thrown after 3 seconds. Normally you would have a big stacktrace which you can follow backwards in order to find your bug. Now, in node.js we have a rather small stack traces. 

In the above example we will only get the stacktrace from `bar()`. Every time we return the event loop we will loose the current stack. That makes analyzing problems hard.

There are [plans/ways around it](nodejs.org/illuminati0.pdf).

## Appendix ##

### Compilation ###

	git clone https://github.com/joyent/node.git
	cd node
	export JOBS=2 # optional, sets number of parallel commands.
	mkdir ~/local
	./configure --prefix=$HOME/local/node
	make
	make install
	export PATH=$HOME/local/node/bin:$PATH

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

### Creating a module ###

[Source]([1](http://howtonode.org/how-to-module)

Follow good engineering standards: 

- use a version control system. You should prefer `git` as most of the node community uses it.
- write a proper `README` and documentation
- write tests

Also:

- write a `package.json` file

#### Modules Types ####

- A binding to some C or C++ library.
- A library of functionality to be used in other node programs, written primarily in JavaScript.
- A command-line program.
- A website or server or something. (That is, an implementation that you'll put on an actual server, not a framework.)

##### Binding #####

- put your C++ sourcecode in `./src`.
- put minimum effort into the c++ layer, prefer wrapping the raw functions again in your JavaScript code to make them more usable.
- node.js programs are compiled using `node-waf` (included with node). Create a `wscript` file and run `node-waf configure build`
-  use `eio` thread pool to do any actions that perform synchronous I/O

Examples:

- [https://github.com/pkrumins/node-async](https://github.com/pkrumins/node-async)
- [https://github.com/isaacs/node-async-simple](https://github.com/isaacs/node-async-simple)
- [https://github.com/pkrumins/node-png](https://github.com/pkrumins/node-png)
- [https://github.com/mranney/node_pcap](https://github.com/mranney/node_pcap)
- [https://github.com/ry/node_postgres](https://github.com/ry/node_postgres)
- [https://github.com/isaacs/node-glob](https://github.com/isaacs/node-glob)

##### Library #####

- organize your own code by using `./lib` directory
- specify `main` module in your `package.json` to expose functionality

Examples:

- [https://github.com/learnboost/Socket.IO-node](https://github.com/learnboost/Socket.IO-node)
- [http://documentcloud.github.com/underscore](http://documentcloud.github.com/underscore)
- [https://github.com/visionmedia/connect](https://github.com/visionmedia/connect)
- [https://github.com/mikeal/request](https://github.com/mikeal/request)
- [http://github.com/tmpvar/jsdom](http://github.com/tmpvar/jsdom)

##### Command Line Tool #####

- define `bin` field in your `package.json` telling `npm` which executables should be in your `PATH`
- `process.argv` is the string array of arguments that represents the command the user started node with. The first item is your executing environment (normally node), the second is the name of your program. To get your arguments you would normally call `var args = process.argv.slice(2)`
- current working directory can be obtained via `process.cwd()`

Examples:

- [http://npmjs.org/](http://npmjs.org/)
- [https://github.com/zpoley/json-command](https://github.com/zpoley/json-command)
- [https://github.com/cloudhead/vows](https://github.com/cloudhead/vows)
- [https://github.com/LearnBoost/cluster](https://github.com/LearnBoost/cluster)

##### Standalone Server #####

This would be what you would normally would program. You just want to use modules. 

- avoid re-inventing modules. [Search](http://search.npmjs.org/) for existing solutions and try to improve them

## Projects/Companies using node.js ##

- [Github](https://github.com/blog/678-meet-nodeload-the-new-download-server) Archive downloads
- [Etsy](http://www.etsy.com/) Website/Application status analysis with UDP
- [transloadit](http://transloadit.com/) Uploading and processing in one step.

## Quotes ##

> Windows is very important. Just like PHP.
-- Ryan Dahl  

> Node [is] very bleeding edge. Looks kinda of snappy and cool here but wait until you get into production - right in the gut. -- Ryan Dahl

> The overarching point, however, is that Node is optimal for the new breed of real-time web apps. Yes, there are other means of building real-time tools, but Node lets you build real-time tools on the same platform that runs the rest of your site.
-- Cade Metz

## References ##

### Videos ###

- [Ryan Dahl, Google Tech Talks, July 2010, Node.js: JavaScript on the Server](http://www.youtube.com/watch?v=F6k8lTrAE2g)
- [Ryan Dahl, jsconf.eu, September 2010, node.js](http://jsconfeu.blip.tv/file/2899135/)
- [Ryan Dahl, The SF PHP Meetup Group, February 2011, Introduction To node.js](http://www.youtube.com/watch?v=jo_B4LTHi3I)
- [Felix Geissendörfer, 27C3, node.js as a networking tool](http://www.youtube.com/watch?v=g29PemqW7lQ)

### Guides/Books ###

- [Felix Geissendörfer, nodeguide](http://nodeguide.com)
- [Up and Running with Node.js](http://ofps.oreilly.com/titles/9781449398583/), Upcoming book about node.js by Tom Hughes-Croucher
- [Mastering node.js, Open Source eBook](http://visionmedia.github.com/masteringnode/), Public book by TJ Holowaychuk
- [Felix Geissendörfer, node.js Styldeguide](http://nodeguide.com/style.html)

### Blogs ###

- [How to Node](http://howtonode.org/), Blog about node.js by Tim Caswell
- [nodejitsu](http://blog.nodejitsu.com/), Blog from a node.js hosting company
- [nodenerd.net](http://nodenerd.net/) node.js links and tutorials

### Other ###

- [Official node.js mailing list](http://groups.google.com/group/nodejs)
- [Tim Caswell: node.js at jsconf 2010](http://creationix.com/jsconf.pdf), Presentation (PDF slides)