Zero initialization is the setting of a variable to a zero value implicitly converted to the type:
- Numeric variables are initialized to 0 (or 0.0, or 0.0000000000, etc.).
- Char variables are initialized to `'\0'`.
- Pointers are initialized to `**nullptr**`.
- Arrays, [POD](https://learn.microsoft.com/en-us/cpp/standard-library/is-pod-class?view=msvc-170) classes, structs, and unions have their members initialized to a zero value.
Zero initialization is performed at different times:
- At program startup, for all named variables that have static duration. These variables may later be initialized again.
- During value initialization, for scalar types and POD class types that are initialized by using empty braces.
- For arrays that have only a subset of their members initialized.
Here are some examples of zero initialization:
```
struct my_struct
{
    int i;
    char c;
};
// zero-initialized to 0
int i0;
int main()
{
		// zero-initialized to 0.000000000
    static float f1;
		// zero-initialized to 0.00000000000000000
    double d{};
		// initialized to nullptr
    int* ptr{};
		// the third char is initialized to '\0'
    char s_array[3]{'a', 'b'};
		// the fourth and fifth ints are initialized to 0
    int int_array[5] = { 8, 9, 10 };
		// i = 0, c = '\0'
    my_struct a_struct{};
}
```