# Constraints

SQL constraints are used to specify rules for the data in a table.
Constraints are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the table. If there is any violation between the constraint and the data action, the action is aborted.
Constraints can be column level or table level. Column level constraints apply to a column, and table level constraints apply to the whole table.
The following constraints are commonly used in SQL:
[[not-null]]
[[unique]]
[[primary-key]]
[[foreign-key]]
  
- [`NOT NULL`](https://www.w3schools.com/sql/sql_notnull.asp) - Ensures that a column cannot have a NULL value
- [`UNIQUE`](https://www.w3schools.com/sql/sql_unique.asp) - Ensures that all values in a column are different
- [`PRIMARY KEY`](https://www.w3schools.com/sql/sql_primarykey.asp) - A combination of a `NOT NULL` and `UNIQUE`. Uniquely identifies each row in a table
- [`FOREIGN KEY`](https://www.w3schools.com/sql/sql_foreignkey.asp) - Prevents actions that would destroy links between tables
- [`CHECK`](https://www.w3schools.com/sql/sql_check.asp) - Ensures that the values in a column satisfies a specific condition
- [`DEFAULT`](https://www.w3schools.com/sql/sql_default.asp) - Sets a default value for a column if no value is specified
- [`CREATE INDEX`](https://www.w3schools.com/sql/sql_create_index.asp) - Used to create and retrieve data from the database very quickly