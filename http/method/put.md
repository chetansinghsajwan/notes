# HTTP Put Method

This method request the server to create a new resource or update the target resource if already exists.

- Request has [body](/http/body).
- Request is not [safe](/http/method-property/safe).
- Request is [idempotent](http/method-property/idempotent).
- Request is not [cacheable](/http/method-property/cacheable).

If the target resource does not have a current representation and the PUT successfully creates one, then the origin server must inform the user agent by sending a [201 (Created)](201.md) response. If the target resource does have a current representation and that representation is successfully modified according to the content in the request, then the origin server must send either a [200 (OK)](200.md) or a [204 (No Content)](204.md) response to indicate successful completion of the request.

An origin server must send a [validator field](/http/field/validator-fields) in a successful response to PUT unless the request's representation data was saved without any transformation applied to the content and the validator field value reflects the new representation. This requirement allows a user agent to know when the representation it sent is the result of the PUT, and thus it doesn't need to be retrieved again from the origin server.

## References

- https://httpwg.org/specs/rfc9110.html#PUT
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT
