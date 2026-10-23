Docker Compose is a tool for defining and running multi-container applications.
- Configuration is written in yaml.
- The default path for a Compose file is `**compose.yaml**` (preferred) or `**compose.yml**` that is placed in the working directory. Compose also supports `**docker-compose.yaml**` and `**docker-compose.yml**` for backwards compatibility of earlier versions. If both files exist, Compose prefers the canonical `**compose.yaml**`.
# ==Compose File Format==
A compose file has 7 top level objects:
## `version`
## `name`
## `services`
### `command`
Overrides the default command declared by the container image, for example by Dockerfile's `**CMD**`.
If the value is `**null**`, the default command from the image is used.
If the value is `**[]**` (empty list) or `**''**` (empty string), the default command declared by the image is ignored, or in other words overridden to be empty.
### `environment`
The `**environment**` attribute defines environment variables set in the container. `**environment**` can use either an array or a map.
Any boolean values; true, false, yes, no, should be enclosed in quotes to ensure they are not converted to True or False by the YAML parser.
When both `**env_file**` and `**environment**` are set for a service, values set by `**environment**` have precedence.
For example,
```yaml
environment:
  RACK_ENV: development
  SHOW: "true"
  USER_INPUT:
```
### `**restart**`
Defines the policy that the platform applies on container termination.
- `**no**`: The default restart policy. It does not restart the container under any circumstances.
- `**always**`: The policy always restarts the container until its removal.
- `**on-failure[:max-retries]**`: The policy restarts the container if the exit code indicates an error. Optionally, limit the number of restart retries the Docker daemon attempts.
- `**unless-stopped**`: The policy restarts the container irrespective of the exit code but stops restarting when the service is stopped or removed.
## `networks`
## `volumes`
## `configs`
## `secrets`
# ==References==

> [!info] Compose file reference  
> Find the latest recommended version of the Docker Compose file format for defining multi-container applications.  
> https://docs.docker.com/reference/compose-file