**Arithmetic Operators**
|   |   |
|---|---|
|Operator|Description|
|+|Add|
|-|Subtract|
|*|Multiply|
|/|Divide|
|%|Modulo|
**Bitwise Operators**
|   |   |
|---|---|
|Operator|Description|
|&|Bitwise AND|
|\||Bitwise OR|
|^|Bitwise exclusive OR|
### Comparison Operators
1. `**=**`
2. **`<>`** **(Not equal to)**
3. **`>`**
4. `**<**`
5. `**≥**`
6. **`≤`**
**Example**
```
SELECT * FROM Products
WHERE Price = 18;
```
### Logical Operators
1. `**ALL**`
    
    `TRUE` if all of the subquery values meet the condition.
    
    **Example**
    
    ```
    SELECT * FROM Products
    WHERE Price > ALL (SELECT Price FROM Products WHERE Price > 500);
    ```
    
2. `**AND**`
    
    `TRUE` if all the conditions separated by `AND` is `TRUE`.
    
    **Example**
    
    ```
    SELECT * FROM Customers
    WHERE City = "London" AND Country = "UK";
    ```
    
3. `**ANY**`
    
    `TRUE` if any of the subquery values meet the condition.
    
    **Example**
    
    ```
    SELECT * FROM Products
    WHERE Price > ANY (SELECT Price FROM Products WHERE Price > 50);
    ```
    
4. `**SOME**`
    
    Exactly same as `ANY` in every respect. I don’t know why it even exists.
    
5. `**BETWEEN**`
    
    `TRUE` if the operand is within the range of comparisons.
    
    **Example**
    
    ```
    SELECT * FROM Products
    WHERE Price BETWEEN 50 AND 60;
    ```
    
6. `**EXISTS**`
    
    `TRUE` if the subquery returns one or more records.
    
    **Example**
    
    ```
    SELECT * FROM Products
    WHERE EXISTS (SELECT Price FROM Products WHERE Price > 50);
    ```
    
7. `**IN**`
    
    `TRUE` if the operand is equal to one of a list of expressions.
    
    **Example**
    
    ```
    SELECT * FROM Customers
    WHERE City IN ('Paris','London');
    ```
    
8. `**LIKE**`
    
    TRUE if the operand matches a pattern.
    
    **Example**
    
    ```
    SELECT * FROM Customers
    WHERE City LIKE 's%';
    ```
    
9. `**NOT**`
    
    Displays a record if the condition(s) is `NOT` `TRUE.`
    
    **Example**
    
    ```
    SELECT * FROM Customers
    WHERE City NOT LIKE 's%';
    ```
    
10. `**OR**`
    
    `TRUE` if any of the conditions separated by `OR` is `TRUE`.
    
    **Example**
    
    ```
    SELECT * FROM Customers
    WHERE City = "London" OR Country = "UK";
    ```
    
# References

> [!info] MySQL Operators  
> W3Schools offers free online tutorials, references and exercises in all the major languages of the web.  
> [https://www.w3schools.com/mysql/mysql_operators.asp](https://www.w3schools.com/mysql/mysql_operators.asp)