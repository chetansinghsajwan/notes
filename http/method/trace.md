---
status: completed
---

# HTTP Trace Method

This method is used to request the final recipent to return the request recieved in the form of response. This allows client to see what is being received by the other end of the request chain.

The final recipient is either the origin server or the first server to receive a [Max-Forwards](/http/fields/max-forwards) value of zero in the request.

The server should respond with status code [200 OK](/http/status-codes/200-ok) and with [Content-Type](/http/headers/content-type) of `message/http`. The server should exclude any fields that are likely to contain sensitive data when responding.

- Request must not have [body](/http/body).
- Request is [safe](/http/requests/safe).
- Request is [idempotent](http/request/idempotent.md).
- Request is not [cacheable](/http/requests/cacheable).

## References

- https://httpwg.org/specs/rfc9110.html#TRACE
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/TRACE