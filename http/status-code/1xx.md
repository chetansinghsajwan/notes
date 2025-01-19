# HTTP Status Code 1xx Class

This class is known as Informational class of status code.

This class of status code indicates an interim (non-final) response.

Since HTTP/1.0 did not define any 1xx status codes, a server _MUST NOT_ send a 1xx response to an HTTP/1.0 client.

A 1xx response is terminated by the end of the header section. it cannot contain content or trailers.

A client must be able to parse one or more 1xx responses received prior to a final response, even if the client does not expect one. 

A proxy _MUST_ forward 1xx responses unless the proxy itself requested the generation of the 1xx response. For example, if a proxy adds an "Expect: 100-continue" header field when it forwards a request, then it need not forward the corresponding [100 (Continue)](http/status-code/100) response(s).

## References

- https://httpwg.org/specs/rfc9110.html#status.codes
