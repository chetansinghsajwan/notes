# HTTP Post Method

This method is used to send data to the server. The data is stored in the requests body and the type of data is represented by [`Content-Type`](/http/headers/content-type) header.

- Request has [body](/http/body).
- Request is not [safe](/http/requests/safe).
- Request is not [idempotent](/http/requests/idempotent).
- Request is [cacheable](/http/requests/cacheable) only if freshness information is included.

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST
- https://httpwg.org/specs/rfc9110.html
