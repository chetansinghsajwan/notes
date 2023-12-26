# Template Argument Deduction

> [!important]
> NOTE: The answer here has been borrowed from effective modern C++ with a (very) few additions of my own.

This is one of those questions that are easy to pose but difficult to answer! I remember reading an entire chapter on template type deduction, and to a rookie reader, the answer is not clear in one read either. Nevertheless, I will try to clarify it here.
One should note that there is something called _**Universal References**_ (which are not the same as references or r-value references) that influences template type deduction, and I assume readers know about l-value and r-value references.

Any ubiquitous function template definition looks like the following:

```cpp
template <typename T>
returnType function(paramType param);
```

A call to function would look somehow look like this :

```cpp
function(expression);
```

The compiler uses **expression** to determine the type of **T** and the type of **paramType**. This is so because more often **paramType** contains decorations like **const**, **const&**, **const&&**, etc. Beginners would be tempted to believe that the type **T** deduced by the compiler will be the same as the type of **expression**, i.e., the argument passed to the function, but it is not always the case. Deduction of type **T** depends both on **expression** and **paramType** . Depending on what the function parameter **paramType** is there are three cases to be considered for template type deduction:

1. **paramType** is pointer or reference but not a universal reference.
2. **paramType** is a universal reference.
3. **paramType** is neither a pointer nor a reference.

Let's take a look at each case one by one
## Case 1: paramType is a pointer or a reference but not a universal reference

Call me crazy, but this is the simplest case that can be encountered. In this case, type deduction works like this:  
(i) If _**expression**_ is a reference, then ignore the reference part  
(ii) then match _**expression's**_ pattern against _**paramType**_ to determine _**T**_
Lets take a look at an example :
```
template <typename T>
returnType function(T &param);
```
We have the following variable declarations:
```
int x = 23;               // x is int
const int const_x = x;    // const_x is const int
const int& ref_x = x;     // ref_x is a reference to x as const int
```
The deduced call for _**T**_ and _**param**_ in various calls are as follows :
```
f(x);                    //T is int, param's type is int&
f(const_x);              //T is const int, param's type is const int&
f(ref_x);                //T is const int, param's type is const int&
```
There are two points to be noted here:
(i) the compiler ignores the reference-ness for type deduction here
(ii) the const-ness becomes a part of type _**T**_  
when passing a const object or reference to a const object, and hence  
passing const objects or references to const object to functions taking  
parameter _**T&**_ is safe.
If we change the function parameter from _**T&**_ to _**const T&**_, because in this case we are assuming _**param**_ to be reference to _const_, the _**const**_-ness need not be deduced as a part of _**T**_ . Below is an example:
```
template <typename T>
returnType function(const T& param);  // param is now a ref-to-const
int x = 23;                    // same as previous
const int const_x = x;         // same as previous
const int& ref_x = x;          // same as previous
f(x);                         // T is int, paramType is const int&
f(const_x);                   // T is int, paramType is const int&
f(ref_x);                     // T is int, paramType is const int&
```
**Note**: variable 'x' is not a const argument to 'f()' but it is till deduced as a const param
If _**paramType**_ is a pointer, things will work  
fundamentally the same way as with references. There will be pointers  
instead of references. E.g., below for the sake of completeness is  
provided:
```
template <typename T>
returnType function( T* paramType);  // paramType is now a pointer
int x = 23;                      // same as before
const int *pointer_x = &x;       // pointer_x is pointer to x as const int
f(&x);                          // T is int, paramType is int*
f(pointer_x);                   // T is const int, paramType is const int*
```
For the sake of completeness I may as well post the case if **paramType** were a pointer to a constant object like the following:
```
template <typename T>
returnType function(const T* paramType);
int x = 23;                      // same as before
const int *pointer_x = &x;       // pointer_x is pointer to x as const int
f(&x);                          // T is int, paramType is const int*
f(pointer_x);                  // T is int, paramType is const int*
```
i.e., again the _**const**_-ness is not anymore deduced as a part of T
In case of r-value references, type _**T**_ and _**paramType**_ deduction follow essentially the same rules as they do in case of l-value references.
This covers most of it for the first case. Let's look at our case 2.
## Case 2: paramType is a universal reference
Universal references are declared like r-value references but take  
l-value, but what makes their behavior different is that the function  
arguments receive l-value references. Here's how the type deduction  
works for this case:
(i) If _**expression**_ is an l-value, both _**T**_ and _**paramType**_ are deduced to be l-value. (This seems strange in the face of how the code looks like because although _**paramType**_  
is declared using the syntax of r-value reference, its deduced type is  
of l-value reference.) It should be noted that this is the only case  
where _**T**_ is deduced to be a reference.
The example below clarifies my explanation:
```
template <typename T>
returnType function(T&& paramType);  // param becomes universal reference if
                                     // argument to function call is an l-value
int x = 23                     // same as previous
const int const_x = x;         // same as previous
const int& ref_x = x;          // same as previous
f(x);             // x is l-value therefore T is int&
                  // paramType is int&
f(const_x);       // const_x is l-value therefore T is const int&
                  //paramType is also const int&
f(ref_x);        // ref_x is l-value therefore T is const int&
                 // paramType is also const int&
f(23);          // 23 is r-value so T is int
                // paramType is now int&&
```
I want to be honest here and say that this doesn't explain why  
universal references work the way they do, but I think this post will  
become too lengthy if I go on to justify it here.
## Case 3: paramType is neither a pointer nor a reference
This is where pass-by-value in template occurs, which implies that  
param will be a copy of whatever is passed to the argument of the  
calling function, i.e., a completely new object, and this motivates the  
rules that govern type deduction of _**T**_ from _**expression**_ . Two points to be noted here are:
(i) ignore the _refrence_-ness in _**expression**_ , if there happens to be one.
(ii) after ignoring the _ref_-ness, ignore _const_-ness or _volatile_-ness too, i.e if present
```
template <typename T>
returnType function(T paramType);
int x = 23;
const int const_x = x;
const int& ref_x = x;
f(x);             // T and paramType are both int
f(const_x);       // T and paramType are both int here too
f(ref_x);         // T and paramType are both int again
```
Note even though const_x and ref_x are const objects which cannot be  
modified, it doesn't mean that their copies cannot be modified.  
This looks straightforward, but it gets tricker when we pass a constant  
pointer to a constant object. Let's take a look at another example:
```
template <typename T>
returnType function(T param);
const double *const dPtr = 23;  // dPtr is const pointer to const double
function(dPtr);             // passing argument of type const double *const
```
When _const_ pointer is passed by value, the _const_-ness is lost, and the pointer is copied by value, which is in sync with the type deduction rules for pass by value, but the _const_-ness of what pointer points to is preserved, and hence the _**paramType**_ will be const *double.
This might get your head spinning as it did to me when I started to  
learn about it. The best way would be to re-read it and try to code it.