# Guidelines

## Formatting guidelines

### Code

- Names of classes, methods, enumerations, public fields, public properties,  
    namespaces: `PascalCase`.
- Names of local variables, parameters: `camelCase`.
- Names of private, protected, internal and protected internal fields and  
    properties: `_camelCase`.
- Naming convention is unaffected by modifiers such as const, static,  
    readonly, etc.
- For casing, a “word” is anything written without internal spaces, including  
    acronyms. For example, `MyRpc` instead of ~~`MyRPC`~~.
- Names of interfaces start with `I`, e.g. `IInterface`.

### Files

- Filenames and directory names are `PascalCase`, e.g. `MyFile.cs`.
- Where possible the file name should be the same as the name of the main  
    class in the file, e.g. `MyClass.cs`.
- In general, prefer one core class per file.

### Organization

- Modifiers occur in the following order: `public` `protected` `internal` `private` `new` `abstract` `virtual` `override` `sealed` `static` `readonly` `extern` `unsafe` `volatile` `async`.
- Namespace `using` declarations go at the top, before any namespaces. `using`  
    import order is alphabetical, apart from `System` imports which always go  
    first.
- Class member ordering:
    - Group class members in the following order:
        - Nested classes, enums, delegates and events.
        - Static, const and readonly fields.
        - Fields and properties.
        - Constructors and finalizers.
        - Methods.
    - Within each group, elements should be in the following order:
        - Public.
        - Internal.
        - Protected internal.
        - Protected.
        - Private.
    - Where possible, group interface implementations together.

### Whitespace rules

Developed from Google Java style.
- A maximum of one statement per line.
- A maximum of one assignment per statement.
- Indentation of 4 spaces, no tabs.
- Column limit: 100.
- No line break before opening brace.
- No line break between closing brace and `else`.
- Braces used even when optional.
- Space after `if`/`for`/`while` etc., and after commas.
- No space after an opening parenthesis or before a closing parenthesis.
- No space between a unary operator and its operand. One space between the  
    operator and each operand of all other operators.
- Line wrapping developed from Google C++ style guidelines, with minor  
    modifications for compatibility with Microsoft’s C# formatting tools:
    - In general, line continuations are indented 4 spaces.
    - Line breaks with braces (e.g. list initializers, lambdas, object  
        initializers, etc) do not count as continuations.
    - For function definitions and calls, if the arguments do not all fit on  
        one line they should be broken up onto multiple lines, with each  
        subsequent line aligned with the first argument. If there is not enough  
        room for this, arguments may instead be placed on subsequent lines with  
        a four space indent. The code example below illustrates this.
## Coding guidelines