# Authentication #

- **Authentication** is any process by which you verify that someone is who they claim they are
- **Authorization** is finding out if the person, once identified, is permitted to have the resource.
- **Access Control** is about controlling entrance by some arbitrary condition which may or may not have anything to do with the attributes of the particular visitor.

## Http BasicAuth ##

HTTP BasicAuth was defined in [RFC 1945](http://tools.ietf.org/html/rfc1945) and improved upon in [RFC 2616](http://tools.ietf.org/html/rfc2616) and [RFC 2617](http://tools.ietf.org/html/rfc2617).

It provides access to web resources by providing a username and password when making a request. It does so by concatenating the username, a colon and the password, encoding it with BASE64 (not for security reasons but just as a means of escaping no HTTP-compatible characters).

1. Client asks for a web resource
2. Server responds with `HTTP/1.1 401 Authorization Required` and provides the authentication realm. 
3. User decides to let client repeat the request by supplying username and password as `Authorization` header
4. Server responds with `HTTP/1.1 200 OK` if authentication was accepted, 401 otherwise.