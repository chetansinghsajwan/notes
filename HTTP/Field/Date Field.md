It is a [[Header Field]] that represents the date and time at which the message was originated.
The field is value is [[Date Time Format|HTTP-date]].
A sender that generates a this field should generate its field value as the best available approximation of the date and time of message generation. In theory, the date ought to represent the moment just before generating the message content. In practice, a sender can generate the date value at any time during message origination.
An [[Origin Server]] with a clock must generate this field in all [[2xx Class|HTTP Status Code 2xx Class]], 3xx, and [[4xx Class|HTTP Status Code 4xx Class]] responses, and may generate a Date header field in [[1xx Class|HTTP Status Code 1xx Class]] and [[5xx Class|HTTP Status Code 5xx Class]] responses.
An origin server without a clock must not generate this field.
A recipient with a clock that receives a response message without this field must record the time it was received and append a corresponding Date header field to the message’s header section if it is cached or forwarded downstream.
A recipient with a clock that receives a response with invalid field value may replace that value with the time that response was received.
A user agent may send this field in a request, though generally will not do so unless it is believed to convey useful information to the server. For example, custom applications of HTTP might convey a Date if the server is expected to adjust its interpretation of the user’s request based on differences between the user agent and server clocks.
## References
- https://httpwg.org/specs/rfc9110.html\#field.date