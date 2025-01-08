# HTTP Method Safe Property

A request method is safe it its defined semantics are read only operation. For example, [GET](get.md) method.

The purpose of distinguishing between safe and unsafe methods is to allow automated retrieval processes (spiders) and cache performance optimization (pre-fetching) to work without fear of causing harm. In addition, it allows a user agent to apply appropriate constraints on the automated use of unsafe methods when processing potentially untrusted content.

## References

- https://httpwg.org/specs/rfc9110.html#safe.methods
