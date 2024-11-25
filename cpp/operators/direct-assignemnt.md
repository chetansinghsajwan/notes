# Direct Assignment Operator

**category**:: assignment
**associativity**:: right-to-left
**in-class-definition**:: `R& K::operator =(S b);`
**inheritance**:: hidden
**priority**:: 17
**syntax**:: `a = b`
**type**:: binary

# Direct Assignment

## Why this operator is not inherited?
^inheritance

The assignment operator is technically inherited; however, it is always hidden by an explicitly or implicitly defined assignment operator for the derived class.
When you leave it to the compiler to define `operator =`, it defines it as follows:

```cpp
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