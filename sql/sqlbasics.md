

SQL — Detailed Revision Notes
1. What is SQL?
SQL (Structured Query Language) is used to interact with relational databases.

2. There are four important terms:
- Database: The organized collection of data, such as CollegeDB.
- Table: A structured collection of related records, such as students.
- Row: One complete record, such as Rahul's details.
- Column: One attribute, such as name or marks.

3. These terms are related, but they are not the same.
Term	    Meaning
SQL	        The language used to query relational databases
MySQL	    A relational database management system (RDBMS)
PostgreSQL	Another RDBMS
SQLite	    A lightweight relational database engine
MongoDB 	A document-oriented NoSQL database
Database     Container for related tables.

Row - Represents one record.
Column - Represents an attribute.

4. SQL Command Categories
SQL
│
├── DDL
│
├── DML
│
├── DQL
│
├── DCL
│
└── TCL

# DDL — Data Definition Language
Deals with database structure.
CREATE
ALTER
DROP - Delete the entire table
TRUNCATE - Delete all recoreds only structure lefts

# DML — Data Manipulation Language
Deals with modifying data.

INSERT
UPDATE
DELETE -  Deletes particular reacord (row)

# Example:
UPDATE students
SET marks = 90
WHERE id = 1;

# DQL — Data Query Language
Primarily:
SELECT


Example:
SELECT *
FROM students;

# DCL - Data Control Language
Deals with permissions.

GRANT
REVOKE
TCL

# TCL - Transaction Control Language
Deals with transactions.

COMMIT
ROLLBACK
SAVEPOINT

You'll study TCL deeply later with transactions and ACID.


5. CRUD 
One of the most important concepts for backend development.

CRUD
│
├── C → Create → INSERT
├── R → Read   → SELECT
├── U → Update → UPDATE
└── D → Delete → DELETE
# create
1. Create Database:
CREATE DATABASE college;

USE college;

2. Create Table:
CREATE TABLE students (
    id INT,
    name VARCHAR(50),
    age INT,
    city VARCHAR(50),
    marks INT
);


# Insert
INSERT INTO students
(id, name, marks)
VALUES (1, 'Rahul', 85);

# Read
SELECT * FROM students;

# UPDATE
UPDATE students
SET marks = 90
WHERE id = 1;

# DELETE
DELETE FROM students
WHERE id = 1;


6. Data Types
Common MySQL data types:

Numeric
├── INT
├── BIGINT
├── DECIMAL
└── FLOAT

String
├── CHAR
├── VARCHAR
└── TEXT

Date/Time
├── DATE
├── DATETIME
└── TIMESTAMP

Other
├── BOOLEAN
└── JSON
VARCHAR(50)

name VARCHAR(50) - means the column can store a variable-length string with a maximum length of 50 characters.

7. SELECT

# Select all columns:
SELECT * FROM students;

# Select specific columns:
SELECT name, marks
FROM students;


8. WHERE
Used to filter rows.

SELECT *
FROM students
WHERE marks > 80;


9. Common operators:

=       Equal
!=      Not equal
<>      Not equal
>       Greater
<       Smaller
>=      Greater/equal
<=      Smaller/equal

Example:
SELECT *
FROM students
WHERE age >= 20;

10. AND / OR / NOT
* AND - Both conditions must be true.
SELECT *
FROM students
WHERE age = 20
AND marks > 80;

* OR - At least one condition must be true.
SELECT *
FROM students
WHERE city = 'Delhi'
OR city = 'Mumbai';

* NOT - Negates a condition.
SELECT *
FROM students
WHERE NOT city = 'Delhi';

11. DISTINCT
Removes duplicate values from the result.

SELECT DISTINCT city
FROM students;

Important: DISTINCT applies to the combination of selected columns.

For example:
SELECT DISTINCT city, age
FROM students;

returns unique (city, age) combinations.


12. ORDER BY
Used to sort results.

Ascending:
SELECT *
FROM students
ORDER BY marks ASC;

Descending:
SELECT *
FROM students
ORDER BY marks DESC;

ASC is the default.

13. LIMIT
Restricts number of returned rows.

SELECT *
FROM students
LIMIT 5;

Top 3 highest salaries:
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;

14. OFFSET
Used to skip rows.

SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 2;

Conceptually:
ORDER BY salary DESC

1st highest
2nd highest
3rd highest ← OFFSET skips first 2
     ↓
LIMIT 1

This is commonly used for pagination.


15. INSERT
* Single row:
INSERT INTO students
(id, name, age, marks)
VALUES
(1, 'Rahul', 20, 85);

