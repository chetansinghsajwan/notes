---
status: completed
---

# POST vs PUT

The difference between [PUT](put.md) and [POST](post.md) is that PUT is [idempotent](http/method-property/idempotent.md), calling it once is no different from calling it several times successively. Successive identical POST requests may have additional effects, such as creating the same order several times.

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST
