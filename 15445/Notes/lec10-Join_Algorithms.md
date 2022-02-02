# Join
Join operator to reconstruct the original tuples without info loss.  
Binary joins using inner equijoin, and smaller table be left one.

## Operator Output
For tuple r\in R and tuple s\in S that match on join attributes, concatenate r and s into new tuple.  
Output contents:
- depends on processing model
- depends on storage model
- depends on data requirements in query
- Data:
  - Early materialization: Copy values into new output tuple. Subsequent operators never need go back to the base tuple for more data.
  - Late materialization: Only copy joins keys along with Record IDs of matching tuples.
    - ideal for column stores

## Cost Analysis
Cost metric: # of IOs to compute join.
- R: M pages and m tuples
- S: N pages and n tuples

### Nested Loop Join
- pick smaller table as outer table
- buffer as much outer table imn mem as possible
- loop over inner table or index
#### Stupid nested loop join
```
foreach tuple r in R:
  foreach tuple s in S:
    emit if r and s match
```
- cons: for every tuple in R, scan all S once;
- cost: M + m x N

#### Block nested loop join
```
foreach block B_R in R
  foreach block B_S in S
    foreach tuple r in B_R
      foreach tuple s in B_S
        emit if r and s match
```
- pros: for every block in R, it scans S once
- cost: M + M x N
- buffer: with B - 2 buffers for outer smaller table
  - cost: M + [M/(B-2)]·N

#### Index Nested Loop Join
```
foreach tuple r in R:
  foreach tuple s in Index(r_i = s_j):
    emit, if r and s match
```
- cost: M + m * C, C is constant index probe cost

### Sort Merge Join
- Sort: sort both table on join keys(external sort)
- Merge: scan each with cursor on both table and emit on matching; backtracing needed
- Cost: Sort Both Table + Merge Cost
- condition:
  - One or both table already sorted on join key;
  - Out put must be sorted

### Hashing Join
- Build: scan outer relation and generate hash table using functon h1 on join attr
- Probe: scan inneer relation and use h1 on tuple to jump to a location and find match tuple
```
build hash table HT_R for R
foreach tuple s in S:
  output, if h_1(s) in HT_R
```
- hash table contents:
  - key: attr query is doing
  - value: depends on implementation
    - full tuple: avoid retrieving other info after match; cost more space;
    - tuple identifier: base table tuple or intermediate output; ideal for col store; better if join selectivity is low.
- probe phase optimization: create bloom filter during build phase when its likely not exist in hash table.
  - probabilistic structure: true if inside; but false determine negative.

Cost:
- how bige table can we use: B · (B - 1);
- 3(M+N)

#### Grace hash join
- Hash R into O buckets
- Hash S into same # of buckets with same hash function
- Perform nested loop join on each pair of matching buckets in save level
- If bucket not fit in memory, repartition bucket
