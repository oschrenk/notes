# node.js #

Die [offizielle Homepage](http://nodejs.org/) erklärt `node.js` erst einmal sehr knapp mit der Tagline:

> Evented I/O for V8 JavaScript. 

Was heißt das und warum sollte uns das interessieren?

- nodejs ist geschrieben in C
- nutzt CommonJS

Thinking about IO is wrong. You query the database and tan you use the result. In most cases you wait for the database to respond. Better not waste CPU Cycles.

Apache uses much Ram when faced with many requests.
NGINX doesn't.

NGINX uses event loop instead of threads. Context-Switcng between thread (Apache) cosrts time. Each thread costs memory. Threads are not good for concurrency. Event loops are better.

You have to have have non blocking IO.

Threads
Grren-Threads
Co-Routines

Threaded concurrency is a leaky abstraction. 

Code liek this

	var result = db.query("select ...")
	
blocks io. Better write with Callback. All we need is a pointer to the callback function.


_Cultursal Bias_ against non-blocking io. We are taught to demand input. We have learned it that way.

_Missing Infrastructure_  missing presentation

Existing Infratructure. Evermahine, Twisted, AnyEvent. But ruby-mysql blocks.


JavaSript was dsigned for an event loop. onCLick Callback. Annonymoius functions, closures. The culture of JavaScript is already geared towards evented programming.


node.js provides a _purely evented_, _non blocking infrastructure_ to script _highly concurrent_ programs.

Design Goals
- no function should perform i/o. To rceive something from we need callback
- never force the user to buffer data
- support many http features (slide)
- API shpuld be both familar to client side JS programmer and old school UNIX hackers

Examples

Promise is an EventEmiter. Send a request to the disk. Tell me mdoefied date of file. "Success" here is the answwer.	

- node js/long poll hang requests

- was ist v8

http://code.google.com/apis/v8/intro.html

Welcome to the developer documentation for V8. V8 is Google's open source, high performance JavaScript engine. It is written in C++ and is used in Google Chrome, Google's open source browser.

V8 compiles and executes JavaScript source code, handles memory allocation for objects, and garbage collects objects it no longer needs. V8's stop-the-world, generational, accurate garbage collector is one of the keys to V8's performance. You can learn about this and other performance aspects in V8 Design Elements.

JavaScript is most commonly used for client-side scripting in a browser, being used to manipulate Document Object Model (DOM) objects for example. The DOM is not, however, typically provided by the JavaScript engine but instead by a browser. The same is true of V8—Google Chrome provides the DOM. V8 does however provide all the data types, operators, objects and functions specified in the ECMA standard.

- was ist "server-seitiges" javascript

Vorteile:
- http://www.readwriteweb.com/hack/2010/10/why-developers-should-pay-atte.php

- laut github ist Javascript mit 19% die beliebteste Sprache
- Es wird vorzugsweise im Frontend eingesetzt
- Vorteil dieses nutzen auch im Backendbereich einzuseten
- open source, community

Was macht nodejs anders (event loops i/o)
http://www.beakkon.com/geek/node.js/why-node.js-single-thread-event-loop-javascript
http://journal.paul.querna.org/articles/2010/06/12/node-js/

    * Everything is Async:  Because the base environment has been built essentially from scratch, everything is asynchronous.  This means there is no ‘defer to thread’ like in Twisted Python;  You just can’t make blocking code.
    * No existing standard library: While this is somewhat a disadvantage today, because its harder to get going with ‘batteries included’ development, it means every bit of  Javascript is written specifically for Node.js, in a style that fits in with Node.
    * First Class Sockets and HTTP: The example Hello World is over HTTP.  Node keeps you focused on on dealing with the data, rather than spending all your time dealing with the sockets or protocols.


Its easy!
- Beispiel


[Nachteile]
- http://nodejs.org/jsconf-eu-2010.pdf
Problem 1: TLS / SSL
  - Fixed in 0.4
Problem 2: Continuous Integration Perf Testing
  - There is some thingie at http://arlolra.no.de/ that tests request/
response perf, but nothing really substantial I think.
Problem 3: Debugging
  - There is a node interactive debugger
  - There is DTrace probes
  - No event origin backtrace yet
Problem 4: Windows
  - Work in progress, node compiles on mingw but a bunch of features
is missing.
  - The switch to iocp (see slides) hasn't been made yet.
Problem 5: V8 on x64 limited to 1 GB heap
  - The bar has been raised to 1.9GB. I don't know how close the V8
team is to pushing it further.
Problem 6: Copying Strings out of V8 heap
  - Nothing changed I think.
Problem 7: Bunching Writes
  - There have been experiments with writev, but it was backed out
because of performance regressions.
Problem 8: File System Event Notification
  - No news.
Problem 9: Kernel AIO
  - No news. 


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


## Was brauchen wir ##

Vorteile hin oder her? Wie können wir es einsetzen.


## Extensions ##

https://www.cloudkick.com/blog/2010/aug/23/writing-nodejs-native-extensions/
https://github.com/pquerna/node-extension-examples
https://github.com/rbranson/node-ffi

## Appendix ##

Mailing List ; http://groups.google.com/group/nodejs


Videos zu Java Script: http://jsconfeu.blip.tv/posts?view=archive&nsfw=dc

nodejs. video: http://jsconfeu.blip.tv/file/2899135/

http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/

http://howtonode.org/how-to-module

node.js makes its trivial to manage thousand of connections

http://www.travisglines.com/web-coding/what-its-like-building-a-real-website-in-node-js 

http://www.travisglines.com/web-coding/a-simple-mvc-setup-in-node-js

### Frameworks ###

### Projects/Companies using node.js ###

### Citations ###

> The overarching point, however, is that Node is optimal for the new breed of real-time web apps. Yes, there are other means of building real-time tools, but Node lets you build real-time tools on the same platform that runs the rest of your site.
-- Cade Metz

### Terminology ###

CommonJS
- good proposals (modules, binary, package)
Threads
Grren-Threads
Co-Routines
POSIX Layer
long poll
