# `reinterpret_cast`

##### `reinterpret_cast` with `constexpr`

At runtime the C++ language has the concept of Undefined Behavior. Under certain (well specified) conditions, the program has Undefined Behavior, that means that it can exhibit any behavior: it can crash, it can hang forever, it can print gibberish, it can appear to work, or it can do anything. A simplified explanation of why this exists is performance.
At runtime this is a tradeoff (a compromise if you will), but it is unacceptable at compile time. If the standard would allow UB at compile time, not only it would be legal to get crashes while compiling the program or compile ad infinitum, but you could never be sure of the validity of the compiled executable.

As such, any form of `constexpr` would have to be 100% free of Undefined Behavior. No exceptions about it. No leeway.

One notorious source of UB is `reinterpret_cast`. There are very few valid uses of `reinterpret_cast`, most of them result in UB. Plus it is practically impossible to check if the use is valid. So `reinterpret_cast` is not allowed during compilation, i.e. it is not allowed in constexpr.

## References

- https://stackoverflow.com/questions/59913275/why-is-reinterpret-cast-not-constexpr
