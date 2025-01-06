# HTTP Post Method

This method is used to send data to the server. The data is stored in the requests body and the type of data is represented by [`Content-Type`](/http/headers/content-type) header.

- Request has [body](/http/body).
- Request is not [safe](/http/requests/safe).
- Request is not [idempotent](/http/requests/idempotent).
- Request is sometimes [cacheable](/http/requests/cacheable) (see below).

almost all of the status codes defined by this specification could be received in a response to POST (the exceptions being [206 (Partial Content)](https://httpwg.org/specs/rfc9110.html#status.206), [304 (Not Modified)](https://httpwg.org/specs/rfc9110.html#status.304), and [416 (Range Not Satisfiable)](https://httpwg.org/specs/rfc9110.html#status.416)).

Responses to POST requests are only cacheable when they include explicit freshness information (see [Section 4.2.1](https://httpwg.org/specs/rfc9111.html#calculating.freshness.lifetime "Calculating Freshness Lifetime") of [[CACHING]](https://httpwg.org/specs/rfc9110.html#CACHING)) and a [Content-Location](https://httpwg.org/specs/rfc9110.html#field.content-location) header field that has the same value as the POST's target URI ([Section 8.7](https://httpwg.org/specs/rfc9110.html#field.content-location "Content-Location")). A cached POST response can be reused to satisfy a later GET or HEAD request. In contrast, a POST request cannot be satisfied by a cached POST response because POST is potentially unsafe; see [Section 4](https://httpwg.org/specs/rfc9111.html#constructing.responses.from.caches "Constructing Responses from Caches") of [[CACHING]](https://httpwg.org/specs/rfc9110.html#CACHING).

If the result of processing a POST would be equivalent to a representation of an existing resource, an origin server _MAY_ redirect the user agent to that resource by sending a [303 (See Other)](https://httpwg.org/specs/rfc9110.html#status.303) response with the existing resource's identifier in the [Location](https://httpwg.org/specs/rfc9110.html#field.location) field.

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST
- https://httpwg.org/specs/rfc9110.html
