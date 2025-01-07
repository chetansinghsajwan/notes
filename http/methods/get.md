# HTTP Get Method

This method is used to request the server to return a representation of the resource.

- Request does not have [body](/http/body).
- Request is [safe](/http/requests/safe).
- Request is [idempotent](/http/requests/idempotent).
- Request is [cacheable](/http/requests/cacheable).

This method has no defined semantics for the body, so it should be empty. Some implementations might reject the request.

## References

- https://httpwg.org/specs/rfc9110.html#GET
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET
