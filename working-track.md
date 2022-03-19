# Report
This file will be organized in a reverse chronological order，with week record after 7 daily record.

## W11-2022
- DB:
  - [Orca Modular Query Optimizer Architecture](https://15721.courses.cs.cmu.edu/spring2020/papers/19-optimizer1/p337-soliman.pdf)
  - tidb
    - optimizer源码阅读
    - 完成tidb的本机部署，为个人服务器增加ssh服务
- DS
  - MapReduce论文阅读

### Mar 15
- DB:
  - [How We Built a Cost-Based SQL Optimizer](https://www.cockroachlabs.com/blog/building-cost-based-sql-optimizer/)
    - Source code: https://github.com/cockroachdb/cockroach/blob/master/pkg/sql/opt/xform/optimizer.go
  - [SQL 查询优化原理与 Volcano Optimizer 介绍](https://io-meter.com/2018/11/01/sql-query-optimization-volcano/)
    - > 因此，Volcano Optimizer 采取了自顶向下的计算方法，在计算开始， 每棵子树先按照原先的样子计算成本并作为初始结果。在不断应用规则的过程中，如果出现一种新的结构被加入到当前的等价集合中， 且这种等价集合具有更优的成本，这时需要向上冒泡到所有依赖这一子集合的父亲等价集合， 更新集合里每个元素的成本并得到新的最优成本和方案。
  - tidb optimizer源码阅读
- atlas:
  - 2/5 apoc.text
### Mar 14
- Go: finished go tour
- atlas:
  - read planner code
  - 1/5 apoc.text

## W10-2022
### Mar 13
- DB:
  - Finished Columbia paper
### Mar 10
- DB:
  - Finished 15721-Optimizer video
### Mar 9
- DB: decide to finish 15721
  - Re-read [An Overview of Query Optimization in Relational Systems](https://15721.courses.cs.cmu.edu/spring2020/papers/19-optimizer1/chaudhuri-pods1998.pdf)
  - Read vocano-optimizer paper [The Volcano Optimizer Generator: Extensibility and Efficient Search](https://15721.courses.cs.cmu.edu/spring2020/papers/19-optimizer1/graefe-icde1993.pdf)
  - Watching first half of video [15721-Optimizer Implementation (Overview)](https://youtu.be/q4NeEGMoKmc)
  - Tried to think about graph optimizer but no idea(fxxk!)
### Mar 8
- DB
  - Designing data-intensive application: Ch.2 
  - Finished cypher documentation
### Mar 7
- DB
  - Cypher: finished [Beginner Cypher](https://neo4j.com/developer/cypher/)
  - Designing data-intensive application: P34-42
- OS
  - finised [ch5-cpu_exception](https://os.phil-opp.com/cpu-exceptions/)
## W9-2022
### Week Summary
Devote much time on rust-vm and rust-os, restore a feeling of system basics.
### Mar 6
- OS
  - reread doc and add comment to ros [ch1](https://os.phil-opp.com/freestanding-rust-binary/) to [ch3](https://os.phil-opp.com/vga-text-mode/)
  - finshed [ch4](https://os.phil-opp.com/testing/)
### Mar 5
- OS
  - installing riscv qemu
  - finished ros ch1 to ch3
### Mar 4
- Rust
  - [Green Threads Explained in 200lines](https://cfsamson.gitbook.io/green-threads-explained-in-200-lines-of-rust/)
- Trying to find a rust project for practice, but cs140 is archived, maybe qemu first.
### Mar 3
- finished multiple rust materials。including books、gitbooks and leetcode
- working on a tutorial vm based on rust: https://blog.subnetzero.io/categories/iridium/

## W7-2022
### Feb 17
- Start mit 6.824, and finish first lec video.
