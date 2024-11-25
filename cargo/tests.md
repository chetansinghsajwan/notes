# Tests

Tests are stored in two folders `src` and `tests` folder in the root of the project. Tests in `src` folder should be unit tests and documentation tests, while the tests in your `tests` folder should be integration-style tests.

Tests can be run by running `cargo test`. This runs all tests it finds. Specific tests can be run by running `carg test <test-name>`. Cargo looks for any tests with matching name and runs them.

Along with tests, cargo runs additional checks. It compiles all the examples to check if they still compile. It also run documentation tests to check if the code samples in your documentation comments still compiles.

## References

- https://doc.rust-lang.org/cargo/guide/tests.html
