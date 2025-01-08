# HTTP Connect Method

The **`CONNECT`** HTTP method requests that the recipient establish a tunnel to the destination server, and if successful, blindly forward data in both directions until the tunnel is closed.

- Request does not have [body](/http/body).
- Request is not [safe](/http/requests/safe).
- Request is not [idempotent](/http/requests/idempotent).
- Request is not [cacheable](/http/requests/cacheable).

CONNECT uses a special form of request target, unique to this method, consisting of only the host and port number of the tunnel destination, separated by a colon. There is no default port; a client _MUST_ send the port number even if the CONNECT request is based on a URI reference that contains an authority component with an elided port ([Section 4.1](https://httpwg.org/specs/rfc9110.html#uri.references "URI References")). For example,

```
CONNECT server.example.com:80 HTTP/1.1
Host: server.example.com
```

A server must reject the request that targets an empty or invalid port number, typically by responding with a [400 (Bad Request)](/http/status/400) status code.

The recipient can establish a tunnel either by directly connecting to the server identified by the request target or, if configured to use another proxy, by forwarding the request to the next inbound proxy.

Any [2xx (Successful)](/http/status/2xx) response indicates that the sender (and all inbound proxies) will switch to tunnel mode immediately after the response header section; data received after that header section is from the server identified by the request target. Any response other than a successful response indicates that the tunnel has not yet been formed.

A tunnel is closed when a tunnel intermediary detects that either side has closed its connection: the intermediary _MUST_ attempt to send any outstanding data that came from the closed side to the other side, close both connections, and then discard any remaining data left undelivered.

A server _MUST NOT_ send any [Transfer-Encoding](https://httpwg.org/specs/rfc9112.html#field.transfer-encoding) or [Content-Length](https://httpwg.org/specs/rfc9110.html#field.content-length) header fields in a [2xx (Successful)](https://httpwg.org/specs/rfc9110.html#status.2xx) response to CONNECT. A client _MUST_ ignore any Content-Length or Transfer-Encoding header fields received in a successful response to CONNECT.

A CONNECT request message does not have body. The interpretation of data sent after the header section of the CONNECT request message is specific to the version of HTTP in use.

## References

- https://httpwg.org/specs/rfc9110.html#CONNECT
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/CONNECT
