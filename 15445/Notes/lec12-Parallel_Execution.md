# Process Model
Defines how system architected to support concurrent requests from multi-user application.  
Worker is responsible for executing tasks on behlf of client and returning the results.

- Process per DBMS Worker: Each worker is separate OS process
  - Relies on OS scheduler.
  - Use shared-memory for global data structure.
  - A process crash doesn't take down whole system.
- Process Pool: A worker uses any process from pool
  - relies on OS scheduler and shared memory
  - bad for CPU cache locality
- Thread per worker: single process with multiple thread
  - DBMS manages its own scheduling
  - may or not use dispatcher thread
  - thread crash kill entire system
  - pros:
    - less overhead per context switch
    - do not have to manage shared memory

# Execution Parallelism
- Inter-query parallelism: multiple query same time; maintain correctness of updating
- Intra-query parallelism: executing operators in parallel
  - Intra-operation(horizontal): decompose operators into independent fragment that **perform same function on different subset of data**.
    - **Exchange operator** inserted into query plan to coalesce/split results from multiple children/parent operators.
      - Gather: combine results from multiple workers into single output stream.
      - Distribute: split single input stream into multiple output streams.
      - Repartition: shuffle multiple input streams across multiple output streams.
  - Inter-operation(vertical): pipelined from one state to next. Workers working on different stage of plan at same time.
  - Bushy Parallelism: hybrid of both.
- problem: adding process/thread won't help is disk is always bottleneck.

# IO Parallelism
Split dbms across multiple storage devices. Configure hardware to store dbms files: storage appliance, RAID configuration.
- Vertical Partition: Store tables attr into separate location. Information needed to reconstruct.
- Horizontal Partition: Divide table into disjoint segments based on partitioning key.
  - Hash partitioning
  - Range partitioning
  - Predicate partitioning
