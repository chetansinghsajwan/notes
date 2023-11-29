# Braced Init List

A braced-init-list is not an expression and therefore has no type, e.g. `decltype({1, 2})` is ill-formed. Having no type implies that template type deduction cannot deduce a type that matches a braced-init-list, so given the declaration `template<class T> void f(T);` the expression `f({1, 2, 3})` is ill-formed. However, the template parameter can otherwise be deduced, as is the case for `std::vector<int> v(std::istream_iterator<int>(std::cin), {})`, where the iterator type is deduced by the first argument but also used in the second parameter position. A special exception is made for type deduction using the keyword auto, which deduces any braced-init-list as std::initializer_list in copy-list-initialization.

Also because a braced-init-list has no type, [special rules for overload resolution](https://en.cppreference.com/w/cpp/language/overload_resolution#Implicit_conversion_sequence_in_list-initialization) apply when it is used as an argument to an overloaded function call.

Aggregates copy/move initialize directly from single-element braced-init-list of the same type, but non-aggregates consider initializer_list constructors first: