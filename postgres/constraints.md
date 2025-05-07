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

This constraint ensures that the column does not store null values. This is a column only constraint.

For example,

```sql
CREATE TABLE products (
    product_no integer NOT NULL,
    name text NOT NULL,
    price numeric
);
```

NOT COMPLETE

## UNIQUE Constraint

Unique constraints ensure that the data contained in a column, or a group of columns, is unique among all the rows in the table.

For example,

This constraint can be specified as column constraint and table constraint.

```sql
CREATE TABLE products (
    product_no integer UNIQUE,
    name text,
    price numeric
);
```

```sql
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric,
    UNIQUE (product_no)
);
```

To define a unique constraint for a group of columns, write it as a table constraint with the column names separated by commas:

```sql
CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    UNIQUE (a, c)
);
```

NOT COMPLETE

## PRIMARY KEY Constraint

A primary key constraint indicates that a column, or group of columns, can be used as a unique identifier for rows in the table. This requires that the values be both unique and not null.

A table can have at most one primary key.

```sql
CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
);
```

This sql query is same as the following

```sql
CREATE TABLE products (
    product_no integer UNIQUE NOT NULL,
    name text,
    price numeric
);
```

Primary keys can span more than one column; the syntax is similar to unique constraints:

```sql
CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    PRIMARY KEY (a, c)
);
```

## References

- https://www.postgresql.org/docs/current/ddl-constraints.html
