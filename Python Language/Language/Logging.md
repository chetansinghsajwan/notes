### Log Levels (in order)
|Level|Use When|
|---|---|
|`DEBUG`|Detailed dev/diagnostic info|
|`INFO`|Confirming things work as expected|
|`WARNING`|Something unexpected, but app still works|
|`ERROR`|Something failed, needs attention|
|`CRITICAL`|App-breaking failure (DB down, disk full)|
---
### Basic Usage
```python
import logging
# Always use __name__, never root logger
logger = logging.getLogger(__name__)
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(name)s %(levelname)s %(message)s"
)
logger.info("App started")
logger.warning("Low memory: %s MB", free_mb)   # use % style, not f-string
logger.error("Failed to connect to DB")
# For exceptions — use .exception() not .error()
try:
    x = 1 / 0
except ZeroDivisionError:
    logger.exception("Unexpected error")  # auto-includes full traceback
```
---
### Best Practices
- **Never use** `**print()**` in production — no levels, no destinations, invisible in Docker/containers
- **Never use root logger** (`logging.info(...)`) — use `logging.getLogger(__name__)` always
- **Use** `**%s**` **style** in log messages, not f-strings — arguments are lazily evaluated (performance)
- **Use** `**logger.exception()**` inside `except` blocks — automatically captures stack trace
- **Never log sensitive data** — passwords, Aadhaar numbers, PAN, UPI IDs, tokens
- **Log to** `**stdout**` in production and let the environment (Docker, systemd) handle routing
- **Rotate logs** to avoid disk exhaustion — use `RotatingFileHandler` or external `logrotate`
---
### Popular Libraries (what the community uses)
|Library|Why Use It|Best For|
|---|---|---|
|`logging` (stdlib)|Built-in, zero deps|All projects, default choice|
|`loguru`|Dead-simple API, colorful output|Scripts, small-medium apps|
|`structlog`|Structured/JSON logging + context|Microservices, APIs|
|`opentelemetry`|Unified logs + traces + metrics|Distributed systems, cloud|
- **Loguru** is commonly cited as an alternative that makes logging nearly as simple as `print()`
- **Structured JSON logging** with `structlog` and `contextvars` is considered a modern advanced best practice
---
### Log Rotation (prevent disk full)
```python
from logging.handlers import RotatingFileHandler
handler = RotatingFileHandler(
    "app.log", maxBytes=5*1024*1024, backupCount=3  # 5MB, keep 3 files
)
logger.addHandler(handler)
```
---
### Config via File (recommended for larger apps)
```python
import logging.config, json
with open("logging.json") as f:
    logging.config.dictConfig(json.load(f))
```
Using JSON config is preferred over YAML — stricter syntax, no extra dependencies, and works consistently across dev, Docker, and cloud environments.
---
### India-Specific Tips
- If building **fintech/banking apps** (UPI, NBFC, insurance), logging is often **mandatory for RBI/SEBI compliance**— audit logs must be retained for 5–7 years
- **Never log PAN, Aadhaar, UPI IDs, or OTPs** — violates IT Act and DPDP Act 2023
- For **cloud deployments on AWS Mumbai / GCP Mumbai region** (common in Indian startups), ship logs to CloudWatch or Cloud Logging rather than local files