# abstrace
- compare modern sequential scans and secondary index scan
  - while scan become more useful in more cases than before, **both access paths are still useful**.
  - aps is still required to achieve the best performance
- take **query concurrency** into account in APS.

# intro
## 回顾aps
- index: when query is predicted **on a clusted index**.
- sequential scan: when query is predicated on an attribute **with no index**.
- when query predicted on a column with a secondary index, secondary index scan may or may not work better than a full sequential scan.
- The decision of aps is typically based on a selectivity threshold, underlying hardware properties, system design parameters and data layout.
  - fixed for all queries.

## modern analysis systems
column-group storage give more pressure on secondary indexing.
1. only attr needed by the query , avoiding unneeded reads in secondary index.
2. **vectorized execution** passes block of tuple to each operator.
    - processing block in tight loop, reducing interpretation logic overhead
3. share scan when system is processing more than one query across same attr.
    - cache friendly
4. compressing individual columns and working derectly over compressed data, reduces cost of moving data through memory hierarchy.
5. holding each attr **contiguously in a dense array** allows tight for loop evaluation and fits well to SIMD.

## aps in main-mem
Hot data can be memory resident due to large memories, questions need for secondary index.
- secondary index used to minimizing data movement from disk, no longer on the critical path.
- opt not to perform secondary index at all and focus on maximizing scan performance with clustered index and data skipping techniques.
- time for optimization takes limited time, and became new bottleneck.

# APS
