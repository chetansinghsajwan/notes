# HTTP Header

**HTTP headers** let the client and the server pass additional information with a message in a request or response.

In `HTTP/1.X`, a header is a case-insensitive name followed by a colon, then optional whitespace which will be ignored, and finally by its value (for example: `Allow: POST`).

Custom proprietary headers have historically been used with an `X-` prefix, but this convention was deprecated in 2012 because of the inconveniences it caused when nonstandard fields became standard.

A server _MUST NOT_ apply a request to the target resource until it receives the entire request header section, since later header field lines might include conditionals, authentication credentials, or deliberately misleading duplicate header fields that could impact request processing.

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers
- https://httpwg.org/specs/rfc9110.html#fields
