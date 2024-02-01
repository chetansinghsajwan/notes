# Types

## Type classification

The C++ type system consists of the following types:

- [[fundamental-types]].
    - `void` type
    - `std::nullptr_t` type
    - Arithmetic Types
        - Integral Types / Integer Types
            - `bool` type
            - Character Types
                - Narrow Character Types
                    - Ordinary Character Types
                        
                        `char`, `char16_t`, `char32_t`
                        
                    - `char8_t` type
                - Wide Character Types
                    
                    `wchar_t`
                    
            - Signed Integer Types
                - Standard Signed Integer Types
                    
                    `signed int`, `signed short int`, `signed long int`, `signed long long int`
                    
                - Extended Signed Integer Types **(implementation-defined)**
            - Unsigned Integer Types
                - Standard Unsigned Integer Types
                    
                    `unsigned int`, `unsigned short int`, `unsigned long int`, `unsigned long long int`
                    
                - Extended Unsigned Integer Types **(implementation-defined)**
        - Floating Point Types
            - Standard Floating Point Types
            - Extended Floating Point Types
                - Fixed Width Floating Point Types
                - Extended Floating Point Types **(implementation-defined)**
- Compound Types
    - Reference Types
        - LValue Reference Types
            - lvalue reference to object types;
            - lvalue reference to function types
        - [rvalue reference types](https://en.cppreference.com/w/cpp/language/reference#Rvalue_references) (see also [std::is_rvalue_reference](https://en.cppreference.com/w/cpp/types/is_rvalue_reference)):
            - rvalue reference to object types
            - rvalue reference to function types
    - [pointer types](https://en.cppreference.com/w/cpp/language/pointer#Pointers) (see also [std::is_pointer](https://en.cppreference.com/w/cpp/types/is_pointer)):
        - [pointer-to-object types](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_objects);
        - [pointer-to-function types](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_functions);
    - [pointer-to-member types](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_members) (see also [std::is_member_pointer](https://en.cppreference.com/w/cpp/types/is_member_pointer)):
        - [pointer-to-data-member](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_data_members) types (see also [std::is_member_object_pointer](https://en.cppreference.com/w/cpp/types/is_member_object_pointer));
        - [pointer-to-member-function](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_member_functions) types (see also [std::is_member_function_pointer](https://en.cppreference.com/w/cpp/types/is_member_function_pointer))
    - [array types](https://en.cppreference.com/w/cpp/language/array) (see also [std::is_array](https://en.cppreference.com/w/cpp/types/is_array));
    - [function types](https://en.cppreference.com/w/cpp/language/function) (see also [std::is_function](https://en.cppreference.com/w/cpp/types/is_function));
    - [enumeration types](https://en.cppreference.com/w/cpp/language/enum) (see also [std::is_enum](https://en.cppreference.com/w/cpp/types/is_enum));
        - [unscoped enumeration types](https://en.cppreference.com/w/cpp/language/enum#Unscoped_enumerations);
        - [scoped enumeration types](https://en.cppreference.com/w/cpp/language/enum#Scoped_enumerations) (see also [std::is_scoped_enum](https://en.cppreference.com/w/cpp/types/is_scoped_enum));
    - [class types](https://en.cppreference.com/w/cpp/language/class):
        - non-union types (see also [std::is_class](https://en.cppreference.com/w/cpp/types/is_class));
        - [union types](https://en.cppreference.com/w/cpp/language/union) (see also [std::is_union](https://en.cppreference.com/w/cpp/types/is_union)).