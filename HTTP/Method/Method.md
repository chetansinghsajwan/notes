HTTP Methods are used to define the purpose of the request. For example, `GET` method is used when the client wants to get data from the server, while `POST` method is used when client wants to post data to server.
The following 8 methods are standardized by RFC 9110:
- [[Get Method|HTTP Get Method]]
- [[Post Method|HTTP Post Method]]
- [[Put Method|HTTP Put Method]]
- [[HTTP/Method/Delete Method|delete]]
- [[Options Method|HTTP Options Method]]
- [[HTTP/Method/Head Method|head]]
- [[Trace Method|HTTP Trace Method]]
- [[Connect Method|HTTP Connect Method]]
All general-purpose servers must support the methods GET and HEAD. All other methods are optional.
An origin server that receives a request method that is unrecognized or not implemented _SHOULD_ respond with the 501 status code. An origin server that receives a request method that is recognized and implemented, but not allowed for the target resource, _SHOULD_ respond with the 405 status code.
Additional methods other than the one specified in the specification are registered in [[Method Registry|HTTP Method Registry]].
## References
- https://httpwg.org/specs/rfc9110.html\#method.overview
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods