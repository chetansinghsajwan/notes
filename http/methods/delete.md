# HTTP Delete Method

This method is used to request the server to delete a resource.

- Request does not have [body](/http/body).
- Request is not [safe](/http/requests/safe).
- Request is [idempotent](/http/requests/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

---
**Request Body**

Content received in a DELETE request has no generally defined semantics, cannot alter the meaning or target of the request, and might lead some implementations to reject the request and close the connection because of its potential as a request smuggling attack ([Section 11.2](https://httpwg.org/specs/rfc9112.html#request.smuggling "Request Smuggling") of [[HTTP/1.1]](https://httpwg.org/specs/rfc9110.html#HTTP11)). A client _SHOULD NOT_ generate content in a DELETE request unless it is made directly to an origin server that has previously indicated, in or out of band, that such a request has a purpose and will be adequately supported.

---
**Cache**

If a successful DELETE request passes through a cache that has one or more stored responses for the target URI, those stored responses will be invalidated.

---

## References

- https://httpwg.org/specs/rfc9110.html#DELETE
