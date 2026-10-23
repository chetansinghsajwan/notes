This method is used to request the server to delete a resource.
- Request does not have body.
- Request is not [[Method Safe Property|safe]].
- Request is [[Method Idempotent Property|idempotent]].
- Request is not [[Method Cacheable Property|cacheable]].
This method has no defined semantics for the body, so it should be empty. Some implementations might reject the request because of its potential as a request smuggling attack.
If a successful DELETE request passes through a cache that has one or more stored responses for the target URI, those stored responses will be invalidated.
## References
- https://httpwg.org/specs/rfc9110.html\#DELETE
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE