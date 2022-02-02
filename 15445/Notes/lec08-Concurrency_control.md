# Concurrency Control
Method that DBMS uses to ensure "correct" results for concurrent operations on a shared object.

# Latches
- protect critical sections of the DBMS's internal data structure for other threads.
- Held for operation duration.
- Do not need to be able to rollback changes.

- Read Mode
  - multiple threads read same object at same time
  - a thread could acquire read latch if one held it in read mode
- Write Mode
  - only one thread could access object
  - cannot acquire write latch when others held it

## Latch Implements
- Blocking OS Mutex: Simple to use; Non-Scalable; std::mutex;
- Test And Set Spin Latch: Very efficient; Non-scalable, not cache-friendly, not friendly; std::atomic
- Reader-Writer Latches
  - Allows for concurrent readers
  - Manage read and write queues to avoid starvation
  - Can be implemented on top of spin latches

### Hash Table Latching
Easy to support as limited ways thread to access data structure. DeadLock impossible.  
To resize the table, take a global write latch on the entire table.
- Page Latches
- Slot Latches
- No Latches: CAS

# B+ Tree Concurrency Control
Get latch from parent to children and release latch for parent if safe.  
A safe node is one that will not split or merge when updated.
- Not full(on insersion)
- More than half-full(on deletion)

- Find: start at root and go down; acquire R latch on child; then unlatch parent.
- insert\Delete: start at root and go down; If child is safe, release all latches on ancestors.

- Problem: Taking a **write latch on the root** every time becomes **bottleneck** for higher concurrency.
  - Set latch as if for search, get to leaf, and set W-latch on leaf;
  - If leaf not safe, release all latches and start over as former method.
- Problem: Scan a range of nodes
  - To avoid deadlock: through code dicipline.
