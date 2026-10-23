This method is used to request the server to return a representation of the resource.
- Request does not have body.
- Request is [[Method Safe Property|safe]].
- Request is [[Method Idempotent Property|idempotent]].
- Request is [[Method Cacheable Property|cacheable]].
This method has no defined semantics for the body, so it should be empty. Some implementations might reject the request because of its potential as a request smuggling attack.
A client can alter the semantics of GET to be a “range request”, requesting transfer of only some parts of the selected representation, by sending a HTTP Range Field header field in the request.
The response to a GET request can be used to satisfy subsequent GET and HEAD requests.
## References
- https://httpwg.org/specs/rfc9110.html\#GET
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET