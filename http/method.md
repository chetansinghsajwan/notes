---
status: completed
---

# HTTP Request Method

HTTP Methods are used to define the purpose of the request. For example, `GET` method is used when the client wants to get data from the server, while `POST` method is used when client wants to post data to server.

The following 8 methods are standardized by [RFC 9110](/ietf/rfc/9110):

- [GET](/http/methods/get)
- [POST](/http/methods/post)
- [PUT](/http/methods/put)
- [DELETE](/http/methods/delete)
- [OPTIONS](/http/methods/options)
- [HEAD](/http/methods/head)
- [TRACE](/http/methods/trace)
- [CONNECT](/http/methods/connect)

All general-purpose servers must support the methods GET and HEAD. All other methods are optional.

An origin server that receives a request method that is unrecognized or not implemented _SHOULD_ respond with the [501 (Not Implemented)](/http/status/501) status code. An origin server that receives a request method that is recognized and implemented, but not allowed for the target resource, _SHOULD_ respond with the [405 (Method Not Allowed)](/http/status/405) status code.

## References

- https://httpwg.org/specs/rfc9110.html#method.overview
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods
