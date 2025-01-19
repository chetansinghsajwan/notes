# HTTP Status Code 5xx Class

The 5xx (Server Error) class of status code indicates that the server is aware that it has erred or is incapable of performing the requested method. Except when responding to a HEAD request, the server _SHOULD_ send a representation containing an explanation of the error situation, and whether it is a temporary or permanent condition. A user agent _SHOULD_ display any included representation to the user. These status codes are applicable to any request method.

## References

- https://httpwg.org/specs/rfc9110.html#status.5xx
