# POST vs PUT

The difference between [`PUT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT) and `POST` is that `PUT` is [idempotent](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent): calling it once is no different from calling it several times successively (there are no _side_ effects). Successive identical `POST` requests may have additional effects, such as creating the same order several times.

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST
