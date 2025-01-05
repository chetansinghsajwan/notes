# HTTP Trace Method

This method is used to request the final recipent to return the request recieved in the form of response. This allows client to see what is being received by the other end of the request chain.

The final recipient is either the origin server or the first server to receive a [Max-Forwards](https://httpwg.org/specs/rfc9110.html#field.max-forwards) value of zero (0) in the request ([Section 7.6.2](https://httpwg.org/specs/rfc9110.html#field.max-forwards "Max-Forwards")).

The server should respond with status code [200 OK](/http/status-codes/200-ok) if returning back the request.

The server should exclude some fields when returning the request though.

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/TRACE
