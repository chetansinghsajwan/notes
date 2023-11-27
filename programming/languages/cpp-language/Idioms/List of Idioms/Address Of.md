### **Intent**
Find the address of an object of a class that has an overloaded unary ampersand (&) operator.
### **Motivation**
C++ allows overloading of the unary ampersand (&) operator for class types. The return type of such an operator need not be the actual address of the object. Intentions of such a class are highly debatable, but the language allows it nevertheless. The address-of idiom is a way to find the real address of an object irrespective of the overloaded unary ampersand operator and its access protection.
In the example below, the `main` function fails to compile because the `operator &` of the class `nonaddressable` is private. Even if it were accessible, a conversion from its return type `double` to a pointer would not have been possible or meaningful.
```
class nonaddressable
{
public:
    typedef double useless_type;
private:
    useless_type operator&() const;
};
int main()
{
  nonaddressable na;
  nonaddressable * naptr = &na;  // Error: operator & of type nonadressable is private.
}
```
### **Solution and Sample Code**
The Address-of idiom retrieves the address of an object using a series of casts.
```
template <class T>
T * addressof(T & v)
{
  return reinterpret_cast<T *>(& const_cast<char&>(reinterpret_cast<const volatile char &>(v)));
}
int main()
{
  nonaddressable na;
  nonaddressable * naptr = addressof(na);  // No more compiler error.
}
```