# HTTP Options Method

This method is used to request information abut communication options available for the target resource at either the origin server or an intervening intermediary.

- Request has [body](/http/body).
- Request is [safe](/http/requests/safe).
- Request is [idempotent](/http/requests/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

This method allows a client to determine the options and/or requirements associated with a resource, or the capabilities of a server, without implying a resource action.

A request with an asterisk `*` as the request target applies to the server in general rather than to a specific resource. Since a server's communication options typically depend on the resource, the request is only useful as a "ping" or "no-op" type of method; it does nothing beyond allowing the client to test the capabilities of the server. For example, this can be used to test a proxy for HTTP/1.1 conformance (or lack thereof).

## References

- https://httpwg.org/specs/rfc9110.html#OPTIONS
