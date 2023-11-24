`bool` type can hold one of the two values, `true` or `false`.
- `sizeof(bool)` is implementation defined, but is generally `1`.
### Boolean Literals
`true` and `false` are _prvalues_ of type `bool`.
### **Boolean conversions**
- **_src_** **→** **`bool`**
    
    **src:** A [prvalue](https://en.cppreference.com/w/cpp/language/value_category#prvalue) of integral, floating-point, unscoped enumeration, pointer, and pointer-to-member types.
    
    The value zero **(for integral, floating-point, and unscoped enumeration)** and the null pointer and the null pointer-to-member values become `false`. All other values become `true`.
    
- **`bool`** **→ dest**
    
    If `bool` value is `false`, it is converted to `0`, else `1` of **dest** type.