* Multiple rows:
INSERT INTO students
(id, name, age, marks)
VALUES
(2, 'Aman', 21, 72),
(3, 'Priya', 20, 91),
(4, 'Neha', 22, 68);


16. UPDATE
UPDATE students
SET marks = 90
WHERE id = 1;

* Multiple columns:
UPDATE students
SET
    marks = 95,
    city = 'Delhi'
WHERE id = 1;

⚠️ Important
Without WHERE:

UPDATE students
SET marks = 90;

all rows are updated.


17. DELETE
Delete specific row:

DELETE FROM students
WHERE id = 1;

* Delete all rows:
DELETE FROM students;

The table itself still exists.


18. DELETE vs TRUNCATE vs DROP
Mental model:

DELETE
 ↓
remove rows

TRUNCATE
 ↓
remove all rows

DROP
 ↓
remove table completely


19. Aggregate Functions
Aggregate functions operate on multiple rows and produce a value.

COUNT()
SUM()
AVG()
MIN()
MAX()

* COUNT
SELECT COUNT(*)
FROM employees;

* SUM
SELECT SUM(salary)
FROM employees;

* AVG
SELECT AVG(salary)
FROM employees;

* MAX
SELECT MAX(salary)
FROM employees;

* MIN
SELECT MIN(salary)
FROM employees;


20. COUNT(*) vs COUNT(column)
* Very important.

COUNT(*) - counts rows.

COUNT(column) - counts non-NULL values in that column.

Example:

id	name	manager_id
1	Rahul	NULL
2	Aman	1
3	Priya	1

Then:
COUNT(*)= 3

But: COUNT(manager_id) = 2
because NULL isn't counted.


21. GROUP BY
Used to create groups of rows.
The best way to understand SQL GROUP BY is to imagine that you are sorting rows into separate buckets based on a common value, and then performing calculations on each bucket.

Example:
SELECT department_id, COUNT(*)
FROM employees
GROUP BY department_id;

If:
IT → Rahul
IT → Priya
HR → Aman

result:
IT → 2
HR → 1

Mental model:
Rows
 ↓
GROUP BY
 ↓
Groups
 ↓
Aggregate function
 ↓
One result per group


22. HAVING
HAVING filters groups.

Example:
SELECT department_id, COUNT(*)
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 1;

Meaning:
Return departments containing more than one employee.


23. WHERE vs HAVING
This is one of your most important SQL interview concepts.

WHERE
 ↓
filters rows

GROUP BY
 ↓
creates groups

HAVING
 ↓
filters groups

Example:

SELECT department_id, AVG(salary)
FROM employees
WHERE salary > 30000
GROUP BY department_id
HAVING AVG(salary) > 50000;

WHERE - Filters individual employee rows.

HAVING - Filters the resulting department groups.


24. SQL Logical Execution Order
Although we write:

SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT

the logical processing order is approximately:

FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
 ↓
LIMIT

This explains why:

WHERE COUNT(*) > 1 - is invalid.

COUNT(*) is an aggregate produced after grouping.

So Use:
HAVING COUNT(*) > 1
instead.



25. Keys
Keys are used to identify rows and establish relationships.

KEYS
│
├── Super Key
├── Candidate Key
├── Primary Key
├── Alternate Key
├── Composite Key
└── Foreign Key

26. Super Key
A super key is any set of attributes that can uniquely identify a row.

Suppose:
employee_id → unique
email       → unique
phone       → unique

Then these are super keys:

{employee_id}

{employee_id, name}

{employee_id, email}

{email}

{phone}

Even if additional unnecessary attributes are included, it remains a super key.

27. Candidate Key
A candidate key is a minimal super key.

"Minimal" means: No unnecessary attribute can be removed while maintaining uniqueness.

If:
employee_id → unique
email → unique
phone → unique

then:
Candidate Keys:
    employee_id
    email
    phone

One candidate key is selected as the primary key.

28. Primary Key
Uniquely identifies every row.

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

Properties:
PRIMARY KEY
│
├── UNIQUE
└── NOT NULL

A table can have one primary-key constraint, though that primary key can contain multiple columns.

29. Alternate Key
Candidate keys that were not selected as the primary key.
Then there a question may arise then what is difference between candidate key and primary key, so candidate key contains 
primary key while alternate key doesn't.

Example:
Candidate Keys:
├── employee_id ← Primary Key
├── email       ← Alternate Key
└── phone       ← Alternate Key

30. Composite Key
A key consisting of multiple columns.
Neither column alone is unique.


Mental model:

Composite Key
      ↓
Multiple columns
      ↓
Together identify row


31. Foreign Key
A foreign key establishes a relationship between tables.

departments
id
↑
│
│ referenced by
│
employees.department_id

Example:

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES departments(id)
);

A foreign key value generally must correspond to a referenced key value, unless it is allowed to be NULL.

31. Primary Key vs Foreign Key
Primary Key	Foreign Key
Identifies row	Establishes relationship
Unique	Can repeat
Cannot be NULL	Can be NULL unless restricted
One PK constraint per table	Multiple FKs possible
Entity identity	Reference to another table

Example:

departments
┌─────────────┐
│ id PK       │
│ name        │
└──────┬──────┘
       │
       │ referenced by
       ↓
employees
┌──────────────────┐
│ id PK            │
│ name             │
│ department_id FK │
└──────────────────┘
32. Constraints

Constraints enforce rules on data.

CONSTRAINTS
│
├── PRIMARY KEY
├── FOREIGN KEY
├── UNIQUE
├── NOT NULL
├── CHECK
└── DEFAULT
NOT NULL
name VARCHAR(50) NOT NULL

Value must be supplied.

UNIQUE
email VARCHAR(100) UNIQUE

Duplicate values aren't allowed.

CHECK
salary INT CHECK (salary > 0)

Enforces a condition.

DEFAULT
status VARCHAR(20) DEFAULT 'ACTIVE'

If no value is supplied, ACTIVE is used.

33. JOINs

JOINs combine data from multiple tables.

Suppose:

employees
id	name	department_id
1	Rahul	10
2	Aman	20
3	Priya	10
departments
id	name
10	IT
20	HR
30	Finance

Relationship:

employees.department_id
          ↓
departments.id
34. INNER JOIN

Returns only matching rows.

SELECT e.name, d.name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id;

Mental model:

A ∩ B

Only rows with matches on both sides.

35. LEFT JOIN

Returns:

All rows from the left table + matching rows from right table.

SELECT e.name, d.name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.id;

If an employee has no matching department:

employee → NULL

The employee still appears.

36. RIGHT JOIN

Returns:

All rows from the right table + matching rows from left table.

SELECT e.name, d.name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.id;

Useful when you want every department, including departments with no employees.

37. FULL OUTER JOIN

Returns all rows from both sides:

matches
+
unmatched left6
+
unmatched right

Conceptually:

A ∪ B

MySQL doesn't directly support FULL OUTER JOIN; it can be simulated using combinations of LEFT JOIN, RIGHT JOIN, and UNION.

38. CROSS JOIN

Produces a Cartesian product.

SELECT *
FROM colors
CROSS JOIN sizes;

If:

colors = 3 rows
sizes = 4 rows

result:

3 × 4 = 12 rows

Mental model:

Every A
   ×
Every B
39. SELF JOIN

A table joins with itself.

Useful for hierarchical relationships.

Example:

employees

id | name  | manager_id
---+-------+-----------
1  | Rahul | NULL
2  | Aman  | 1
3  | Priya | 1
4  | Neha  | 2

Query:

SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.id;

Result:

Rahul → NULL
Aman  → Rahul
Priya → Rahul
Neha  → Aman

Mental model:

employees e
     │
     │ self join
     ↓
employees m
40. JOIN + GROUP BY

Example:

Number of employees in each department.

SELECT
    d.name,
    COUNT(e.id) AS employee_count
FROM departments d
LEFT JOIN employees e
ON d.id = e.department_id
GROUP BY d.id, d.name;

Why LEFT JOIN?

Because we want departments with zero employees too.

And why:

COUNT(e.id)

instead of:

COUNT(*)

Because COUNT(e.id) ignores the NULL employee generated for an empty department.

41. Subqueries

A subquery is:

A query inside another query.

Example:

SELECT name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);

Conceptually:

Inner Query
    ↓
AVG salary
    ↓
result
    ↓
Outer Query
    ↓
compare salaries
42. Scalar Subquery

Returns one value.

SELECT AVG(salary)
FROM employees;

Then:

WHERE salary > (
    SELECT AVG(salary)
    FROM employees
)

The inner query produces a single value.

43. IN Subquery

Used when multiple values may be returned.

SELECT *
FROM employees
WHERE department_id IN (
    SELECT id
    FROM departments
    WHERE name IN ('IT', 'HR')
);

Mental model:

= 
 ↓
one value

IN
 ↓
multiple values
44. ANY
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 10
)

Means:

Greater than at least one returned value.

45. ALL
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 10
)

Means:

Greater than every returned value.

Remember:

ANY → at least one
ALL → every value
46. EXISTS

Checks whether the subquery returns at least one row.

SELECT d.name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.id
);

The actual value 1 doesn't matter.

EXISTS only asks:

Does a matching row exist?
        ↓
YES → TRUE
NO  → FALSE
47. Correlated Subquery

A correlated subquery depends on the outer query.

Example:

Find employees earning more than their department's average.

SELECT e.name, e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);

Important part:

e2.department_id = e.department_id

The inner query uses a value from the outer query.

48. Derived Table

A subquery inside FROM acts like a temporary result table.

SELECT *
FROM (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) x;

Mental model:

Subquery
   ↓
Temporary result
   ↓
Treat as table
49. Window Functions ⭐

Window functions calculate values across related rows without collapsing them.

Basic syntax:

FUNCTION() OVER (
    PARTITION BY ...
    ORDER BY ...
)

Example:

SELECT
    name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS dept_avg
FROM employees;

Every employee remains in the result.

50. GROUP BY vs Window Function

This is one of the most important distinctions.

GROUP BY
SELECT department, AVG(salary)
FROM employees
GROUP BY department;
5 employees
     ↓
GROUP BY
     ↓
2 department rows
Window Function
SELECT
    name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    )
FROM employees;
5 employees
     ↓
Window Function
     ↓
5 employees + average
Remember

GROUP BY collapses rows.

Window functions preserve rows.

51. PARTITION BY

PARTITION BY creates logical groups for a window function.

AVG(salary) OVER (
    PARTITION BY department
)

Conceptually:

Employees
│
├── IT
│   ├── Rahul
│   ├── Priya
│   └── Raj
│
└── HR
    ├── Aman
    └── Neha

Each partition is processed separately.

52. ROW_NUMBER()

Assigns a unique sequential number.

ROW_NUMBER() OVER (
    ORDER BY salary DESC
)

For:

100
90
90
80

result:

100 → 1
90  → 2
90  → 3
80  → 4

Ties still get different numbers.

53. RANK()

Ties receive the same rank, but gaps appear.

100 → 1
90  → 2
90  → 2
80  → 4
54. DENSE_RANK()

Ties receive the same rank, but there are no gaps.

100 → 1
90  → 2
90  → 2
80  → 3
55. ROW_NUMBER vs RANK vs DENSE_RANK
Function	Ties	Gaps
ROW_NUMBER()	Different numbers	N/A
RANK()	Same rank	Yes
DENSE_RANK()	Same rank	No

Remember:

ROW_NUMBER
→ unique position

RANK
→ tie + gap

DENSE_RANK
→ tie + no gap
56. Ranking Within Each Department
SELECT
    name,
    department,
    salary,
    ROW_NUMBER() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS rn
FROM employees;

Result conceptually:

IT
 ├── Priya → 1
 ├── Raj   → 2
 └── Rahul → 3

HR
 ├── Neha  → 1
 └── Aman  → 2

The ranking restarts for each partition.

57. LEAD()

Looks at a future row.

LEAD(sales) OVER (
    ORDER BY month
)

Example:

Jan → 100 → next = 150
Feb → 150 → next = 120
Mar → 120 → next = NULL

Mental model:

LEAD
 ↓
look forward
58. LAG()

Looks at a previous row.

LAG(sales) OVER (
    ORDER BY month
)

Example:

Jan → 100 → previous = NULL
Feb → 150 → previous = 100
Mar → 120 → previous = 150

Mental model:

LAG
 ↓
look backward
59. Running Total

Use:

SUM(amount) OVER (
    ORDER BY date
)

Example:

Day    Amount    Running Total
1      100       100
2      200       300
3      150       450
4      300       750

Unlike:

SUM(amount)

with GROUP BY, the window version preserves each row.

60. Running Total Per Customer
SELECT
    customer,
    date,
    amount,
    SUM(amount) OVER (
        PARTITION BY customer
        ORDER BY date
    ) AS running_total
FROM orders;

Mental model:

customer
   ↓
partition
   ↓
order by date
   ↓
cumulative SUM
61. Classic Interview Patterns
Second Highest Salary
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
Nth Highest Distinct Salary

Using DENSE_RANK():

SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
) x
WHERE rnk = N;
Top 3 Salaries Per Department
SELECT *
FROM (
    SELECT
        e.*,
        DENSE_RANK() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
) x
WHERE rnk <= 3;

Use DENSE_RANK() when you mean top 3 distinct salary levels.

