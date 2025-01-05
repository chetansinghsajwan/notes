# HTTP Trace Method

- Request has [body](/http/body).
- Request is [safe](/http/requests/safe).
- Request is [idempotent](/http/requests/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

This method is used to request the final recipent to return the request recieved in the form of response. This allows client to see what is being received by the other end of the request chain.

The final recipient is either the origin server or the first server to receive a [Max-Forwards](/http/fields/max-forwards) value of zero in the request.

The server should respond with status code [200 OK](/http/status-codes/200-ok) if returning back the request.

The server should exclude any request fields that are likely to contain sensitive data when that recipient generates the response content.

The client must not send content in this request.

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/TRACE
