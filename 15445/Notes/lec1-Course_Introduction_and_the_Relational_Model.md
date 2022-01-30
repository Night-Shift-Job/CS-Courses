# Relational Model
- **primary key** uniquely identifies a single tuple.  
  some dbms automatically create an internal key if table does not define one.
- **foreign key** specifies attribute from one relation to map to a tuple in another relation.
# Relational Algebra
- Select-$\sigma$: choose subset of tuples to satisfy predicate;
  ```sql
    SELECT * FROM R
      WHERE PREDICATION;
  ```
- Projection-$\pi$: generate relation with tuples contains only the specified attributes
  ```sql
    SELECT ATTRS
      FROM R WHERE PREDICATION;
  ```
- Union: generate relation contains all tuple appear in either or both relations
  ```sql
    (SELECT * FROM R)
      UNION ALL
    (SELECT * FROM S);
  ```
- Intercection: generate a relation contains only tuple appear in both relations
  ```sql
    (SELECT * FROM R)
      EXCEPT
    (SELECT * FROM S);
  ```
- Product: generate a relation contains all possible combinations of tuples from input relations
  ```sql
    SELECT * FROM R CROSS JOIN S;
    SELECT * FROM R, S;
  ```
- Join: generate a relation that contains all tuples that are combination of two tuples with common values for one or more attrs
  ```sql
    SELECT * FROM R NATURAL JOIN S;
  ```
