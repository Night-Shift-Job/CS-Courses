# Query Plan
Operators arranged in a tree, and data flows from leaves upward to the root.  
Output of the root node is the result of the query.

Cannot assume query results fit in mem. Rely on buffer pool, and maximize the amount of sequential I/O.

## External Merge Sort
Query may want tuples in `ORDERED BY`, `DISTINCT`, `GROUP BY` and bulk insert to b-tree.
Divide-and-Conquer split data into separate runs, sort individually and combine.
- Pass #0: use B buffer pages, produce $\ceil{N/B}$ sorted runs of size B;
- Pass #1: merge B-1 runs
- Number of passes: $1+\ceil{\log_{B-1}^{N/B}}$;(B-1)^P >= N-B
- Total I/O Cost: $2N\mult(# of Passes)$; read and write all pages each pass.

### Using B+ tree for sorting
If table already has an index on the sort attribute, just traversing the leaf pages of tree.
- Clustered: sequential scan leaf.
- Unclustered: chase each pointer to the page contains data. one I/O per data record.

## Aggregations
Collapse values for single attr from multiple tuples into single scalar value.
- Sorting: get same value together
- Hashing: generate a hash table during scanning, check if already exist in hash table:
  - distinct: discard duplicate
  - groupby: perform aggregate computation
  - External Hashing:
    - Partitions: divide tuples into bucket based on hash key, write to disk when full
    - Rehash: read into mem and build an in mem hash with another hash function, go through each bucket of this hash to bring together matching tuples.
    - During rehash, sotre {GroupKey: RunningVal}, when find a matching GroupKey, update RunningVal, otherwise insert new pair
