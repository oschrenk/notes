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

## OpenId ##

Open ID gives you one login for multiple sites. Each time you need to log into a site using Open ID you will be redirected to your Open ID site where you login, and then back to the site you wanted orignally to log into.

With Open ID, there is no suggestion of two web apps sharing your data. Except in the very limited sense that the Open ID provider may hold some general information about you, e.g. some photos, addresses, phone numbers, etc., and with your consent, send it back to the consumer so you don’t have to re-enter all the boring profile details again. However, this is data of a generic, non-application-specific, nature. (And even this limited form of sharing is an extension to the core Open ID spec.) 

## OAuth ##

OAuth lets you authorise one website – the consumer – to access your data from another website – the provider. For instance, you want to authorise a printing provider – call it Moo – to grab your photos from a photo repository – call it Flickr. Moo will redirect you to Flickr which will ask you, for instance, “Moo wants to download your Flickr photos. Is that cool?”, and then back to Moo to print your photos.

With OAuth, you still need to log into the provider. e.g. When Moo sends you to Flickr, you still have to log into Flickr (or be logged in already). How Flickr decides you’re logged in is completely orthogonal to OAuth. It could be a standard username-password login, it could be via a physical password device, or it could well be via Open ID.

With OAuth, any information you hold on any website can be shared with another website. You could share your GMail with a clever consumer that automatically tags items by inspecting the content, if GMail was an OpenAuth consumer.