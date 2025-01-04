# HTTP Post Method

This method is used to send data to the server. The data is stored in the requests body and the type of data is represented by [`Content-Type`](/http/headers/content-type) header.

- Request has [body](/http/body).
- Request is not [safe](/http/requests/safe).
- Request is not [impudent](/http/requests/impudent).
- Request is [cacheable](/http/requests/cacheable) only if freshness information is included.

---
TODO: **What the hell is this?**

When the `POST` request is sent following a [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch "fetch()") call, or for any other reason than an HTML form, the body can be any type. As described in the HTTP 1.1 specification, `POST` is designed to allow a uniform method to cover the following functions:

---

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST
- https://httpwg.org/specs/rfc9110.html
