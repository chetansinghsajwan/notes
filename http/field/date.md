---
status: completed
---

# HTTP Date Field

It is a [header field](http/header-field) that represents the date and time at which the message was originated.

The field is value is [HTTP-date](http/format/http-date).

A sender that generates a this field should generate its field value as the best available approximation of the date and time of message generation. In theory, the date ought to represent the moment just before generating the message content. In practice, a sender can generate the date value at any time during message origination.

An [origin server](http/origin-server) with a clock must generate this field in all [2xx (Successful)](http/status/2xx), [3xx (Redirection)](http/status/3xx), and [4xx (Client Error)](http/status/4xx) responses, and may generate a Date header field in [1xx (Informational)](http/status/1xx) and [5xx (Server Error)](http/status/5xx) responses.

An origin server without a clock must not generate this field.

A recipient with a clock that receives a response message without this field must record the time it was received and append a corresponding Date header field to the message's header section if it is cached or forwarded downstream.

A recipient with a clock that receives a response with invalid field value may replace that value with the time that response was received.

A user agent may send this field in a request, though generally will not do so unless it is believed to convey useful information to the server. For example, custom applications of HTTP might convey a Date if the server is expected to adjust its interpretation of the user's request based on differences between the user agent and server clocks.

## References

- https://httpwg.org/specs/rfc9110.html#field.date
