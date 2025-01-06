# HTTP Put Method

This method request the server to create a new resource or update the target resource if already exists.

- Request has [body](/http/body).
- Request is [safe](/http/requests/safe).
- Request is [idempotent](/http/requests/idempotent).
- Request is [cacheable](/http/requests/cacheable).

If the target resource does not have a current representation and the PUT successfully creates one, then the origin server _MUST_ inform the user agent by sending a [201 (Created)](https://httpwg.org/specs/rfc9110.html#status.201) response. If the target resource does have a current representation and that representation is successfully modified in accordance with the state of the enclosed representation, then the origin server _MUST_ send either a [200 (OK)](https://httpwg.org/specs/rfc9110.html#status.200) or a [204 (No Content)](https://httpwg.org/specs/rfc9110.html#status.204) response to indicate successful completion of the request.

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT
