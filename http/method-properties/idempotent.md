# HTTP Method Impodent Property

A request method is considered idempotent if the intended effect on the server of multiple identical requests with that method is the same as the effect for a single such request.

All safe methods are considered impodent by default.

Idempotent methods are distinguished because the request can be repeated automatically if a communication failure occurs before the client is able to read the server's response. For example, if a client sends a PUT request and the underlying connection is closed before any response is received, then the client can establish a new connection and retry the idempotent request. It knows that repeating the request will have the same intended effect, even if the original request succeeded, though the response might differ.

## References

- https://httpwg.org/specs/rfc9110.html#idempotent.methods
