A `**@staticmethod**` is another built-in Python decorator used to define a method inside a class.
However, unlike regular methods or class methods, a static method is completely isolated—**it does not receive any automatic first argument**. It gets neither the object instance (`self`) nor the abstract class pointer (`cls`).
It behaves exactly like a plain, regular Python function, but it lives inside the class's namespace for clean organization.
### The Anatomy of a Static Method
To create a static method, you place the `@staticmethod` decorator directly above your function definition:
```python
class DataUtility:
    @staticmethod
    def clean_whitespace(text: str) -> str:
        # 🎯 Pure function logic: No self, no cls.
        # It only cares about the inputs passed directly into it.
        return str(text).strip()
```
### Calling the Method
Just like class methods, you can call them directly using the class name without creating an object first:
Python
```
# 🚀 Works instantly anywhere in your pipeline framework
cleaned_val = DataUtility.clean_whitespace("  my_column_name  ")
print(cleaned_val)  # Output: my_column_name
```
### The Primary Use Case in Production: Isolated Helpers
Static methods are used for **utility or helper functions** that perform a generic mathematical or structural calculation.
If a function performs an operation that is highly relevant to your class's domain, but it doesn't need to read or modify any class variables or instance attributes, make it a `@staticmethod`.
Python
```
class CurrencyConverter:
    # Static Field
    BASE_CURRENCY = "INR"
    @staticmethod
    def calculate_tax(amount: float, tax_rate: float) -> float:
        """Isolated helper math calculation."""
        # 🚀 It doesn't need to know BASE_CURRENCY or instance states to do this math
        return amount * (tax_rate / 100)
```
### Dynamic Checklist: Which Decorator Should You Use?
To choose the right method type for your pipeline wrappers, ask yourself what data the method needs to touch:
|   |   |   |
|---|---|---|
|**If your method needs to...**|**Use this Decorator**|**First Argument**|
|Read or modify **instance variables** (`self.df`, `self._cols`)|**No Decorator** (Regular Method)|`self`|
|Access **class configuration constants** (`cls.AUTO_FORMATS`) or instantiate new objects|`**@classmethod**`|`cls`|
|Perform an **isolated utility computation** using _only_ the explicit arguments passed to it|`**@staticmethod**`|_None_|
### Why not just use a regular function outside the class?
You could! A `@staticmethod` behaves identically to a regular function defined at the top level of your script file.
The only reason we put it inside the class is for **logical grouping (encapsulation)**. It tells other engineers using your framework: _"This utility function belongs specifically to this data cleaning module's ecosystem."_