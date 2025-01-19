# HTTP Message

Each major version of HTTP defines its own syntax for communicating messages. This section defines an abstract data type for HTTP messages.

A message consists of the following:

- control data to describe and route the message,
- a headers lookup table of name/value pairs for extending that control data and conveying additional information about the sender, message, content, or context,
- a potentially unbounded stream of content, and
- a trailers lookup table of name/value pairs for communicating information obtained while sending the content.

## References

- 