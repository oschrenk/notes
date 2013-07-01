# REST API Design #


- **Design your API for developers first**, they are the main users. In that respect, simplicity and intuitivity matter.
- **Use HTTP verbs** instead of relying on parameters (e.g. `?action=create`). HTTP verbs map nicely with CRUD:
	- `POST` for create,
	- `GET` for read,
	- `DELETE` for remove,
	- `PUT` for update,
	- `PATCH` for partial update
- **Endpoint names should be plural** It may hurt your inner grammar nazi, but it helps keeping things easy (such as edge cases with odd pluralization)
- **Use SSL everywhere - all the time**
- **Use HTTP status codes**, especially for errors (authentication required, error on the server side, incorrect parameters). There are plenty to choose from, here are a few:
	- `200`: OK
	- `201`: Created
	- `304`: Not Modified
	- `400`: Bad Request
	- `401`: Unauthorized
	- `403`: Forbidden
	- `404`: Not Found
	- `500`: Internal Server Error
- **Simple URLs for resources**: first a noun for the collection, then the item. For example `/emails` and `/emails/1234`; the former gives you the collection of emails, the second one a specific one identified by its internal id.
- **Use verbs for special actions**. For example, `/search?q=my+keywords`.
- **Keep errors simple but verbose** (and use HTTP codes). We only send something like { message: "Something terribly wrong happened" } with the proper status code (e.g. 401 if the call requires authentication) and log more verbose information (origin, error code…) in the backend for debugging and monitoring.

Relying on HTTP status codes and verbs should already help you keep your API calls and responses lean enough. Less crucial, but still useful:

- JSON first, then extend to other formats if needed and if time permits.
- Unix time, or you’ll have a bad time.
- Prepend your URLs with the API version, like `/v1/emails/1234`.
- Lowercase everywhere in URLs.

## Sources ##

- Ronan Berder's [Designing A RESTful API That Doesn't Suck](devo.ps/blog/2013/03/22/designing-a-restful-api-that-doesn-t-suck.html):
- Vanay Sahni's [Best Practices for Designing a Pragmatic RESTful API](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#errors)
