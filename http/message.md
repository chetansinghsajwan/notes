# HTTP Message

Each major version of HTTP defines its own syntax for communicating messages. This section defines an abstract data type for HTTP messages.

A message consists of the following in this order:

- control data to describe and route the message
- a header section too convey additional information
- a potentially unbounded stream of content
- a trailer section too convey additional information

## References

- https://httpwg.org/specs/rfc9110.html#message.abstraction
