### Libraries Overview
|Library|Type|Best For|Install|
|---|---|---|---|
|`urllib`|Built-in|Basic, no dependencies|—|
|`requests`|3rd party|Most popular, simple REST|`pip install requests`|
|`httpx`|3rd party|Async + sync, modern|`pip install httpx`|
|`aiohttp`|3rd party|Pure async, high performance|`pip install aiohttp`|

> **Community choice:** `requests` for sync, `httpx` for async (2025 trend)
---
### `requests` — Most Common
```python
import requests
# GET
response = requests.get(
    "https://api.example.com/users",
    params={"page": 1, "limit": 10},        # query params
    headers={"Authorization": "Bearer TOKEN"},
    timeout=10                                # always set timeout!
)
response.status_code     # 200
response.json()          # parse JSON
response.text            # raw text
response.headers         # response headers
# POST
response = requests.post(
    "https://api.example.com/users",
    json={"name": "Rahul", "city": "Mumbai"},  # auto sets Content-Type
    headers={"Authorization": "Bearer TOKEN"},
    timeout=10
)
# PUT / PATCH / DELETE
requests.put(url, json=data, timeout=10)
requests.patch(url, json=data, timeout=10)
requests.delete(url, timeout=10)
```
---
### All HTTP Methods
|Method|Purpose|requests call|
|---|---|---|
|`GET`|Fetch data|`requests.get(url)`|
|`POST`|Create resource|`requests.post(url, json=data)`|
|`PUT`|Replace resource|`requests.put(url, json=data)`|
|`PATCH`|Partial update|`requests.patch(url, json=data)`|
|`DELETE`|Delete resource|`requests.delete(url)`|
|`HEAD`|Headers only|`requests.head(url)`|
---
### Exception Handling (Production Must)
```python
import requests
from requests.exceptions import (
    Timeout, ConnectionError, HTTPError, RequestException
)
try:
    response = requests.get(url, timeout=10)
    response.raise_for_status()   # raises HTTPError for 4xx/5xx
    data = response.json()
except Timeout:
    print("Request timed out")          # common on Indian mobile networks!
except ConnectionError:
    print("No internet / DNS failed")
except HTTPError as e:
    print(f"HTTP error: {e.response.status_code}")
except RequestException as e:
    print(f"Something went wrong: {e}")
```
---
### Session (Reuse Connections — Better Performance)
```python
# Use Session when making multiple calls to same host
with requests.Session() as session:
    session.headers.update({"Authorization": "Bearer TOKEN"})
    session.timeout = 10
    r1 = session.get("/users")
    r2 = session.get("/orders")
    r3 = session.post("/payment", json=payload)
```
---
### Async with `httpx` (Modern Approach)
```python
import httpx
import asyncio
async def fetch_data():
    async with httpx.AsyncClient(timeout=10) as client:
        response = await client.get(
            "https://api.example.com/data",
            headers={"Authorization": "Bearer TOKEN"}
        )
        response.raise_for_status()
        return response.json()
# Run
data = asyncio.run(fetch_data())
```
---
### Retry Logic (Production Essential)
```python
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry
retry_strategy = Retry(
    total=3,                              # 3 retries
    backoff_factor=1,                     # wait 1s, 2s, 4s
    status_forcelist=[429, 500, 502, 503, 504]  # retry on these
)
adapter = HTTPAdapter(max_retries=retry_strategy)
session = requests.Session()
session.mount("https://", adapter)
session.mount("http://", adapter)
response = session.get(url, timeout=10)
```
---
### Calling Indian APIs (Real Examples)
```python
# Razorpay — most used payment gateway in India
import requests
response = requests.post(
    "https://api.razorpay.com/v1/orders",
    auth=("rzp_key_id", "rzp_secret"),     # Basic auth
    json={"amount": 49900, "currency": "INR", "receipt": "order_1"},
    timeout=10
)
# GST verification API (via RapidAPI / Govt sandbox)
response = requests.get(
    "https://api.gstin.ai/api/v0.3.0/commonapi",
    params={"gstin": "27AAPFU0939F1ZV"},
    headers={"authtoken": "YOUR_TOKEN"},
    timeout=10
)
# DigiLocker / Aadhaar APIs — always use HTTPS, never log response body
```
---
### Best Practices
|✅ Do|❌ Don't|
|---|---|
|Always set `timeout`|Leave timeout as None (hangs forever)|
|Use `raise_for_status()`|Manually check `status_code == 200`|
|Use `Session` for multiple calls|Create new connection every request|
|Add retry with backoff|Retry immediately in a loop|
|Log request ID / trace ID|Log full request body (may have secrets)|
|Store API keys in env vars|Hardcode API keys in source|
|Validate response schema|Blindly trust API response structure|
```python
# Store keys safely
import os
API_KEY = os.environ.get("RAZORPAY_KEY")   # never hardcode!
```
---
### Env Vars Pattern (Standard in India's Startups)
```python
# .env file (use python-dotenv)
# RAZORPAY_KEY=rzp_live_xxx
# GST_API_TOKEN=xxx
from dotenv import load_dotenv
import os
load_dotenv()
key = os.getenv("RAZORPAY_KEY")
```