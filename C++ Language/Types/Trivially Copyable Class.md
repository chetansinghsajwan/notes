It is a class that:
- has at least one eligible [copy constructor](https://en.cppreference.com/w/cpp/language/copy_constructor#Eligible_copy_constructor), [move constructor](https://en.cppreference.com/w/cpp/language/move_constructor#Eligible_move_constructor), [copy assignment operator](https://en.cppreference.com/w/cpp/language/copy_assignment#Eligible_copy_assignment_operator), or [move assignment operator](https://en.cppreference.com/w/cpp/language/move_assignment#Eligible_move_assignment_operator),
- each eligible copy constructor is [trivial](https://en.cppreference.com/w/cpp/language/copy_constructor#Trivial_copy_constructor)
- each eligible move constructor is [trivial](https://en.cppreference.com/w/cpp/language/move_constructor#Trivial_move_constructor)
- each eligible copy assignment operator is [trivial](https://en.cppreference.com/w/cpp/language/copy_assignment#Trivial_copy_assignment_operator)
- each eligible move assignment operator is [trivial](https://en.cppreference.com/w/cpp/language/move_assignment#Trivial_move_assignment_operator), and
- has a non-deleted [trivial destructor](https://en.cppreference.com/w/cpp/language/destructor#Trivial_destructor).
## References
- https://en.cppreference.com/w/cpp/language/classes