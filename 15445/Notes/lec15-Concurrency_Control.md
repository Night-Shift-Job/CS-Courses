# Transactions
A transaction is the execution of a sequence of one or more operation.  
Definition:
- Database: A fixed set of named data objects.
- Transaction: A sequence of read and write operations. Start with `Begin` and end with `Commit` or `Abort`.

Correctness criteria: ACID:
- Atomicity: transaction executed all or none
  - Logging: able to undo
  - Shadow Paging: COW, only when txn commits is page able seen by others
- Consistency: Logically correct
  - DB consistency:
    - accurately models the real world and follows integrity constaints.
    - txns in the future sees effects of txns commited in the past
  - Txn consistency: cosistent during dbms execute, not dbms responsibility
- Isolation: interleave txns but still make it appear as if ran one-at-a-time. Concurrency control.
  - Pessimistic: don't let problem happen.
  - Optimistic: assume conflict rare; deal with them after they happen.
- Durability

## Schedule
Serializable Schedule: A schedule that is equivalent to some serial execution of the transactions.  
Two operation are conflict if: by different transactions and one of them is wrt on same object.
- R-W conflict: Unrepeatable read
- W-R conflict: Dirty read
- W-W conflict: Overwriting uncommited data

# Two level of serializability:
## Conflict Serilizability
most try to; and two schedule are conflict equivalent if
- Involve same actions of same transaction.
- Every pair of conflicting action ordered same way.

Use dependency graph to find serilal: One node per txn。
- one edge from T_i to T_j if an operation in from T_i conflict with T_j
- serializable if acyclic

## View Serilizability
none could;
- T reads initail value A in S1 and also reads initial value A in S2.
- T1 reads A writen by T2 in S1 and also reads A writen by T2 in S2.
- T wrt final A in S1, and wrt final A in S2

