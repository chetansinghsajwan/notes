# HTTP Connect Method

The **`CONNECT`** HTTP method requests that a [proxy](https://developer.mozilla.org/en-US/docs/Glossary/Proxy_server) establish a HTTP tunnel to a destination server, and if successful, blindly forward data in both directions until the tunnel is closed.

- Request does not have [body](/http/body).
- Request is not [safe](/http/requests/safe).
- Request is not [idempotent](/http/requests/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

A server must reject the request that targets an empty or invalid port number, typically by responding with a [400 (Bad Request)](/http/status/400) status code.

The recipient can establish a tunnel either by directly connecting to the server identified by the request target or, if configured to use another proxy, by forwarding the request to the next inbound proxy.

Any [2xx (Successful)](/http/status/2xx) response indicates that the sender (and all inbound proxies) will switch to tunnel mode immediately after the response header section; data received after that header section is from the server identified by the request target. Any response other than a successful response indicates that the tunnel has not yet been formed.

A tunnel is closed when a tunnel intermediary detects that either side has closed its connection: the intermediary _MUST_ attempt to send any outstanding data that came from the closed side to the other side, close both connections, and then discard any remaining data left undelivered.

## References

- https://httpwg.org/specs/rfc9110.html#CONNECT
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/CONNECT
