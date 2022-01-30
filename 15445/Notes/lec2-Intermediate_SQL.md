# Basic Syntax
Example with simple selection, projection and join
```sql
  SELECT s.name
  FROM enrolled AS e, student AS s
  WHERE e.grade = 'A' AND e.cid = '15-721'
  AND e.sid = s.sid
```

# Aggregates
- decl
  - AVG(col) - returns the average col value
  - MIN(col) - returns minimum col value
  - MAX(col) - returns maximum col value
  - SUM(col) - returns sum of col value
  - COUNT(col) - returns # of values fro col
- usage: almost only be used in the `SELECT` output list
  ```sql
    SELECT COUNT(login) as cnt
      FROM student WHERE login LIKE '%@cs'
  ```
  Output of other columns outside of an aggregate is undefined
  ```sql
    SELECT AVG(s.gpa), e.cid
      FROM enrolled AS e, student AS s
      WHERE e.sid = s.sid
  ```
- distinct aggregates: `COUNT`, `SUM` and `AVG` support `DISTINCT`
  ```
    SELECT COUNT(DISTINCT login)
      FROM student WHERE login LIKE '%@cs'
  ```

# Group By
- Project tuples into subsets and calculate aggregates against each subset
```sql
GROUP BY attr
```
where this time output other columns make sense, and non-aggregated value in `SELECT` output clause **must** appear in `GROUP BY` clause.

# Having
- Filter results based on aggregation computation, like `WHERE` clause for a `GROUP BY`.
```sql
SELECT AVG(s.gpa) AS avg_gpa, e.cid
FROM enrolled AS e, student AS s
WHERE e.sid = s.sid
GROUP BY e.cid
HAVING AVG(s.gpa) > 3.9
```

# String Operations
- `LIKE`: used for string matching, '%" any substring(including empty string), '\_' match any one character.
- And other string functions
- '||' operator to concatenate two or more strings

# Output Redirection
- `INTO`
- `ORDER BY` [column*] [ASC|DESC]
- `LIMIT` [count] [offset]

# Nested Query
- Queries containing other queries, often difficult to optimize.
```sql
  SELECT name FROM student WHERE
    sid IN (SELECT sid FROM enrolled)
```
- types
  - `ALL` -  must satisfy expression for all rows in the sub-query
  - `ANY` - must satisfy expression for at least one in sub-query
  - `IN` - equivalent to `=ANY()`
  - `EXISTS` - at least one row is returned

# Window Function
- Performs a sliding calculation across a set of tuples that are related
```sql
  SELECT ... FUNC_NAME() OVER() FROM tb
```
- Function could be aggregation function or special window function
  - ROW_NUMBER()
  - RANK
- `OVER` keyword shows how to group together tuples when computing the window function
  - use `PARTITION BY` to specify group, just like group by
  - `ORDER BY` to sort

# Common Table Expressions
- Provides a way to write auxiliary statements for use in larger query
- out put columns could be bind to names with `AS`
