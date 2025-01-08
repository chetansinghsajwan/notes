---
status: completed
---

# HTTP Delete Method

This method is used to request the server to delete a resource.

- Request does not have [body](/http/body).
- Request is not [safe](/httpmethod-property/safe).
- Request is [idempotent](http/method-property/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

This method has no defined semantics for the body, so it should be empty. Some implementations might reject the request because of its potential as a [request smuggling attack](/http/security/request-smuggling-attack).

If a successful DELETE request passes through a cache that has one or more stored responses for the target URI, those stored responses will be invalidated.

## References

- https://httpwg.org/specs/rfc9110.html#DELETE
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE
