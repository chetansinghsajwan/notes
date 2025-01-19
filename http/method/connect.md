---
status: completed
---

# HTTP Connect Method

The **`CONNECT`** HTTP method requests that the recipient establish a tunnel to the destination server, and if successful, blindly forward data in both directions until the tunnel is closed.

- Request does not have [body](/http/body).
- Request is not [safe](/http/method-property/safe).
- Request is not [idempotent](http/method-property/idempotent).
- Request is not [cacheable](/http/method-property/cacheable).

This method requires the target to be the address of the host in the form `host:port`. There is no default port. the client must send the port number. For example,

```http
CONNECT server.example.com:80 HTTP/1.1
```

A server must reject the request that targets an empty or invalid port number, typically by responding with a [400 (Bad Request)](400.md) status code.

The recipient can establish a tunnel either by directly connecting to the server identified by the request target or, if configured to use another proxy, by forwarding the request to the next inbound proxy.

Any [2xx (Successful)](2xx.md) response indicates that the sender (and all inbound proxies) will switch to tunnel mode immediately after the response header section; data received after that header section is from the server identified by the request target. Any response other than a successful response indicates that the tunnel has not yet been formed.

A tunnel is closed when a tunnel intermediary detects that either side has closed its connection: the intermediary _MUST_ attempt to send any outstanding data that came from the closed side to the other side, close both connections, and then discard any remaining data left undelivered.

A server must not send any [Transfer-Encoding](/http/field/transfer-encoding) or [Content-Length](/http/field/content-length) header fields in a [2xx (Successful)](2xx.md) response to CONNECT. The client must also ignore those fields received in a successful response to CONNECT.

A CONNECT request message does not have body. The interpretation of data sent after the header section of the CONNECT request message is specific to the version of HTTP in use.

## References

- https://httpwg.org/specs/rfc9110.html#CONNECT
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/CONNECT
