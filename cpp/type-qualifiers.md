# Type Qualifiers

Type qualifiers in C++ add extra properties to the variable like a variable being a constant or a volatile.
Type qualifiers express the way in which a variable is accessed or where a variable is stored in the memory by keeping the meaning or interpretation of the variable same. In a way, type qualifiers add more refinement to variables.
In C++, a type qualifier is specified just before the type specifier (data type) of the variable.
**Type qualifiers in C++ are classified as shown below:**
## Const

Type specifier “const” is to define the objects of type const. A const object or variable cannot be modified once declared. If an attempt is made to modify const object or variable, then the compiler raises an error. We have already seen constants/literal in our previous tutorial.
The definition of constants using ‘const’ keyword corresponds to the type qualifier ‘const’.
## Volatile

The type qualifier “volatile” means that the value of the variable marked volatile may be changed in other ways that are not specified by the program. The variables that are volatile change usually due to some external factors and not necessarily because of the program. In other words, they are volatile in nature.
**For Example,** a variable that reads the temperature in the real world can be made volatile as the reading temperature may not be completely controlled by the program.
## Mutable

“mutable” type qualifier makes the members or variable modifiable.
The mutable qualifier is usually applied to non-static class members of non-const and non-reference type. As per specific situations, we might need some variables to remain immutable (cannot be changed) and some variables to be mutable. This type of qualifier is of much help when we want mutable characteristics.

## References

- https://www.softwaretestinghelp.com/storage-classes-in-cpp/#Type_Qualifiers_In_C

- https://prepinsta.com/c-plus-plus/type-qualifiers