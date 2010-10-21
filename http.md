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

RESTful HTTP is when I don't (want to) know much about the infrastructure of the communication partner. It't not a good choice when you develop performance critical systems and you use strongly coupled systems.

### REST vs WS-\* ###

REST precedes the WS-\* standards. Most of the communication today is based on HTTP. Web services are independent from the underlying communication protocols. They can't make use of the advantages of a particular protocol.

## Best-Practices ##

### Post-Redirect-Get ###

A pattern for web development.

Problem: When a form is submitted through HTTP POST, a user that refreshes the page can cause the POST to be resubmitted.

The PRG Pattern solves this problem by returning a 303 (or sometimes 302) REDIRECT upon the POST operation instructing the browser to load a different page via HTP GET. This page can then be safely refreshed.

## HTTP Error Codes ##

Taken from Google's [HTTP status codes](http://www.google.com/support/webmasters/bin/answer.py?hl=en&answer=40132). Removed references to Google Bot.

| Code | Description |
| :---- | :---- |
| 100 (Continue) | The requestor should continue with the request. The server returns this code to indicate that it has received the first part of a request and is waiting for the rest. |
| 101 (Switching protocols) | The requestor has asked the server to switch protocols and the server is acknowledging that it will do so. |

| 200 (Successful) | The server successfully processed the request. Generally, this means that the server provided the requested page. |
| 201 (Created) | The request was successful and the server created a new resource. |
| 202 (Accepted) | The server has accepted the request, but hasn't yet processed it. |
| 203 (Non-authoritative information) | The server successfully processed the request, but is returning information that may be from another source. |
| 204 (No content) | The server successfully processed the request, but isn't returning any content. |
| 205 (Reset content) | The server successfully proccessed the request, but isn't returning any content. Unlike a 204 response, this response requires that the requestor reset the document view (for instance, clear a form for new input). |
| 206 (Partial content) | The server successfully processed a partial GET request. |

| 300 (Multiple choices) | The server has several actions available based on the request. The server may choose an action based on the requestor (user agent) or the server may present a list so the requestor can choose an action. |
| 301 (Moved permanently) | The requested page has been permanently moved to a new location. When the server returns this response (as a response to a GET or HEAD request), it automatically forwards the requestor to the new location.  |
| 302 (Moved temporarily) | The server is currently responding to the request with a page from a different location, but the requestor should continue to use the original location for future requests. This code is similar to a 301 in that for a GET or HEAD request, it automatically forwards the requestor to a different location. |
| 303 (See other location) | The server returns this code when the requestor should make a separate GET request to a different location to retrieve the response. For all requests other than a HEAD request, the server automatically forwards to the other location. |
| 304 (Not modified) | The requested page hasn't been modified since the last request. When the server returns this response, it doesn't return the contents of the page. You should configure your server to return this response (called the If-Modified-Since HTTP header) when a page hasn't changed since the last time the requestor asked for it. |
| 305 (Use proxy) | The requestor can only access the requested page using a proxy. When the server returns this response, it also indicates the proxy that the requestor should use. |
| 307 (Temporary redirect) | The server is currently responding to the request with a page from a different location, but the requestor should continue to use the original location for future requests. This code is similar to a 301 in that for a GET or HEAD request, it automatically forwards the requestor to a different location. |

| 400 (Bad request) | The server didn't understand the syntax of the request. |
| 401 (Not authorized) | The request requires authentication. The server might return this response for a page behind a login. |
| 403 (Forbidden) | The server is refusing the request. |
| 404 (Not found) | The server can't find the requested page. For instance, the server often returns this code if the request is for a page that doesn't exist on the server. |
| 405 (Method not allowed) | The method specified in the request is not allowed. |
| 406 (Not acceptable) | The requested page can't respond with the content characteristics requested. |
| 407 (Proxy authentication required) | This status code is similar 401 (Not authorized); but specifies that the requestor has to authenticate using a proxy. When the server returns this response, it also indicates the proxy that the requestor should use. |
| 408 (Request timeout) | The server timed out waiting for the request. |
| 409 (Conflict) | The server encountered a conflict fulfilling the request. The server must include information about the conflict in the response. The server might return this code in response to a PUT request that conflicts with an earlier request, along with a list of differences between the requests. |
| 410 (Gone) | The server returns this response when the requested resource has been permanently removed. It is similar to a 404 (Not found) code, but is sometimes used in the place of a 404 for resources that used to exist but no longer do. If the resource has permanently moved, you should use a 301 to specify the resource's new location. |
| 411 (Length required) | The server won't accept the request without a valid Content-Length header field. |
| 412 (Precondition failed) | The server doesn't meet one of the preconditions that the requestor put on the request. |
| 413 (Request entity too large) | The server can't process the request because it is too large for the server to handle. |
| 414 (Requested URI is too long) | The requested URI (typically, a URL) is too long for the server to process. |
| 415 (Unsupported media type) | The request is in a format not support by the requested page. |
| 416 (Requested range not satisfiable) | The server returns this status code if the request is for a range not available for the page. |
| 417 (Expectation failed) | The server can't meet the requirements of the Expect request-header field. |

| 500 (Internal server error) | The server encountered an error and can't fulfill the request. |
| 501 (Not implemented) | The server doesn't have the functionality to fulfill the request. For instance, the server might return this code when it doesn't recognize the request method. |
| 502 (Bad gateway) | The server was acting as a gateway or proxy and received an invalid response from the upstream server. |
| 503 (Service unavailable) | The server is currently unavailable (because it is overloaded or down for maintenance). Generally, this is a temporary state. |
| 504 (Gateway timeout) | The server was acting as a gateway or proxy and didn't receive a timely request from the upstream server. |
| 505 (HTTP version not supported) | The server doesn't support the HTTP protocol version used in the request. |

## Testing ##

A good resource for testing HTTP with CURL can be found [here](http://curl.haxx.se/docs/httpscripting.html)