---
status: completed
---

# HTTP Options Method

This method is used to request information abut communication options available for the target resource at either the origin server or an intervening intermediary.

- Request has [body](/http/body).
- Request is [safe](/http/method-property/safe).
- Request is [idempotent](http/method-property/idempotent).
- Request is not [cacheable](/http/method-property/cacheable).

This method allows a client to determine the options and/or requirements associated with a resource, or the capabilities of a server, without implying a resource action.

A request with an asterisk `*` as the request target applies to the server in general rather than to a specific resource. Since a server's communication options typically depend on the resource, the request is only useful as a "ping" or "no-op" type of method; it does nothing beyond allowing the client to test the capabilities of the server. For example, this can be used to test a proxy for HTTP/1.1 conformance (or lack thereof).

A server generating a successful response to OPTIONS _SHOULD_ send any header that might indicate optional features implemented by the server and applicable to the target resource.

The response content, if any, might also describe the communication options in a machine or human-readable representation.

A client maysend a [Max-Forwards](/http/fields/max-forwards) header field in an OPTIONS request to target a specific recipient in the request chain. A proxy must not generate a Max-Forwards header field while forwarding a request unless that request was received with a Max-Forwards field.

A client that generates an this request containing content must send a valid [Content-Type](/http/fields/content-type) header field describing the representation media type.

## References

- https://httpwg.org/specs/rfc9110.html#OPTIONS
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS
