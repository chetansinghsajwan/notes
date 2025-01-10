# HTTP Field

A http field is a key value pair, which adds semantic meaning to the message.

- Field names are case insensitive.
- Field names are ought to be registered [HTTP Field Name Registry](/http/field-registry).

According to the specification, new fields can be introduced without changing the protocol version if their defined semantics allow them to be safely ignored by recipients that do not recognize them.

A proxy _MUST_ forward unrecognized header fields unless the field name is listed in the [Connection](https://httpwg.org/specs/rfc9110.html#field.connection) header field ([Section 7.6.1](https://httpwg.org/specs/rfc9110.html#field.connection "Connection")) or the proxy is specifically configured to block, or otherwise transform, such fields. Other recipients _SHOULD_ ignore unrecognized header and trailer fields. Adhering to these requirements allows HTTP's functionality to be extended without updating or removing deployed intermediaries.

---
**FIeld Line**

A line containing a field-name and value pair.

---
**Combined Field Value**

When a field name is only present once in a section, the combined field value for that field consists of the corresponding field line value. When a field name is repeated within a section, its combined field value consists of the list of corresponding field line values within that section, concatenated in order, with each field line value separated by a comma.

For example, this section:

```http
Example-Field: Foo, Bar
Example-Field: Baz
```

contains two field lines, both with the field name "Example-Field". The first field line has a field line value of "Foo, Bar", while the second field line value is "Baz". The field value for "Example-Field" is the list "Foo, Bar, Baz".

---


## References

- https://httpwg.org/specs/rfc9110.html#fields
