# HTTP Header

HTTP Header refers to a field present in the header section of message 

Custom proprietary headers have historically been used with an `X-` prefix, but this convention was deprecated in 2012 because of the inconveniences it caused when nonstandard fields became standard.

A server must not apply a request to the target resource until it receives the entire request header section, since later header field lines might include conditionals, authentication credentials, or deliberately misleading duplicate header fields that could impact request processing.

## References

- https://httpwg.org/specs/rfc9110.html#fields
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers
