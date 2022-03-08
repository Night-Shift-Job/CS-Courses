# Query Optimization
Optimizer generates mapping of logical algebra expr to optimal equivalent physical algebra expression.  
Physical operators define a specific execution strategy using access path.
- Depend on physical format of data that process.
- Not always a 1:1 mapping from logical to physical.

Query Optimization is NP-H
- Heuristics/Rules
  - rewrite query to remove stupid / inefficient things.
  - need to exam catalog, not data
- Cost-based Search
  - use model to estimate cost of executing plan
  - evaluate multiple equivalent plans for query and pick lowest cost.

## Heuristics Rules
### Relational Algebra Equivalence(Query rewrting)
Two relational algebra expression equivalent if generate some set of tuples.  
- Logical query Optimization: increase likelihood of enumerating the optimal plan in search.
- Some sketch
  - Predicate Pushdown
  - Selection: perform filter as early as posible
  - Join: Commutative, associative
- method:
  - split conjunctive predicates: into basic predicates
  - predicate pushdown: push down as posible
  - replace cartesian products: replace with inner join
  - prejection pushdown: eliminate redundant attributes before pipeline breakers

### Nested subquery
Treat nested sub-queries in where clause as functions that take parameters and return a single value or set of values.
- Rewrt to de-correlate and/or flatten them
- Decompose nested query and store result to temporary table

### Expression Rewriting
Transform query's expressions into optimal/minimal set of expressions.

## Cost Based Search
Generate an estimate of cost of executing particular query plan.
- Physical Costs: Predict CPU cycles, IO, cache misses, RAM consumption, prefetching
- Logical Costs: Estimate result of sizes per operator.
- Algorithmic Costs: Complexity of operator algorithm

### Statistics, Logical Cost
For each relation R, DBMS maintains the following info:
- N_R: number of tuples in R
- V(A, R): number of distinct values for attribute A.

Selection Cardinality: SC(A,R) is N_R / V(A,R), which assumes data uniformity where every value has same frequency.  
- Predicate estimate: Most same as probability;
- Join estimate: estSize = NR x NS / MAX(V(A,S), V(A,R));

- Histogram for non-uniform data assumption.
- Sampling to get estimation

## Query Plan
- enumerate ordering
- enumerate plan for each operator
- enumerate access path for each table

### Dynamic Programming
Enumerate combinations and cost, add up together.
- Left-deep only: (((AB) C) D)
```
R <- set of relations to join:
  for \alpha in [1,|R|]:
    for S in {all length of \alpha subset of R}:
      optjoin(S) = a join(S-a), where a is single relation that:
      minimize(cost(optjoin(S-a)) + min(cost join (S-a) to a + min access cost for a;
```
- Complexity:
  - \sum_n^{choose(n, 1} subsets considered
  - so 2^n subsets.
  - for each subset: m ways to remove 1 join, so total: O(nm 2^n)
