---
status: completed
---

# HTTP Put Method

This method request the server to create a new resource or update the target resource if already exists.

- Request has [body](/http/body).
- Request is not [safe](/httpmethod-property/safe).
- Request is [idempotent](http/method-property/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

If the target resource does not have a current representation and the PUT successfully creates one, then the origin server must inform the user agent by sending a [201 (Created)](http/status/201) response. If the target resource does have a current representation and that representation is successfully modified according to the content in the request, then the origin server must send either a [200 (OK)](http/status/200) or a [204 (No Content)](http/status/204) response to indicate successful completion of the request.

An origin server must send a [validator field](/http/fields/validator-fields) in a successful response to PUT unless the request's representation data was saved without any transformation applied to the content and the validator field value reflects the new representation. This requirement allows a user agent to know when the representation it sent is the result of the PUT, and thus it doesn't need to be retrieved again from the origin server.

## References

- https://httpwg.org/specs/rfc9110.html#PUT
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT
