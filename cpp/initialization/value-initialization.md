This is the initialization performed when an object is constructed with an empty initializer.
## Syntax
|   |   |
|---|---|
|1.|T `**()**`|
|2.|`**new**` T `**()**`|
|3.|Class`**::**`Class`**(**`...`**)**` `**:**` member `**()**` `**{**` ... `**}**`|
|4.|T object `**{};**`|
|5.|T `**{}**`|
|6.|`**new**` T `**{}**`|
|7.|Class`**::**`Class`**(**`...`**)**` `**:**` member `**{}**` `**{**` ... `**}**`|
## Explanation
Value initialization is performed in these situations:
1,5) When a nameless temporary object is created with the initializer consisting of an empty pair of parentheses or braces.
2,6) When an object with dynamic storage duration is created by a [new-expression](https://en.cppreference.com/w/cpp/language/new) with the initializer consisting of an empty pair of parentheses or braces.
3,7) When a non-static data member or a base class is initialized using a [member initializer](https://en.cppreference.com/w/cpp/language/initializer_list) with an empty pair of parentheses or braces.
4) when a named object (automatic, static, or thread-local) is declared with the initializer consisting of a pair of braces.

> [!important]  
> In all cases, if the empty pair of braces {} is used and T is an aggregate type, aggregate-initialization is performed instead of value-initialization.  
  
> [!important]  
> If T is a class type that has no default constructor but has a constructor taking std::initializer_list, list-initialization is performed.  
## Effects
1. If `T` is a class type with no [default constructor](https://en.cppreference.com/w/cpp/language/default_constructor) or with a user-declared u[ser-provided](https://en.cppreference.com/w/cpp/language/function#User-provided_functions) or deleted default constructor, the object performs [[default-initialization]].
2. If `T` is a class type with a default constructor that is not user-declared (until C++11)neither user-provided nor deleted (since C++11) (that is, it may be a class with an implicitly-defined or defaulted default constructor), the object is [zero-initialized](https://en.cppreference.com/w/cpp/language/zero_initialization) and the semantic constraints for default-initialization are checked, and if `T` has a non-trivial default constructor, the object is [default-initialized](https://en.cppreference.com/w/cpp/language/default_initialization).
3. If `T` is an array type, each element of the array is value-initialized.
4. Otherwise, the object is [zero-initialized](https://en.cppreference.com/w/cpp/language/zero_initialization).
  
## References
[[default-initialization]]