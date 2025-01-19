# HTTP Status Code

It is a three digit number which defines the result of a request. It is used only in response.

All valid status codes are within the range of 100 to 599, inclusive.

The first digit of the status code defines the class. There are five classes:

- [1xx (Informational)](http/status-code/1xx)
- [2xx (Successful)](http/status-code/2xx)
- [3xx (Redirection)](http/status-code/3xx)
- [4xx (Client Error)](http/status-code/4xx)
- [5xx (Server Error)](http/status-code/5xx)

HTTP status codes are extensible.

A client is not required to understand the meaning of all registered status codes, though such understanding is obviously desirable. However, a client must understand the class of any status code, as indicated by the first digit, and treat an unrecognized status code as being equivalent to the x00 status code of that class.

Values outside the range 100..599 are invalid. Implementations often use values outside of that range for internal communication of non-HTTP status (For example, library errors). A client that receives a response with an invalid status code should process the response as if it had a [5xx (Server Error)](http/status-code/5xx) status code.

A single request can have multiple associated responses: zero or more interim (non-final) responses with status codes in the "informational" ([1xx](https://httpwg.org/specs/rfc9110.html#status.1xx)) range, followed by exactly one final response with a status code in one of the other ranges.

## References

- https://httpwg.org/specs/rfc9110.html#status.codes
