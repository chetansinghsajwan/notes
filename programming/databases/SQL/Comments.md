**Single Line Comments**
Single line comments start with `--`.
**Example**
```
-- Select all:
SELECT * FROM Customers;
```
```
SELECT * FROM Customers -- WHERE City='Berlin';
```
**Multi-line Comments**
Multi-line comments start with `/*` and end with `*/`.
**Example**
```
/*Select all the columns
of all the records
in the Customers table:*/
SELECT * FROM Customers;
```
```
SELECT CustomerName, /*City,*/ Country FROM Customers;
```