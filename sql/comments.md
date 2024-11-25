# Comments

## Single Line Comments

Single line comments start with `--`.

**Example**

```sql
-- Select all:
SELECT * FROM Customers;
```

**Example**

```sql
SELECT * FROM Customers -- WHERE City='Berlin';
```

## Multi-line Comments

Multi-line comments start with `/*` and end with `*/`.


**Example**

```sql
/*Select all the columns
of all the records
in the Customers table:*/
SELECT * FROM Customers;
```

**Example**

```sql
SELECT CustomerName, /*City,*/ Country FROM Customers;
```