Use ROW_NUMBER() when you mean exactly 3 employees.

62. Complete SQL Mental Model

This is the most useful summary of what you've learned:

                         SQL
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ↓                 ↓                 ↓
    DATABASE           QUERYING         DESIGN
        │                 │                 │
        │            ┌────┴────┐            │
        │            ↓         ↓            │
        │          SELECT    FILTER         │
        │                     │             │
        │              ┌──────┴──────┐      │
        │              ↓             ↓      │
        │            WHERE         DISTINCT  │
        │              │                    │
        │              ↓                    │
        │           GROUP BY                │
        │              │                    │
        │              ↓                    │
        │           HAVING                  │
        │              │                    │
        │              ↓                    │
        │          ORDER BY                 │
        │              │                    │
        │              ↓                    │
        │            LIMIT                  │
        │                                   │
        │                              ┌────┴────┐
        │                              ↓         ↓
        │                             KEYS   CONSTRAINTS
        │                              │         │
        │                              │         ├── PK
        │                              │         ├── FK
        │                              │         ├── UNIQUE
        │                              │         ├── NOT NULL
        │                              │         ├── CHECK
        │                              │         └── DEFAULT
        │                              │
        │                              └── Relationships
        │                                      │
        │                                      ↓
        │                                    JOINs
        │                                      │
        │                              ┌───────┼────────┐
        │                              ↓       ↓        ↓
        │                           INNER    LEFT     SELF
        │                           RIGHT    CROSS
        │
        └──────────────────────────────────────────────┐
                                                       ↓
                                                   SUBQUERIES
                                                       │
                                            ┌──────────┼─────────┐
                                            ↓          ↓         ↓
                                         Scalar      IN       EXISTS
                                                       │
                                                     ANY/ALL
                                                       │
                                                  Correlated
                                                       │
                                                       ↓
                                               WINDOW FUNCTIONS
                                                       │
                                      ┌────────────────┼──────────────┐
                                      ↓                ↓              ↓
                                  Ranking          Navigation     Aggregation
                                      │                │              │
                              ROW_NUMBER          LAG/LEAD       SUM OVER
                              RANK                                AVG OVER
                              DENSE_RANK                          COUNT OVER
63. Most Important Things to Memorize

If you're revising before an interview, prioritize these:

⭐ Level 1 — Must Know
SELECT
WHERE
ORDER BY
GROUP BY
HAVING
DISTINCT
LIMIT
⭐ Level 2
COUNT
SUM
AVG
MIN
MAX

INSERT
UPDATE
DELETE
⭐ Level 3
Primary Key
Foreign Key
Candidate Key
Super Key
Composite Key
Constraints
⭐ Level 4
INNER JOIN
LEFT JOIN
RIGHT JOIN
SELF JOIN
CROSS JOIN
⭐ Level 5
Subqueries
IN
EXISTS
ANY
ALL
Correlated Subqueries
⭐ Level 6 — Advanced
ROW_NUMBER
RANK
DENSE_RANK
LAG
LEAD
SUM() OVER()
AVG() OVER()
PARTITION BY
64. One-Line Revision Sheet
SELECT      → choose columns
WHERE       → filter rows
DISTINCT    → remove duplicate result combinations
ORDER BY    → sort
LIMIT       → restrict rows
OFFSET      → skip rows

COUNT       → count
SUM         → total
AVG         → average
MIN         → minimum
MAX         → maximum

GROUP BY    → create groups / collapse rows
HAVING      → filter groups

PK          → uniquely identifies a row
FK          → references another table
Candidate   → minimal unique identifier
Super Key   → any unique identifier set
Composite   → multiple columns form key

INNER JOIN  → matching rows
LEFT JOIN   → all left + matching right
RIGHT JOIN  → all right + matching left
CROSS JOIN  → every combination
SELF JOIN   → table joined with itself

Subquery    → query inside query
IN          → matches any returned value
EXISTS      → checks whether row exists
ANY         → condition true for at least one
ALL         → condition true for every value

GROUP BY    → reduces/collapses rows
WINDOW      → preserves rows

ROW_NUMBER  → unique sequence
RANK        → ties + gaps
DENSE_RANK  → ties + no gaps
LAG         → previous row
LEAD        → next row
PARTITION   → logical window groups

The next major section should be NULL + CASE + COALESCE + string/date functions, because these fill the gaps needed to solve most practical SQL queries. After that, the natural progression is CTEs → normalization → indexes → transactions/ACID → isolation levels → EXPLAIN/query optimization → interview problems.