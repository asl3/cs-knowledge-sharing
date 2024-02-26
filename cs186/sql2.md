# SQL II [WIP]

[Link to SQL 2 playlist](https://www.youtube.com/watch?v=PD8dIic-5IE&list=PLzzVuDSjP25QapEtTMxw56ZtKRf62lkL_)

## Conceptual SQL Evaluation

## Evaluation Order

1. FROM: Identify table
2. WHERE: Apply selections (eliminate rows)
3. SELECT: Keep only columns from SELECT, GBY, HAVING and project away others
4. GROUP BY: Form groups and aggregate
5. HAVING: Eliminate groups
6. DISTINCT: Eliminate duplicates

Optional formatters:

- ORDER BY
- LIMIT

## Conceptual Evaluation

### 1. FROM

- Relation cross-product
  - form all combinations of tuples, concatenated together

#### Note: Column Name Aliases

```%sql
SELECT ...
FROM Sailors AS S, Reserves as R
WHERE S.sid = R.sid
```

#### Cross product of table with ITSELF

```%sql
SELECT x.sname, x.age, y.sname AS sname2, y.age AS age2
FROM Sailors as x, Sailors AS y
WHERE x.age > y.age
```

- aliasing same table as different alias (x and y)
- result table: displays all combos of sailors older than other sailors

#### SQL as a Calculator

```%sql
SELECT
log(1000) as three
exp(ln(2)) as two ...
;
```

### 2. WHERE

#### String Comparisons

Old:
`WHERE S.name LIKE 'B_%'`

Standard Regex:
`WHERE S.name ~ 'B.*'`

#### Combining Predicates

- Careful when converting from boolean logic to set logic (e.g. AND/OR to UNION/INTERSECT), not always obvious

#### Set Semantics

- Set: Collection of distinct elements
- Ways to manipulate sets
  - Union
  - Intersect
  - Except
- Treat tuples (rows) within relation as elements of a set

#### Set Semantics Review

R = {A, A, A, A, B, B, C, D}
S = {A, A, B, B, B, C, E}

- UNION: {A, B, C, D, E}
- INTERSECT: {A, B, C}
- EXCEPT: {D}
  - tuples in R except those in S

#### Multiset Semantics

- Can track cardinality of tuples in set
- Multiset Operations
  - UNION: Sum cardinalities
  - INTERSECT: Min of cardinalities
  - EXCEPT: Difference of cardinalities

## Nested Queries

IN

```%sql
SELECT S.sname
FROM Sailors S
WHERE S.sid IN
(SELECT R.sid
FROM Reserves R
WHERE R.bid=102)
```

NOT IN

```%sql
SELECT S.sname
FROM Sailors S
WHERE S.sid NOT IN
(SELECT R.sid
FROM Reserves R
WHERE R.bid=102)
```

EXISTS

```%sql
SELECT S.sname
FROM Sailors S
WHERE S.sid EXISTS
(SELECT * FROM Reserves R
WHERE R.bid=102 AND S.sid=R.sid)
```

ANY

```%sql
SELECT *
FROM Sailors S
WHERE S.rating > ANY
(SELECT S2.rating FROM Sailors S2
WHERE S2.sname='Popeye')
```

- return any tuples where sailor rating is greater than any sailor named Popeye

## ARGMAX

How can we get the ARGMAX from a table?

```%sql
SELECT MAX(S.rating)
FROM Sailors S;
```

- NOT the ARGMAX, just the max

```%sql
SELECT S.*, MAX(S.rating)
FROM Sailors S;
```

- NOT legal SQL

```%sql
SELECT *
FROM Sailors S
WHERE S.rating >= ALL
(SELECT S2.rating FROM Sailors S2)
```

- this works! gives the correct ARGMAX

```%sql
SELECT *
FROM Sailors S
WHERE S.rating =
(SELECT MAX(S2.rating) FROM Sailors S2)
```

- this also works! gives the correct ARGMAX

```%sql
SELECT *
FROM Sailors S
ORDER BY rating DESC
LIMIT 1;
```

- also returns ARGMAX
- difference between previous two: this approach guarantees 1 output, whereas the previous two may return multiple rows. the last code may be non-deterministic in the row it returns

## Inner Joins

```%sql
SELECT s.*, r.bind
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND ...
```

equivalent to

```%sql
SELECT s.*, r.bind
FROM Sailors s INNER JOIN Reserves r
ON s.sid = r.sid
WHERE ...
```

## Join Variants

- INNER
  - default
- NATURAL
  - special case of INNER: equi-join for pairs of attributes with same name
  - generally avoid bc can break unpredictably if you're adding columns to rows
- LEFT/RIGHT/FULL OUTER
  - LEFT OUTER:
    - returns all matched rows and preserves all unmatched rows from table on left of join clause
    - use nulls in fields of non-matching tuples
  - RIGHT OUTER:
    - same as above, but replace "left" with "right"
  - FULL OUTER:
    - returns all matched or unmatched rows from the tables on both sides of join clause

## VIEWS

```%sql
CREATE VIEW view_name
AS select_statement
```

- simplifies development
- often used for security
- not materialized into storage

Using WITH

```%sql
WITH Reds(bid, scount) AS
(SELECT B.bid, COUNT (*)
FROM Boars2 B, Reserves2 R
WHERE R.bid = B.bid AND B.color = 'red'
GROUP BY B.bid);
```

## NULL Values

- Complications:
  - Selection predicates (WHERE)
  - Aggregates
- NULL comes naturally from OUTER JOIN

NULL in comparators

- x op NULL always = NULL

  - SELECT rating = NULL FROM SAILORS;
    - every sailor will return NULL
  - INSTEAD, use IS
    - SELECT \* FROM SAILORS WHERE rating IS NULL;

- Rule: Do not output a result where NULL

  - if a query evaluates to NULL, don't output any tuple

- Null in Boolean logic

  - NULL can take on either T or F, so answer should accomodate either value

- Null in Aggregation
  - NULL column values are IGNORED by aggregate functions (count, sum, avg...)
