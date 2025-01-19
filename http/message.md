# HTTP Message

**Note:** Each major version of HTTP defines its own syntax for communicating messages. This section defines an abstract data type for HTTP messages.

A message consists of the following in this order:

- Control Data
- Header
- Content
- Trailer

Messages are expected to be processed as a stream, wherein the purpose of that stream and its continued processing is revealed while being read. Hence, control data describes what the recipient needs to know immediately, header fields describe what needs to be known before receiving content, the content (when present) presumably contains what the recipient wants or needs to fulfill the message semantics, and trailer fields provide optional metadata that was unknown prior to sending the content.

The client must retain knowledge of the request when parsing, interpreting, or caching a corresponding response. For example, responses to the [HEAD](http/method/head) method look just like the beginning of a response to [GET](https/method/get) but cannot be parsed in the same manner.

## References

- https://httpwg.org/specs/rfc9110.html#message.abstraction
