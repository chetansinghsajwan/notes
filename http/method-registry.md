# HTTP Method Registry

It is a centralized list maintained by [IANA](iana/iana) that defines standard HTTP methods.

The registry is located at https://www.iana.org/assignments/http-methods.

HTTP method registrations _MUST_ include the following fields:

- Method Name
- Safe ("yes" or "no")
- Idempotent ("yes" or "no")
- Pointer to specification text

The party generating the registration request should follow the guidelines for the new method specified at https://httpwg.org/specs/rfc9110.html#considerations.for.new.methods.

## References

- https://httpwg.org/specs/rfc9110.html#method.registry
