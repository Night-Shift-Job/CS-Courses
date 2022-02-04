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
