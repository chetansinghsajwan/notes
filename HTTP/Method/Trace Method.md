This method is used to request the final recipent to return the request recieved in the form of response. This allows client to see what is being received by the other end of the request chain.
The final recipient is either the origin server or the first server to receive a Max-Forwards value of zero in the request.
The server should respond with status code 200 and with Content-Type of `message/http`. The server should exclude any fields that are likely to contain sensitive data when responding.
- Request must not have body.
- Request is [[Method Safe Property|safe]].
- Request is [[Method Idempotent Property|idempotent]].
- Request is not [[Method Cacheable Property|cacheable]].
## References
- https://httpwg.org/specs/rfc9110.html\#TRACE
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/TRACE