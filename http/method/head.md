---
status: completed
---

# HTTP Head Method

This method is used to request the server to return metadata about the resource. The metdata is sent in the form of headers in the response.

- Request does not have [body](/http/body).
- Request is [safe](/httpmethod-property/safe).
- Request is [idempotent](http/method-property/idempotent).
- Request is [cacheable](/http/requests/cacheable).

The server should send the same header fields in response to a HEAD request as it would have sent if the request method had been GET. However, a server may omit header fields for which a value is determined only while generating the content.

This method has no defined semantics for the body, so it should be empty. Some implementations might reject the request because of its potential as a [request smuggling attack](/http/security/request-smuggling-attack).

The response to a GET request can be used to satisfy subsequent HEAD requests. A HEAD response might also affect previously cached responses to GET.

## References

- https://httpwg.org/specs/rfc9110.html#HEAD
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD
