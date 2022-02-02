# Buffer Pool Manager
Memory region organized as array of fixed-sized pages, entry is frame.  
When request a page, exact copy is placed into frame.

**Page table** keeps track of pages in mem. also maintains additional meta-data: **Dirty Flag**, and **Pin/Reference Counter**.

## Buffer Pool Optimizations
- Multiple buffer pools
  - reduce latch contentions and improve locality
  - get page by Object-id map or hashing
- Pre-fetching
  - prefetch pages based on query plan
    - sequential scan
    - index scan(get range of ojb/index)
- Scan sharing: reuse data retrieved from storage or operator computations
  - allow multiple queries to attach a single cursor(queries need not be same)
  - share intermediate results
- Buffer Pool Bypass
  - sequential scan operator will not store fetched pages in the buffer pool
 
 ## Buffer Replacement Policies
 decide which page to evict:
 ### LRU
 Maintain timestamp of page last accessed.
 When needs to evict a page, select one with oldest time stamp.
 ### Clock
 Approximation of LRU, no timestamp;
 - each page has ref bit
 - when page accessed set to 1
 - page organized in circular with clock hand
   - upon sweeping, check if page's bit is set to 1
   - if yes, set to 0; if no, evict
 ### LRU/CLOCK problem
 sequential flooding: perform sequential access to read every page; buffer pool full of pages read once then never again.
 - LRU-K: Track history of last K references to each page as timestamps and compute interval between subsequent accesses.  
 DBMS uses history to estimate next time that page is going to be access.
 - Localization: choose page to evict on a per txn/query basis.
 - Priority Hints: provide hints to puffer pool on page is important or not

- Dirty Pages: if page is dirty, dbms must write back to disk to ensure its changes are persisted.
- Background Walking: periodically walk through page table and write back to disk.
