# Trivially Copyable Class

It is a class that:

- has at least one eligible [copy constructor](https://en.cppreference.com/w/cpp/language/copy_constructor#Eligible_copy_constructor "cpp/language/copy constructor"), [move constructor](https://en.cppreference.com/w/cpp/language/move_constructor#Eligible_move_constructor "cpp/language/move constructor"), [copy assignment operator](https://en.cppreference.com/w/cpp/language/copy_assignment#Eligible_copy_assignment_operator "cpp/language/copy assignment"), or [move assignment operator](https://en.cppreference.com/w/cpp/language/move_assignment#Eligible_move_assignment_operator "cpp/language/move assignment"),
- each eligible copy constructor is [trivial](https://en.cppreference.com/w/cpp/language/copy_constructor#Trivial_copy_constructor "cpp/language/copy constructor")
- each eligible move constructor is [trivial](https://en.cppreference.com/w/cpp/language/move_constructor#Trivial_move_constructor "cpp/language/move constructor")
- each eligible copy assignment operator is [trivial](https://en.cppreference.com/w/cpp/language/copy_assignment#Trivial_copy_assignment_operator "cpp/language/copy assignment")
- each eligible move assignment operator is [trivial](https://en.cppreference.com/w/cpp/language/move_assignment#Trivial_move_assignment_operator "cpp/language/move assignment"), and
- has a non-deleted [trivial destructor](https://en.cppreference.com/w/cpp/language/destructor#Trivial_destructor "cpp/language/destructor").

## References

- https://en.cppreference.com/w/cpp/language/classes
