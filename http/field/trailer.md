# HTTP Trailer Field

This field is used to provide a list of field names that the sender anticipates sending as trailer fields within that message. This allows a recipient to prepare for receipt of the indicated metadata before it starts processing the content.

For example, a sender might indicate that a signature will be computed as the content is being streamed and provide the final signature as a trailer field. This allows a recipient to perform the same check on the fly as it receives the content.

A sender that intends to generate one or more trailer fields in a message _SHOULD_ generate a [Trailer](https://httpwg.org/specs/rfc9110.html#field.trailer) header field in the header section of that message to indicate which fields might be present in the trailers.

If an intermediary discards the trailer section in transit, the [Trailer](https://httpwg.org/specs/rfc9110.html#field.trailer) field could provide a hint of what metadata was lost, though there is no guarantee that a sender of Trailer will always follow through by sending the named fields.

## References

- https://httpwg.org/specs/rfc9110.html#field.trailer
