# HTTP Put Method

This method request the server to create a new resource or update the target resource if already exists.

- Request has [body](/http/body).
- Request is not [safe](/http/requests/safe).
- Request is [idempotent](/http/requests/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

If the target resource does not have a current representation and the PUT successfully creates one, then the origin server _MUST_ inform the user agent by sending a [201 (Created)](https://httpwg.org/specs/rfc9110.html#status.201) response. If the target resource does have a current representation and that representation is successfully modified in accordance with the state of the enclosed representation, then the origin server _MUST_ send either a [200 (OK)](https://httpwg.org/specs/rfc9110.html#status.200) or a [204 (No Content)](https://httpwg.org/specs/rfc9110.html#status.204) response to indicate successful completion of the request.

An origin server _MUST NOT_ send a validator field ([Section 8.8](https://httpwg.org/specs/rfc9110.html#response.validator "Validator Fields")), such as an [ETag](https://httpwg.org/specs/rfc9110.html#field.etag) or [Last-Modified](https://httpwg.org/specs/rfc9110.html#field.last-modified) field, in a successful response to PUT unless the request's representation data was saved without any transformation applied to the content (i.e., the resource's new representation data is identical to the content received in the PUT request) and the validator field value reflects the new representation. This requirement allows a user agent to know when the representation it sent (and retains in memory) is the result of the PUT, and thus it doesn't need to be retrieved again from the origin server. The new validator(s) received in the response can be used for future conditional requests in order to prevent accidental overwrites ([Section 13.1](https://httpwg.org/specs/rfc9110.html#preconditions "Preconditions")).

## References

- https://httpwg.org/specs/rfc9110.html#PUT
