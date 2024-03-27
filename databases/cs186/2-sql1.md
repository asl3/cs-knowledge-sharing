# SQL 1

[Link to SQL 1 playlist](https://www.youtube.com/watch?v=6b5VdxHE4Fw&list=PLzzVuDSjP25R1px8yE4wJcXcRbwsCuunP)

## SQL History

- Developed at IBM Research
- SQL persisted, despite emerging alternatives
  - Object-oriented DBMS
  - XML (Xquery, Xpath, XSLT)
  - NoSQL, MapReduce (Google)
- Hadoop, Spark mostly used via SQL interfaces

## SQL Pros and Cons

- Declarative
  - Say what you want, not how to compute it (decision left to database system)
- Implemented widely
  - SQLite on phone, and implemented on Cloud
  - Varying levels of efficiency and completeness
- Constrained
  - Not targeted to be Turing-complete
- General-purpose and feature-rich
  - Extensible to other languages and data sources
  - Can mix queries with imperitive code

## Database

- **Database**: set of named relations
- **Relation (aka Table)**
  - **Schema**: description, metadata
  - **Instance**: set of data satisfying schema
- **Attribute** (aka Column, Field)
- **Tuple** (aka Record, Row)

## Relational Tables

- Schema is fixed (Schema MUST be enforced)
  - Unique attribute names
  - ATOMIC types (not collections, can’t be further broken down)
- Instance (aka data in the rows) can change often
  - Insert and delete rows often
- Multiset of rows: can have duplicate rows
  - Can keep count of tuples, and count of distinct tuples

## SQL Sublanguages

- DDL: Data Definition Language
  - Used to define + modify schema
- DML: Data Manipulation Language
  - Used to write queries
- **Relational Database Management System (RDBMS)**: evaluate language *efficiently* using choice of algorithms

## Code Example: Creating Table in SQL, Primary Key

```sql
CREATE TABLE Sailors (
  sid INTEGER,
  sname CHAR(20), 
  rating INTEGER,
  age FLOAT,
  PRIMARY KEY (sid)
);
```

### Primary Key Notes

- provides unique lookup key for relation
- *cannot* have duplicate values
- can have > 1 column (firstname, lastname, id...)

## Code Example: Foreign Key

```sql
CREATE TABLE Reserves (
  sid INTEGER,
  bid INTEGER, 
  day DATE,
  PRIMARY KEY (sid, bid, day),
  FOREIGN KEY (sid) REFERENCES Sailors
);
```

### Foreign Key Notes

- sid POINTS to sid column from other table, Sailors
  - any value that's the same is a reference from Reserves to the corresponding value in Sailors
  - in general, foreign references the **PRIMARY KEY** specified in referrent table

## Basic Single-Table Query

```sql
SELECT [DISTINCT] <column epxression list>
FROM <single table>
[WHERE <predicate>]
```

- produce all tuples in table that satisfy predicate
- output expressions in SELECT list
- expression can be column reference or arithmetic expression

## Code Examples: SQL Query

```sql
SELECT s.name, S.gpa, S.age*2 AS a2
FROM Students S
WHERE S.dept = 'CS'
ORDER BY S.gpa DESC, S.name ASC, a2;
LIMIT 3
```

- ORDER BY specifies sorting order (lexcographic) -> refers to columns in the output
  - can order by descending or ascending (ascending is default for ints)
- LIMIT: number of rows you want to see
  - LIMIT with no ORDER BY is non-deterministic bc rows NOT in particular order

### Code Example: Aggregates

```sql
SELECT [DISTINCT] AVG(S.gpa)
FROM Students
WHERE S.dept = 'CS'
```

- produces 1 row of output
  - in this case, 1 column (avg)
- Other aggregates: SUM, COUNT, MAX, MIN

### Code Example: GROUP BY

```sql
SELECT [DISTINCT] AVG(S.gpa)
FROM Students
GROUP BY S.dept
```

- number of rows = **cardinality** of output
  - cardinality = # of distinct groups

### Code Example: Illegal GROUP BY

```sql
SELECT S.name, AVG(S.gpa)
FROM Students S
GROUP BY S.dept;
```

Why is this illegal?

- There is no unique S.name from S.dept groups
  - column in SELECT list must either 1) be included in the GROUP BY clause OR 1) nested in an AGGREGATE function

### Code Example: HAVING

```sql
SELECT [DISTINCT] AVG(S.gpa), S.dept
FROM Students
GROUP BY S.dept
HAVING COUNT(*) > 2
```

- HAVING filters for only groups with counts greater than 2
  - applied after grouping and aggregation
  - can ONLY be used in grouping queries

### Code Example: Putting all together

```sql
SELECT S.dept, AVG(S.gpa), COUNT(*)
FROM Students S
WHERE S.gender = 'F'
GROUP BY S.dept
HAVING COUNT(*) > 2
ORDER BY S.dept;
```

consider what the above code does

### Code Example: Order of Operation for DISTINCT Aggregates

Option 1:

```sql
SELECT COUNT(DISTINCT S.name)
FROM Students S
WHERE S.dept = 'CS';
```

Option 2:

```sql
SELECT DISTINCT COUNT(S.name)
FROM Students S
WHERE S.dept = 'CS';
```

What's the difference between Option 1 vs. Option 2?

- Option 1 counts the unique names
- Option 2's DISTINCT is redundant because there's already only 1 row, with a count

## General Basic Single-Table Query

```sql
SELECT [DISTINCT] <column expression list> FROM <single table>
[WHERE <predicate>]
[GROUP BY <column list>]
[HAVING <predicate>]
[ORDER BY <column list>]
[LIMIT <integer>];
```

## Summary

- Relational model has well-defined query semantics
- Modern SQL extends pure relational model
- DBMS finds a fast way to execute a query, regardless of how it's written
