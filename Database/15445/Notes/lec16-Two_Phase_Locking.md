# 2-PL locking
Ways to guarantee schedule correct without knowing entire schedule.
- #1: Growing, each txn requests locks needed from lock manager
- #2: Shrinking, the txn allowed only release lock previously acquired, unable to aquire new locks.

## Strong 2PL
Sufficient to guarant conflict serializability. But subject to **cascading aborts**.  
If t2 aborts, in order to prevents t2's execution, t1 has to aborts either.(Read uncommited).  
Solution: strict 2-PL, release all lock in the end of txn.

## Deadlock
- Detection: wait-for graph created and checked periodically.
- Handling: select a victim txn to rollback, by age/grogress/#of locks/txns have to rollback
- Prevention: when tries to acquire lock held by another txn, kills one of them.

# Lock granularities
Intention lock allows higher-level node to be locked in share/ex mode without check all decendent nodes.  
If node is intention locked, some explicity lock is on decendent.
- To get S or IS, must hold at least IS on parent.
- To get X, IX, SIX, must hold at least IX on parent.
- Lock Escalation: too many low-level locks automaticly combined into coarser-grained locks. Reduce overhead.
