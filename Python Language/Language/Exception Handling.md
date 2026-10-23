### Basic Syntax
```python
try:
    x = 1 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
else:
    print("No error occurred")   # runs only if no exception
finally:
    print("Always runs")         # cleanup — always executes
```
---
### Exception Hierarchy (most common)
|Exception|Cause|
|---|---|
|`Exception`|Base for all non-system exceptions|
|`ValueError`|Wrong value type (e.g. `int("abc")`)|
|`TypeError`|Wrong type (e.g. `"a" + 1`)|
|`KeyError`|Dict key missing|
|`IndexError`|List index out of range|
|`AttributeError`|Object has no such attribute|
|`FileNotFoundError`|File doesn't exist|
|`IOError` / `OSError`|File/IO failure|
|`ZeroDivisionError`|Divide by zero|
|`ImportError`|Module not found|
|`PermissionError`|No OS-level permission|
|`TimeoutError`|Operation timed out|
|`MemoryError`|RAM exhausted|
|`RecursionError`|Max recursion depth exceeded|
|`StopIteration`|Iterator exhausted|
|`NotImplementedError`|Abstract method not implemented|
|`RuntimeError`|Generic runtime failure|
|`ConnectionError`|Network connection failed|
---
### Catching Multiple Exceptions
```python
# Option 1 — group them
try:
    pass
except (ValueError, TypeError) as e:
    print(e)
# Option 2 — handle separately
try:
    pass
except ValueError:
    print("Bad value")
except TypeError:
    print("Bad type")
except Exception as e:
    print(f"Unexpected: {e}")   # catch-all — always put LAST
```
---
### Raising Exceptions
```python
# Raise built-in
raise ValueError("Invalid input")
# Re-raise caught exception
try:
    risky()
except Exception:
    logger.exception("Failed")
    raise                        # re-raises same exception with traceback
# Chain exceptions (Python 3+)
try:
    open("x.txt")
except FileNotFoundError as e:
    raise RuntimeError("Config missing") from e
```
---
### Custom Exceptions ✅ (Best Practice)
```python
# Define hierarchy
class AppError(Exception):
    """Base exception for your app"""
    pass
class PaymentError(AppError):
    def __init__(self, amount, reason):
        self.amount = amount
        super().__init__(f"Payment of ₹{amount} failed: {reason}")
class UPIError(PaymentError):
    pass
# Usage
try:
    raise UPIError(499, "VPA not found")
except PaymentError as e:
    print(e)   # Payment of ₹499 failed: VPA not found
```
---
### Exception + Logging (Production Pattern)
```python
import logging
logger = logging.getLogger(__name__)
try:
    process_payment()
except UPIError as e:
    logger.exception("UPI payment failed")   # logs full traceback
    raise                                     # propagate up
except Exception as e:
    logger.critical("Unexpected failure", exc_info=True)
    raise RuntimeError("System error") from e
```
---
### Context Managers (`with`) for Cleanup
```python
# Instead of try/finally for resources
with open("data.csv") as f:       # auto-closes even on exception
    data = f.read()
# Custom context manager
from contextlib import contextmanager
@contextmanager
def db_transaction(conn):
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
```
---
### Best Practices
|✅ Do|❌ Don't|
|---|---|
|Catch specific exceptions|`except Exception` as first catch|
|Use `logger.exception()` in except|Use `print()` for errors|
|Define custom exception classes|Use bare `except:` (catches even `KeyboardInterrupt`)|
|Use `raise ... from e` for chaining|Silently swallow exceptions (`except: pass`)|
|Put `finally` for resource cleanup|Catch what you can't handle|
|Re-raise if you can't recover|Overuse `try/except` as flow control|
---
### India-Specific Patterns
- In **UPI/payment integrations** (Razorpay, PayU, PhonePe SDK), always catch `ConnectionError` and `TimeoutError`separately — network flakiness is common on Indian mobile networks
- For **Aadhaar/DigiLocker API** calls, always handle `PermissionError` and custom HTTP errors — government APIs have strict rate limits and frequent downtime windows
- **DPDP Act 2023** — never log PII in exception messages (don't do `raise ValueError(f"Aadhaar {uid} invalid")`)