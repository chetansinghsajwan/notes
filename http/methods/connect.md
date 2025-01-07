# HTTP Connect Method

The **`CONNECT`** HTTP method requests that a [proxy](https://developer.mozilla.org/en-US/docs/Glossary/Proxy_server) establish a HTTP tunnel to a destination server, and if successful, blindly forward data in both directions until the tunnel is closed.

- Request does not have [body](/http/body).
- Request is not [safe](/http/requests/safe).
- Request is not [idempotent](/http/requests/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

A server must reject the request that targets an empty or invalid port number, typically by responding with a [400 (Bad Request)](/http/status/400) status code.

The recipient can establish a tunnel either by directly connecting to the server identified by the request target or, if configured to use another proxy, by forwarding the request to the next inbound proxy.

## References

- https://httpwg.org/specs/rfc9110.html#CONNECT
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/CONNECT
