# HTTP #

The foundation of the web is the HTTP protocol ([RFC 2616](http://www.ietf.org/rfc/rfc2616.txt) covers HTTP 1.1). The protocol is part of the application layer (layer 5-7 according to the ISO model). Key figures defining the protocol were [Roy Fielding](http://www.ics.uci.edu/~fielding/) and Tim Berners Lee.

In short HTTP brings pages from the servers to your browser. With it you can uniquely identify resources worldwide. A web page is just a representation of such a resource. The URL tell the browser, that there is a concept for this particular resource. A browser can than ask for a representation of this concept. HTML is one type of a representation. A _web service_ is just another one.

URLs are a key component of the language of a machine. There are much like a noun. To use these nouns we need verbs: POST, GET, PUT, DELETE. These verbs suffice to describe a CRUD system. You just need to agree on what information is being exchanged.

## REST ##

Is an abstract architecture style defined by [Roy Fielding](http://www.ics.uci.edu/~fielding/) defined in his dissertation [Architectural Styles and the Design of Network-based Software Architectures](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm).

RESTful HTTP is an application of the web standards in particular HTTP aligned with the REST architectural style.

5 key ideas:

*   each concept gets its own ID, a URI
*   standard methods to interact with resources (`GET`, `PUT`, `POST`, `DELETE`)
*   you can link to other resources (with the URI)
*   each resource can have different representations
*   stateless communication to have entry points in the application

`GET` is a safe operation, meaning that it a read only operation, easily cacheable, easily scalable

RESTful HTTP is when I don&#8217;t (want to) know much about the infrastructure of the communication partner. It&#8217;t not a good choice when you develop performance critical systems and you use strongly coupled systems.

### REST vs WS-\* ###

REST precedes the WS-\* standards. Most of the communication today is based on HTTP. Web services are independent from the underlying communication protocols. They can&#8217;t make use of the advantages of a particular protocol.

## Best-Practices ##

### Post-Redirect-Get ###

A pattern for web development.

Problem: When a form is submitted through HTTP POST, a user that refreshes the page can cause the POST to be resubmitted.

The PRG Pattern solves this problem by returning a 303 (or sometimes 302) REDIRECT upon the POST operation instructing the browser to load a different page via HTP GET. This page can then be safely refreshed.

## cURL ##

A good resource for testing HTTP with CURL can be found [here](http://curl.haxx.se/docs/httpscripting.html)

### POST a file ###

Given this form

    <form method="POST" enctype='multipart/form-data' action="upload.cgi">
    	<input type=file name=upload>
    	<input type=submit name=press value="OK">
    </form>

you can POST it with

    curl -F upload=@localfilename -F press=OK [URL]$1