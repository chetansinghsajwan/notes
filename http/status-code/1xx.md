# HTTP Status Code 1xx Class

This class is known as Informational class.

This class of status code indicates an interim (non-final) response.

Since HTTP/1.0 did not define any 1xx status codes, a server _MUST NOT_ send a 1xx response to an HTTP/1.0 client.

A 1xx response is terminated by the end of the header section. it cannot contain content or trailers.

A client must be able to parse one or more 1xx responses received prior to a final response, even if the client does not expect one. A user agent _MAY_ ignore unexpected 1xx responses.

## References

- https://httpwg.org/specs/rfc9110.html#status.codes
