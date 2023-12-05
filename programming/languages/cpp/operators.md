# List Of Operators
#### Operators
|Name|![](https://www.notion.so/icons/code_gray.svg)Syntax|![](https://www.notion.so/icons/friends_gray.svg)Alias|Category|Type|Since|![](https://www.notion.so/icons/stairs_gray.svg)Priority|![](https://www.notion.so/icons/swap-horizontally_gray.svg)Associativity|![](https://www.notion.so/icons/code_gray.svg)In Class Definiton|![](https://www.notion.so/icons/code_gray.svg)Outside Class Definition|Inheritance|
|---|---|---|---|---|---|---|---|---|---|---|
|[[Addition]]|`a + b`||Arithmetic|Binary|C++|6|Left to Right|R K::**operator** +(S b);|R **operator** +(K a, S b);|Inherited|
|[[Subtraction]]|`a - b`||Arithmetic|Binary|C++|6|Left to Right|R K::**operator** -(S b);|R **operator** -(K a, S b);|Inherited|
|[[Unary Plus]]|`+a`||Arithmetic|Unary|C++|3|Right to Left|R K::**operator** +();|R **operator** +(K a);|Inherited|
|[[Unary Minus]]|`-a`||Arithmetic|Unary|C++|3|Right to Left|R K::**operator** -();|R **operator** -(K a);|Inherited|
|[[Multiplication]]|`a * b`||Arithmetic|Binary|C++|5|Left to Right|R K::**operator** *(S b);|R **operator** *(K a, S b);|Inherited|
|[[Division]]|`a / b`||Arithmetic|Binary|C++|5|Left to Right|R K::**operator** /(S b);|R **operator** /(K a, S b);|Inherited|
|[[Modulo]]|`a % b`||Arithmetic|Binary|C++|5|Left to Right|R K::**operator** %(S b);|R **operator** %(K a, S b);|Inherited|
|[[Prefix Increment]]|`++a`||Arithmetic|Unary|C++|3|Right to Left|R& K::**operator** ++();|R& **operator** ++(K& a);|Inherited|
|[[Postfix Increment]]|`a++`||Arithmetic|Unary|C++|2|Left to Right|R& K::**operator** ++(int);|R **operator** ++(K& a, int);|Inherited|
|[[Prefix Decrement]]|`--a`||Arithmetic|Unary|C++|3|Right to Left|R& K::**operator** --();|R& **operator** --(K& a);|Inherited|
|[[Postfix Decrement]]|`a--`||Arithmetic|Unary|C++|2|Left to Right|R& K::**operator** --(int);|R **operator** --(K& a, int);|Inherited|
|[[Equal To]]|`a == b`||Comparision|Binary|C++|10|Left to Right|bool K::**operator** ==(const S& b) **const**;|bool K::**operator** ==(const S& a, const S& b) **const**;|Inherited|
|[[Not Equal To]]|`a != b`|not_eq|Comparision|Binary|C++|10|Left to Right|bool K::**operator** !=(const S& b) **const**;|bool K::**operator** !=(const S& a, const S& b) **const**;|Inherited|
|[[Greater Than]]|`a > b`||Comparision|Binary|C++|9|Left to Right|bool K::**operator** >(const S& a, const S& b) **const**;|bool K::**operator** >(const S& a, const S& b) **const**;|Inherited|
|[[Less Than]]|`a < b`||Comparision|Binary|C++|9|Left to Right|bool K::**operator** <(const S& a, const S& b) **const**;|bool K::**operator** <(const S& a, const S& b) **const**;|Inherited|
|[[Greater Than or Equal To]]|`a >= b`||Comparision|Binary|C++|9|Left to Right|bool K::**operator** >=(const S& a, const S& b) **const**;|bool K::**operator** >=(const S& a, const S& b) **const**;|Inherited|
|[[Less Than or Equal To]]|`a <= b`||Comparision|Binary|C++|9|Left to Right|bool K::**operator** <=(const S& a, const S& b) **const**;|bool K::**operator** <=(const S& a, const S& b) **const**;|Inherited|
|[[Three Way Compairision]]|`a <=> b`||Comparision|Binary|C++20|8|Left to Right|auto K::**operator** <=>(const S& a, const S& b) **const**;|bool K::**operator** <=>(const S& a, const S& b) **const**;|Inherited|
|[[Logical Negation]]|`!a`|not|Logical|Unary|C++|3|Right to Left|bool K::**operator** !();|bool **operator** !(K a);|Inherited|
|[[Logical And]]|`a && b`|and|Logical|Binary|C++|14|Left to Right|bool K::**operator** &&(S b);|bool K::**operator** &&(K a, S b);|Inherited|
|[[Logical Or]]|`a \| b`|or|Logical|Binary|C++|15|Left to Right|bool K::**operator** \|(S b);|bool K::**operator** \|(K a, S b);|Inherited|
|[[Bitwise Not]]|`~a`|compl|Bitwise|Unary|C++|3|Right to Left|R K::**operator** ~();|R **operator** ~(K a);|Inherited|
|[[Bitwise And]]|`a & b`|bitand|Bitwise|Binary|C++|11|Left to Right|R K::**operator** &(S b);|R **operator** &(K a, S b);|Inherited|
|[[Bitwise Or]]|`a \| b`|bitor|Bitwise|Binary|C++|13|Left to Right|R K::**operator** \|(S b);|R **operator** \|(K a, S b);|Inherited|
|[[Bitwise Xor]]|`a ^ b`|xor|Bitwise|Binary|C++|12|Left to Right|R K::**operator** ^(S b);|R **operator** ^(K a, S b);|Inherited|
|[[Bitwise Left Shift]]|`a << b`||Bitwise|Binary|C++|7|Left to Right|R K::**operator** <<(S b);|R **operator** <<(K a, S b);|Inherited|
|[[Bitwise Right Shift]]|`a >> b`||Bitwise|Binary|C++|7|Left to Right|R K::**operator** >>(S b);|R **operator** >>(K a, S b);|Inherited|
|[[Direct Assignment]]|`a = b`||Assignment|Binary|C++|17|Right to Left|R& K::**operator** =(S b);||Hidden|
|[[Addition Assignment]]|`a += b`||Assignment|Binary|C++|17|Right to Left|R& K::**operator** +=(S b);|R& **operator** +=(K& a, S b);|Inherited|
|[[Subtraction Assignment]]|`a -= b`||Assignment|Binary|C++|17|Right to Left|R& K::**operator** -=(S b);|R& **operator** -=(K& a, S b);|Inherited|
|[[Multiplication Assignment]]|`a *= b`||Assignment|Binary|C++|17|Right to Left|R& K::**operator** *=(S b);|R& **operator** *=(K& a, S b);|Inherited|
|[[Division Assignment]]|`a /= b`||Assignment|Binary|C++|17|Right to Left|R& K::**operator** /=(S b);|R& **operator** /=(K& a, S b);|Inherited|
|[[Modulo Assignment]]|`a %= b`||Assignment|Binary|C++|17|Right to Left|R& K::**operator** %=(S b);|R& **operator** %=(K& a, S b);|Inherited|
|[[Bitwise And Assignment]]|`a &= b`|and_eq|Assignment|Binary|C++|17|Right to Left|R& K::**operator** &=(S b);|R& **operator** &=(K& a, S b);|Inherited|
|[[Bitwise Or Assignment]]|`a \|= b`|or_eq|Assignment|Binary|C++|17|Right to Left|R& K::**operator** \|=(S b);|R& **operator** \|=(K& a, S b);|Inherited|
|[[Bitwise Xor Assignement]]|`a ^= b`|xor_eq|Assignment|Binary|C++|17|Right to Left|R& K::**operator** ^=(S b);|R& **operator** ^=(K& a, S b);|Inherited|
|[[Bitwise Left Shift Assignment]]|`a <<= b`||Assignment|Binary|C++|17|Right to Left|R& K::**operator** <<=(S b);|R& **operator** <<=(K& a, S b);|Inherited|
|[[Bitwise Right Shift Assignment]]|`a >>= b`||Assignment|Binary|C++|17|Right to Left|R& K::**operator** >>=(S b);|R& **operator** >>=(K& a, S b);|Inherited|
|[[Subscript]]|`a[b]`||Member Access|Other|C++|2|Left to Right|R& K::**operator** [](S b);||Inherited|
|[[Indirection]]|`*a`||Member Access|Unary|C++|3|Right to Left|R& K::**operator** *();|R& **operator** *(K a);|Inherited|
|[[programming/languages/cpp/operators/Address Of]]|`&a`|bitand|Member Access|Unary|C++|3|Right to Left|R* K::**operator** &();|R* **operator** &(K a);|Inherited|
|[[Structure Dereference]]|`a->b`||Member Access|Binary|C++|2|Left to Right|R* K::**operator** ->();||Inherited|
|[[Structure Reference]]|`a.b`||Member Access|Binary|C++|2|Left to Right|||Inherited|
|[[Member selected by pointer to member b of object pointed to by a]]|`a->*b`||Member Access|Binary|C++|4|Left to Right|R& K::**operator** ->*(S b);|R& **operator** ->*(K a, S b);|Inherited|
|[[Member of object a selected by pointer to member b]]|`a.*b`||Member Access|Binary|C++|4|Left to Right|||Inherited|
|[[Function Call]]|`a(a1, a2)`||Other|Other|C++|2|Left to Right|R K::operator ()(...);||Inherited|
|[[Comma]]|`a, b`||Other|Binary|C++|18|Left to Right|R K::**operator** ,(S b);|R **operator** ,(K a, S b);|Inherited|
|[[Ternary]]|`a ? b : c`||Other|Other|C++|17|Right to Left|||Inherited|
|[[Scope Resolution]]|`a::b`||Other|Binary|C++|1|None|||Inherited|
|[[User Defined Literals]]|`“a”_b`||Other|Unary|C++|-1|None||R **operator** ""_b(T a);|Inherited|
|[[Size Of]]|`sizeof a sizeof(type)`||Other|Unary|C++|3|Right to Left|||Inherited|
|[[Size Of Parameter Pack]]|`sizeof…(Args)`||Other|Unary|C++|-1|None|||Inherited|
|[[Align Of]]|`alignof(type)`||Other|Unary|C++|3|Right to Left|||Inherited|
|[[Type Identification]]|`typeid(type) typeid(a)`||Other|Other|C++|2|Left to Right|||Inherited|
|[[C Style Cast]]|`(type)a`||Other|Unary|C++|3|Right to Left|K::**operator** R();||Inherited|
|[[Functional Cast]]|`type(a)`||Other|Unary|C++|2|Left to Right|||Inherited|
|[[Static Cast]]|`static_cast<type>(a)`||Other|Unary|C++|2|Left to Right|K::**operator** R();  <br>**explicit** K::**operator** R();||Inherited|
|[[Dynamic Cast]]|`dynamic_cast<type>(a)`||Other|Unary|C++|2|Left to Right|||Inherited|
|[[Const Cast]]|`const_cast<type>(a)`||Other|Unary|C++|2|Left to Right|||Inherited|
|[[Reinterpret Cast]]|`reinterpret_cast<type>(a)`||Other|Unary|C++|2|Left to Right|||Inherited|
|[[programming/languages/cpp/operators/New]]|`new type`||Other|Unary|C++|3|Right to Left|void* K::**operator** **new**(size_t a);|void* **operator** **new**(size_t x);|Inherited|
|[[New Array]]|`new type[n]`||Other|Unary|C++|3|Right to Left|void* K::**operator** **new**[](size_t a);|void* **operator** **new**[](size_t a);|Inherited|
|[[programming/languages/cpp/operators/Delete]]|`delete a`||Other|Unary|C++|3|Right to Left|void K::**operator** **delete**(void* a);|void **operator** **delete**(void* a);|Inherited|
|[[Delete Array]]|`delete[] a`||Other|Unary|C++|3|Right to Left|void K::**operator** **delete**[](void* a);|void **operator** **delete**[](void* a);|Inherited|
|[[Exception Check]]|`noexcept(a)`||Other|Other|C++|-1|None|||Inherited|
|[[Throw]]|`throw a throw`||Other|Unary|C++|16|Right to Left|||Inherited|
|[[Coroutine Await]]|`co_await a`||Coroutine|Unary|C++20|3|Right to Left|Unknwon||Inherited|
|[[Coroutine Yeild]]|`co_yield a`||Coroutine|Unary|C++20|16|Right to Left|Unknwon||Inherited|
  
  
# Operator Inheritance
## Why `operator =` is not inherited?
The assignment operator is technically inherited; however, it is always hidden by an explicitly or implicitly defined assignment operator for the derived class.
When you leave it to the compiler to define `operator =`, it defines it as follows:
```
Derived& operator = (const Derived& that)
{
    Base1::operator =(that);
    ...
    Basen::operator =(that);
    member1 = that.member1;
    ...
    membern = that.membern;
}
```
where `Base1,...,Basen` are the bases of the class (in order of specifying them in the inheritance list) and `member1, ..., membern`/ are the members of Derived (not counting the members that were inherited) in the order you declared them in the class definition.
You can use `using` declaration to pull `Base::operator =` into `Dervied` scope.
```
struct Derived: Base
{
    using Base::operator=;
};
```
# References

> [!info] Operators in C and C++  
> This is a list of operators in the C and C++ programming languages.  
> [https://en.wikipedia.org/wiki/Operators_in_C_and_C++](https://en.wikipedia.org/wiki/Operators_in_C_and_C++)  

> [!info] Expressions - cppreference.com  
>  
> [https://en.cppreference.com/w/cpp/language/expressions#Operators](https://en.cppreference.com/w/cpp/language/expressions#Operators)  

> [!info] operator= and functions that are not inherited in C++?  
> Until a test I've just made, I believed that only Constructors were not inherited in C++.  
> [https://stackoverflow.com/questions/12009865/operator-and-functions-that-are-not-inherited-in-c](https://stackoverflow.com/questions/12009865/operator-and-functions-that-are-not-inherited-in-c)  

> [!info] Assignment operator inheritance  
> There is this code:  
> [https://stackoverflow.com/questions/9161512/assignment-operator-inheritance](https://stackoverflow.com/questions/9161512/assignment-operator-inheritance)