# HTTP Trailer Field

It is a header field is used in header and is used to provide a list of field names that the sender anticipates sending as trailer fields within that message. This allows a recipient to prepare for receipt of the indicated metadata before it starts processing the content.

For example, a sender might indicate that a signature will be computed as the content is being streamed and provide the final signature as a trailer field. This allows a recipient to perform the same check on the fly as it receives the content.

If an intermediary discards the trailer section in transit, this field still could provide a hint of what metadata was lost, though there is no guarantee that a sender of Trailer will always follow through by sending the named fields.

## References

- https://httpwg.org/specs/rfc9110.html#field.trailer
