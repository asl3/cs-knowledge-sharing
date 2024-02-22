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
  - INTERSECT ALL: Take min of cardinalities
  - EXCEPT: Difference of cardinalities

note to self: finished up to SQLII 23 28
