# Git Cache Credential Helper

Credential helper to temporarily store passwords in memory.

This command caches credentials for use by future Git programs. The stored credentials are kept in memory of the cache-daemon process (instead of being written to a file) and are forgotten after a configurable timeout. Credentials are forgotten sooner if the cache-daemon dies, for example if the system restarts. The cache is accessible over a Unix domain socket, restricted to the current user by filesystem permissions.

If you would like the daemon to exit early, forgetting all cached credentials before their timeout, you can issue an `exit` action:

```shell
git credential-cache exit
```

## References

- https://git-scm.com/docs/git-credential-cache
