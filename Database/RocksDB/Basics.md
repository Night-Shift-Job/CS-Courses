1. [LSM-based Storage Techniques: A Survey](https://arxiv.org/abs/1812.07527)
    - 关于LSM Tree的简介，很好的概览入门材料，比较了LSM sst的两个technique的优缺点。并列举了当时的LSM研究方向（未阅读完）
1. [RocksDB-Overview](https://github.com/facebook/rocksdb/wiki/RocksDB-Overview)
    - Rocksdb 概览，必读，掌握基本知识
    - > performance: The primary design point for RocksDB is that it should be performant for fast storage and for server workloads. It should support efficient point lookups as well as range scans. It should be configurable to support high random-read workloads, high update workloads or a combination of both. Its architecture should support easy tuning of trade-offs for different workloads and hardware.
    - > high level architecture:
        - all data in sorted order
        - 3 basic constructs of RocksDB: memtable, sstfile and logfile
            - memtable: in-mem data, new writes inserted into memtable and optionally written to logfile.
            - logfile: sequentially written file on storage. when memtable fills up, flush to sstfile and delete log file corresponded to.
            - sstfile is sorted
    - > features:
        - column family: partitioning database instances into multiple column families. guarantee users **consistent view** on column families. see [Q: What are column families used for?](https://github.com/facebook/rocksdb/wiki/RocksDB-FAQ)
        - updata: `put` to insert single kv, overwrite if exist. `write` allows multiple kv **atomically** insert, update and delete. `DeleteRange` for range delete.
        - get, iterator and snapshots:
            - kv are **pure byte stream**, no limit of size.
            - `Get` for single get, `MultiGet` for multiple get and its consistent with each other.
            - data in sorted order and method could be customed.
            - `Iterator` allows **range scan**. it could **seek to specified key** and then start scanning one key at a time. **reverse scan** is also supported. Data return by iterator is consistent at the time iterator created.
            - `Snapshot` also create point-in-time view of database, and `Get`&`Iterator` api used to read data from specified snapshot. not persist after db restart.
            - `Iterator` for **short-lived forground scan** while `Snapshot` for **long-running background**. `Iterator` keeps rc of all underlying files until being released. `Snapshot` never prevent file deletion but compaction process understands existence of snapshot and nect delete a key visible in any existing snapshot.
        - transaction: multi-operational transaction supported, both optimistic and pessimistic mode.
        - Prefix Iterator: A hash of prefix is added to Bloom, `Iterator` with specified key-prefix will use Bloom to avoid look into file not contain data.
        - Multithreaded Compaction: remove kv bindings deleted or overwritten and reorganize data.
            - When memtable full, content written out to a file in L0 of LSM-tree, in compaction, goes to next LSM level.
        - Both compaction style store data in a fixed number of logical levels in the database. More recent data is stored in Level-0 (L0) and older data in higher-numbered levels, up to Lmax. Files in L0 may have **overlapping keys**, but files in other levels generally form a single sorted run per level.
            - Level style compaction **optimizes disk footprint** vs. logical database size (space amplification) minimizing files involved in each compaction step: merging one file in Ln with all its overlapping files in Ln+1 and replacing them with new file in Ln+1
            - Universal style compaction **optimizes total bytes written to disk** vs. logical database size (write amplification) by merging potentially many files and levels at once, requiring more temporary space.
            - Universal typically results in lower write-amplification but higher space- and read-amplification than Level Style Compaction.
            - compation method could be customed by users.
        - avoid stall: if all threads are busy, sudden burst of write may fillup memtables quickly and stall writing. **keep a small set of threads explicitly reserved for sole purpose of flushing memtable to storage**.
        - logs: rocksdb write detailed logs to a file named LOG*, user could choose different log level.
        - Data compression: could configure different compression algorithms for data at bottommost level where 90% of data lives.
        - Full backups and replication: provide backup API
        and no replicated(but provide replication helper).