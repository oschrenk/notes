# Authentication #

- **Authentication** is any process by which you verify that someone is who they claim they are. It is about identifying the identity of the user. Is the user who say they are?
- **Authorization** is finding out if the person, once identified, is permitted to have the resource, it is requesting.
- **Access Control** is about controlling entrance by some arbitrary condition which may or may not have anything to do with the attributes of the particular visitor.

## Http BasicAuth ##

HTTP BasicAuth was defined in [RFC 1945](http://tools.ietf.org/html/rfc1945) and improved upon in [RFC 2616](http://tools.ietf.org/html/rfc2616) and [RFC 2617](http://tools.ietf.org/html/rfc2617).

It provides access to web resources by providing a username and password when making a request. It does so by concatenating the username, a colon and the password, encoding it with BASE64 (not for security reasons but just as a means of escaping no HTTP-compatible characters).

1. Client asks for a web resource
2. Server responds with `HTTP/1.1 401 Authorization Required` and provides the authentication realm.
3. User decides to let client repeat the request by supplying username and password as `Authorization` header
4. Server responds with `HTTP/1.1 200 OK` if authentication was accepted, 401 otherwise.

## OpenId ##

### Terminology ###

- **Identity Provider**: In the real world one of the most commonly known identity provider is the DMV. You provide the information and they give you a drivers license.
- **Relying Party**: If a cop stops you, he relies on the information provided by the the DMV, the *Identity Provider*, to issue you a ticket. He puts trust into the identity provider that the information is correct.

### Idea ###

Open ID gives you one login for multiple sites. Each time you need to log into a site using Open ID you will be redirected to your Open ID site where you login, and then back to the site you wanted originally to log into.

With Open ID, there is no suggestion of two web apps sharing your data. Except in the very limited sense that the Open ID provider may hold some general information about you, e.g. some photos, addresses, phone numbers, etc., and with your consent, send it back to the consumer so you don’t have to re-enter all the boring profile details again. However, this is data of a generic, non-application-specific, nature. (And even this limited form of sharing is an extension to the core Open ID spec.)

## OAuth ##

### Idea ###

OAuth lets you authorize one website – the consumer – to access your data from another website – the provider. For instance, you want to authorize a printing provider – call it Moo – to grab your photos from a photo repository – call it Flickr. Moo will redirect you to Flickr which will ask you, for instance, “Moo wants to download your Flickr photos. Is that cool?”, and then back to Moo to print your photos.

With OAuth, you still need to log into the provider. e.g. When Moo sends you to Flickr, you still have to log into Flickr (or be logged in already). How Flickr decides you’re logged in is completely orthogonal to OAuth. It could be a standard username-password login, it could be via a physical password device, or it could well be via Open ID.

With OAuth, any information you hold on any website can be shared with another website. You could share your GMail with a clever consumer that automatically tags items by inspecting the content, if GMail was an OAuth consumer.

### OAuth 2.0 ###

#### Terminology ####

**Protected Resource**:

- resides on a server
- requires authorization

**Resource Owner**:

- owns protected resource
- approves access

**Server**:

- holds the protected resource

**Client**:

- web application
- needs access to protected resource

OAuth has different flavors:

1. individual own the resource, decides whether to grant access
2. company own the resource, access is granted by IT guardians

#### Individual own the resource, decides whether to grant access ####

- developer registers application with Google, gets a `client_id` and `client_secret`
- application redirects user to Google specifying `client_id`, `redirect_uri` (URI where to redirect the user on success), `scope` (what API is the application is trying to get access to)
- Google redirects back to the application `redirect_url` and includes an `authorization_code` in the URL.
- the application performs a HTTP `Post` request to Google, including `client_id`, `client_secret` and `code`. Google returns an `access_token` and a `refresh_token`. `refresh_token_` is used to regain access when the `access_token` expires.

**Tip**
- put the `access_token` in the HTTP header instead as a query param. Prevents (some) caching. Looks better.

## HMac ##

> A server and a client know a public and private key; only the server and client know the private key, but everyone can know the public key… who cares what they know.
> A client creates a unique HMAC (hash) representing it’s request to the server. It does this by combining the request data (arguments and values or XML/JSON or whatever it was planning on sending) and hashing the blob of request data along with the private key.
>The client then sends that HASH to the server, along with all the arguments and values it was going to send anyway.
> The server gets the request and re-generates it’s own unique HMAC (hash) based on the submitted values using the same methods the client used.
> The server then compares the two HMACs, if they are equal, then the server trusts the client, and runs the request.

- the client does not encode the payload
- the client sends a checksum(hash) of the payload along with the payload using a private key that only the client and server know
- as the server also  has access to the private key it can generate the same hash of the payload and can compare the results
