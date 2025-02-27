# HTTP Post Method

This method is used to send data to the server. The data is stored in the requests body and the type of data is represented by [`Content-Type`](/http/field/content-type) header.

- Request has [body](/http/body).
- Request is not [safe](/http/method-property/safe).
- Request is not [idempotent](http/method-property/idempotent).
- Request is sometimes [cacheable](/http/method-property/cacheable) (see below).

Almost all of the status codes defined in HTTP specification can be used in response to POST, except [206 (Partial Content)](206.md), [304 (Not Modified)](304.md), and [416 (Range Not Satisfiable)](416.md).

If one or more resources has been created on the origin server as a result of successfully processing a POST request, the origin server should send a [201 (Created)](201.md) response containing a [Location](/http/field/location) header field that provides an identifier for the primary resource created and a representation of the new resources in the body.

If the result of processing a POST would be equivalent to a representation of an existing resource, an origin server _MAY_ redirect the user agent to that resource by sending a [303 (See Other)](303.md) response with the existing resource's identifier in the [Location](/http/field/location) field.

Responses to POST requests are only cacheable when they include explicit freshness information and a [Content-Location](/http/field/content-location) header field that has the same value as the POST's target URI. A cached POST response can be reused to satisfy a later GET or HEAD request. In contrast, a POST request cannot be satisfied by a cached POST response because POST is potentially unsafe.

## References

- https://httpwg.org/specs/rfc9110.html#POST
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST
