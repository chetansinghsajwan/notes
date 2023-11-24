A `JOIN` clause is used to combine rows from two or more tables, based on  
a related column between them.
**Example**
**Orders Table**
|   |   |   |
|---|---|---|
|OrderID|CustomerID|OrderDate|
|10308|2|1996-09-18|
|10309|37|1996-09-19|
|10310|77|1996-09-20|
**Customers Table**
|   |   |   |   |
|---|---|---|---|
|CustomerID|CustomerName|ContactName|Country|
|1|Alfreds Futterkiste|Maria Anders|Germany|
|2|Ana Trujillo Emparedados y helados|Ana Trujillo|Mexico|
|3|Antonio Moreno Taquería|Antonio Moreno|Mexico|
The `CustomerID` column in the `Orders` table refers to the `CustomerID` in the `Customers` table. The relationship between the two tables above is the `CustomerID` column.
This selects records that have matching values in both tables.
```
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
```
  
**Different Types of SQL JOINs**
- `(INNER) JOIN`
    
    Returns records that have matching values in both tables
    
![[/Untitled 9.png|Untitled 9.png]]
- `LEFT (OUTER) JOIN`
    
    Returns all records from the left table, and the matched records from the right table
    
![[/Untitled 1 2.png|Untitled 1 2.png]]
- `RIGHT (OUTER) JOIN`
    
    Returns all records from the right table, and the matched records from the left table
    
![[/Untitled 2 3.png|Untitled 2 3.png]]
- `FULL (OUTER) JOIN`: Returns all records when there is a match in either left or right table