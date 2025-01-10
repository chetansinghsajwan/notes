# HTTP Date Time Format

This format is referred as "HTTP-date".

It is case sensitive.

HTTP used to use 3 formats:

- IMF-fixdate
- RFC 850
- ANSI C's asctime()

IMF-fixdate format is defined [here](/http/formats/imf-fixdate).
RFC 850 uses this syntax, "Sunday, 06-Nov-94 08:49:37 GMT".
ANSI C's asctime() uses this syntax, "Sun Nov  6 08:49:37 1994"

Since 1995, HTTP has set IMF-fixdate as the preferred format for datetime. However, HTTP continues to support the other two formats to support legacy implementations.

A recipient that parses a timestamp value in an HTTP field must accept all three HTTP-date formats.

When a sender generates a field that contains one or more timestamps defined as HTTP-date, the sender must generate those timestamps in the IMF-fixdate format.

## References

- https://httpwg.org/specs/rfc9110.html#http.date
