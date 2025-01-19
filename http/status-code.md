# HTTP Status Code

It is a three digit number which defines the result of a request. It is used only in response.

All valid status codes are within the range of 100 to 599, inclusive.

The first digit of the status code defines the category. There are five categories:

- [1xx (Informational)](http/status-code/1xx)
- [2xx (Successful)](http/status-code/2xx)
- [3xx (Redirection)](http/status-code/3xx)
- [4xx (Client Error)](https://httpwg.org/specs/rfc9110.html#status.4xx): The request contains bad syntax or cannot be fulfilled
- [5xx (Server Error)](https://httpwg.org/specs/rfc9110.html#status.5xx): The server failed to fulfill an apparently valid request

## References

- https://httpwg.org/specs/rfc9110.html#status.codes
