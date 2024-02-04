# Standard Layout Class

A _standard-layout class_ is a class that

- has no [non-static data members](https://en.cppreference.com/w/cpp/language/data_members "cpp/language/data members") of type non-standard-layout class (or array of such types) or reference,

- has no [virtual functions](https://en.cppreference.com/w/cpp/language/virtual "cpp/language/virtual") and no [virtual base classes](https://en.cppreference.com/w/cpp/language/derived_class#Virtual_base_classes "cpp/language/derived class"),

- has the same [access control](https://en.cppreference.com/w/cpp/language/access "cpp/language/access") for all non-static data members,

- has no non-standard-layout base classes,

- only one class in the hierarchy has non-static data members, and

- Informally, none of the base classes has the same type as the first non-static data member. Or, formally: given the class as S, has no element of the set M(S) of types as a base class, where M(X) for a type X is defined as:

    - If X is a non-union class type with no (possibly inherited) non-static data members, the set M(X) is empty.
 
    - If X is a non-union class type whose first non-static data member has type X0 (where said member may be an anonymous union), the set M(X) consists of X0 and the elements of M(X0).

    - If X is a union type, the set M(X) is the union of all M(Ui) and the set containing all Ui, where each Ui is the type of the ith non-static data member of X.

    - If X is an array type with element type Xe, the set M(X) consists of Xe and the elements of M(Xe).

    - If X is a non-class, non-array type, the set M(X) is empty.

## References

- https://en.cppreference.com/w/cpp/language/classes#Standard-layout_class
