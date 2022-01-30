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
