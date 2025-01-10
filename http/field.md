# HTTP Field

A http field is a key value pair, which adds semantic meaning to the message.

- Field names are case insensitive.
- Field names are ought to be registered [HTTP Field Name Registry](/http/field-registry).

According to the specification, new fields can be introduced without changing the protocol version if their defined semantics allow them to be safely ignored by recipients that do not recognize them.

A proxy _MUST_ forward unrecognized header fields unless the field name is listed in the [Connection](https://httpwg.org/specs/rfc9110.html#field.connection) header field ([Section 7.6.1](https://httpwg.org/specs/rfc9110.html#field.connection "Connection")) or the proxy is specifically configured to block, or otherwise transform, such fields. Other recipients _SHOULD_ ignore unrecognized header and trailer fields. Adhering to these requirements allows HTTP's functionality to be extended without updating or removing deployed intermediaries.

## References

- https://httpwg.org/specs/rfc9110.html#fields
