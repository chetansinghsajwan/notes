# Constraints

SQL constraints are used to specify rules for the data in a table.
Constraints are used to limit the type of data that can go into a table.

Constraints can be column level or table level. Column level constraints apply to a column, and table level constraints apply to the whole table.

For example,

```sql
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CHECK (price > 0), -- column constraint
    discounted_price numeric CHECK (discounted_price > 0), -- column constraint
    CHECK (price > discounted_price) -- table constraint
);
```

Column constraints can also be written as table constraints, while the reverse is not necessarily possible, since a column constraint is supposed to refer to only the column it is attached to.

Note: PostgreSQL doesn't enforce that rule, but you should follow it if you want your table definitions to work with other database systems.

Names can be assigned to constraints. This clarifies error messages and allows you to refer to the constraint when you need to change it. If you don't specify a constraint name, the system chooses a name for you.

For example,

```sql
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CONSTRAINT positive_price CHECK (price > 0)
);
```

When specifying multiple constraints, the order doesn't matter. It does not necessarily determine in which order the constraints are checked.

For example,

```sql
CREATE TABLE products (
    product_no integer NOT NULL,
    price numeric NOT NULL CHECK (price > 0) -- multiple constraints
);
```

## NOT NULL Constraint

This constraint ensures that the column does not store null values. This constraint can only 


The following constraints are commonly used in SQL:
  
- [`NOT NULL`](https://www.w3schools.com/sql/sql_notnull.asp) - Ensures that a column cannot have a NULL value
- [`UNIQUE`](https://www.w3schools.com/sql/sql_unique.asp) - Ensures that all values in a column are different
- [`PRIMARY KEY`](https://www.w3schools.com/sql/sql_primarykey.asp) - A combination of a `NOT NULL` and `UNIQUE`. Uniquely identifies each row in a table
- [`FOREIGN KEY`](https://www.w3schools.com/sql/sql_foreignkey.asp) - Prevents actions that would destroy links between tables
- [`CHECK`](https://www.w3schools.com/sql/sql_check.asp) - Ensures that the values in a column satisfies a specific condition
- [`DEFAULT`](https://www.w3schools.com/sql/sql_default.asp) - Sets a default value for a column if no value is specified
- [`CREATE INDEX`](https://www.w3schools.com/sql/sql_create_index.asp) - Used to create and retrieve data from the database very quickly

## References

- https://www.postgresql.org/docs/current/ddl-constraints.html
