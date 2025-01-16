---
status: completed
---

# HTTP Field

A http field is a key value pair, which adds semantic meaning to the message.

**FIeld Name**: It is the name that denotes the field.
**FIeld Value**: It is the value of the field.
**FIeld Line**: It is the line containing the field.

Field names are case insensitive. They are ought to be registered HTTP Field Name Registry.

Fields that only anticipate a single member as the field value are referred to as singleton fields.

Fields that allow multiple members as the field value are referred to as list-based fields.

New fields can be introduced without changing the protocol version if their defined semantics allow them to be safely ignored by recipients that do not recognize them.

A proxy must forward unrecognized header fields unless the field name is listed in the [Connection](http/field/connection) header field or the proxy is specifically configured to block, or otherwise transform, such fields. Other recipients should ignore unrecognized header and trailer fields. Adhering to these requirements allows HTTP's functionality to be extended without updating or removing deployed intermediaries.

A sender must not generate multiple field lines with the same field name in a message, unless that field's definition allows multiple field line values to be recombined.

**Note:** In practice, the [Set-Cookie](/http/field/set-cookie) header field often appears in a response message across multiple field lines and does not use the list syntax, violating the above requirements on multiple field lines with the same field name. Since it cannot be combined into a single field value, recipients ought to handle "Set-Cookie" as a special case while processing fields.

---
**Combined Field Value**

It is the final value for a field by combining all the occurrences of field line with the same field name within a section.

If a field name is only present once in a section, the combined field value for that field is that value of that field itself.

If a field name is repeated within a section, the combined field value consists of the list of corresponding values within that section, concatenated in order, with each field line value separated by a comma and optional space.

For example, this section:

```http
Example-Field: Foo, Bar
Example-Field: Baz
```

contains two field lines, both with the field name `Example-Field`. The first field line has a field line value of `Foo, Bar`, while the second field line value is `Baz`. So the combined field value for `Example-Field` is the list `Foo, Bar, Baz`.

---

#### Field Length

HTTP does not place a predefined limit on the length of each field line, field value, or on the length of a header or trailer section as a whole

A server that receives a request header field line, field value, or set of fields larger than it wishes to process must respond with an appropriate [4xx (Client Error)](/http/status/4xx) status code. Ignoring such header fields would increase the server's vulnerability to [request smuggling attacks](/http/security/request-smuggling-attack).

## References

- https://httpwg.org/specs/rfc9110.html#fields
