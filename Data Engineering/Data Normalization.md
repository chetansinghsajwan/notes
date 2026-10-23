The process of organizing a database to **reduce redundancy** and **improve data integrity** by structuring tables and relationships efficiently.
### ==Normal Forms==
|Normal Form|Eliminates|Key Rule|
|---|---|---|
|**1NF**|Repeating groups|Atomic values, unique rows|
|**2NF**|Partial dependencies|Every non-key column depends on the _whole_ primary key|
|**3NF**|Transitive dependencies|Non-key columns depend _only_ on the primary key|
|**BCNF**|Anomalies 3NF misses|Every determinant must be a candidate key|
|**4NF**|Multi-valued dependencies|No independent multi-valued facts|
|**5NF**|Join dependencies|No loss from decomposing further|
### Why It Matters
- **No duplicate data** → saves storage
- **Easier updates** → change in one place only
- **Data consistency** → no conflicting records
- **Better query design** → cleaner relationships
### ==Trade-off==
||Normalized|Denormalized|
|---|---|---|
|**Storage**|Less|More|
|**Write speed**|Faster|Slower|
|**Read speed**|Slower (more joins)|Faster|
|**Use case**|OLTP (banking, e-commerce)|OLAP (analytics, reporting)|

> **Most real-world databases (including Indian fintech, banking systems like UPI infrastructure) target 3NF or BCNF** as the sweet spot between integrity and performance.
### ==Who created it?==
- Introduced by ==Edgar F. Codd== — a British computer scientist working at **IBM** introduced data normalization in his paper ==A Relational Model of Data for Large Shared Data Banks== in 1970.
### ==1NF — First Normal Form==
- Each cell must have a **single (atomic) value**
- No repeating groups or arrays
- Each row must be **unique**
**Violation example:**
|Student|Subjects|
|---|---|
|Ravi|Maths, Science|
**Fixed:**
|Student|Subject|
|---|---|
|Ravi|Maths|
|Ravi|Science|
### ==2NF — Second Normal Form==
- Must be in **1NF first**
- Every non-key column must depend on the **entire primary key** (no partial dependency)
- Only relevant when there's a **composite primary key**
**Violation:** `(StudentID, CourseID) → StudentName` — StudentName depends only on StudentID, not the full key
**Fix:** Split into separate tables — one for students, one for enrollments
### ==3NF — Third Normal Form==
- Must be in **2NF first**
- No **transitive dependencies** (non-key column depending on another non-key column)
**Violation:**
|EmpID|DeptID|DeptName|
|---|---|---|
- DeptName depends on DeptID, not EmpID
**Fix:** Move DeptID → DeptName into a separate `Department` table
### ==BCNF — Boyce-Codd Normal Form==
- Stricter version of 3NF
- Every **determinant must be a candidate key**
- Handles edge cases 3NF misses with **overlapping composite keys**
### ==4NF — Fourth Normal Form==
- Must be in **BCNF first**
- No **multi-valued dependencies**
**Violation:**
|Teacher|Subject|Hobby|
|---|---|---|
|Priya|Maths|Cricket|
- Subject and Hobby are independent — shouldn't be in one table
**Fix:** Split into `Teacher-Subject` and `Teacher-Hobby` tables
### ==5NF — Fifth Normal Form==
- Must be in **4NF first**
- No **join dependencies** — table cannot be broken further without losing data
- Rarely needed in practice; used in very **complex many-to-many relationships**