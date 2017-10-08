---
title: "Distributed systems for Fun and Profit"
date: 2017-10-08
draft: false
tags: ["distributed-systems", "paxos", "byzantine-problem", "raft"]
---
# Distributed systems for Fun and Profit

## Partitioning:

1. Divide data into smaller independent subsets thereby reducing impact of dataset growth.
2. Improves performance by limiting the amount of data to be examined and locating required data within the subset
3. Improves availability as the nodes can fail independently
4. Partition is very application specific

## Replication:

1. Copies of same data over multiple machines to make available more bandwidth and computation
2. Provides more availability as nodes can fail independently
3. Since there will be multiple copies of a Data, need for good consistency model is required.
    1. Strong Consistency: Lets you program as though no replication is happening
    2. Weak Consistency: Leads to low latency and higher availability

Abstractions, fundamentally, are fake. Every situation is unique, as is every node. But abstractions make the world manageable: simpler problem statements - free of reality - are much more analytically tractable and provided that we did not ignore anything essential, the solutions are widely applicable.

## A Systems Model:

1. Key Properties
    1. Programs run concurrently on independent node
    2. No shared memory or share clock
    3. Network between may introduce message loss or nondeterminism

2. Implications
    1. Programs have fast access to local state but global state maybe outdated
    2. Messages can be delayed/lost
    3. Programs run concurrently

A System model is a set of assumptions about environments and facilities underneath a Distributed System.
A robust system model is one that makes the weakest assumptions: any algorithm written for such a system is very tolerant of different environments, since it makes very few and very weak assumptions.

## Nodes:
Node serves as host to make our program computations and storages and has
1. Volatile and non-volatile memory
2. a clock(may not be accurate)
3. ability to run a program

Nodes use deterministic algorithms i.e. local computation, local state after computation, and messages sent are determined uniquely by the message received and local state when the message was received.

Most nodes assume crash-recovery failure model where in nodes can only fail by crashing and can recover at some later point.

### Byzantine Fault tolerance:
1. Nodes can misbehave or fail arbitrarily.
2. This is not employed in commercial systems due to its high computations and costs.

### Communication Links:
These are the links that connect nodes and allows each node to send/receive messages. Most of the distributed algorithm books assumes that links follow FIFO for message passing. A Network partition is occurs when the link between nodes is broken but the nodes still remains operational. During this, messages might get lost or delayed indefinitely. Also, the nodes must be treated differently from crashed nodes.

### Time/Order:
Each node can receive a same message at a different time due to distances between each node.
1. Synchronous System model:
    1. Fixed delays
    2. Accurate clocks
2. Asynchronous System Models:
    1. No reliance on times
    2. absent clocks

### Consensus Problem:
1. Agreement: Every node must agree on same value
2. Integrity: Agreed value mush have been chosen by one of the processes
3. Termination: All process must reach decision
4. Validity: All processes must use the same value

### Impossibility Results:
1. FLP impossibility is use by those who design the distributed systems
2. CAP theorem result is used by practitioners who want choose a System design to use.

#### FLP result
1. For async system model
2. Assumes Node fails by crashing, network is reliable and unbound on message delay applies
3. Under these, then can exist no algorithm as it cannot decide on message delays there by imposing restrictions on system design

#### CAP theorem
1. Assume network failure than node failure
2. Can simultaneously satisfy 2 of the 3 properties
    1. Consistency: Data remain constant across nodes
    2. Availability: Node failures doesn’t prevent operational nodes to fail
    3. Partition Tolerance: System continues to operate despite message loss due to network/node failure
3. The CA and CP system designs both offer the same consistency model: strong consistency. The only difference is that a CA system cannot tolerate any node failures; a CP system can tolerate up to f faults given 2f+1 nodes in a non-Byzantine failure model (in other words, it can tolerate the failure of a minority f of the nodes as long as majority f+1 stays up).
4. First, that many system designs used in early distributed relational database systems did not take into account partition tolerance (e.g. they were CA designs)
5. Second, that there is a tension between strong consistency and high availability during network partitions .
6. Third, that there is a tension between strong consistency and performance in normal operation.
7. Fourth - and somewhat indirectly - that if we do not want to give up availability during a network partition, then we need to explore whether consistency models other than strong consistency are workable for our purposes.

#### Consistency model
A contract between programmer and system, wherein the system guarantees that if the programmer follows some specific rules, the results of operations on the data store will be predictable.

  1. The “C” in CAP is “strong consistency”
  2. Linearizable Consistency Model:
      1. That writes should be instantaneous and post write, all reads should give latest written value
  3. Serializable Consistency:
      1. applies serial set of operations as long as system doesn’t break any rules at individual nodes and order is same on all nodes
  4. Strict serialization:
      1. Mix of Linearizable and serializable consistency models
  5. Other consistent Models:
      1. Client Centric consistency:
          1. Client never sees an older version of the Value
          2. Usually achieved with Memcache. So when primary node fails, the cached version is served until the other latest version is written to the next primary
      2. Eventual Consistency:
          1. That client will agree on value after some undefined amount of time given the value is unchanged.
          2. How long is eventually ?
          3. If going with time stamp as the latest value, any node with wrong clock will give undesired results.
  6. Lamport and Vector Clocks:
      1. Interesting read at - https://en.wikipedia.org/wiki/Vector_clock

## Primary/backup replication:
1. Asynchronous
    1. primary just waits for update and commit to backup is done async
    2. failure at backup —> data loss if the primary fails before ack to client and after updating(not committed) backup
2. Synchronous
    1. Primary waits till ack is received from backup
    2. Will have a data loss if the backup gives ack but primary fails post backup ack

### 2PC replication;
1. Most relational DBs use this form
2. This is CA and any partition failures, the system has to wait till the partition recovers.
3. Assumptions - Failures always recover
4. First phase includes just getting an update from backups and backups store it in a temporary area
5. The update is committed in the second(commit) phase. If primary fails, then the backups know to recover
6. Data loss is possible if the data is corrupted during the failover
7. Is latency sensitive due n-n update/ack

### Partition tolerant consensus algorithms:
1. Paxos
2. Raft

#### Network partition
1. Node failure is different from network partition between nodes
2. It is not possible to discover node failure/network partition
3. Updates can only happen based on the votes
4. Use odd number of patrons to get clear majority votes.
5. System can still handle updates  during network partition if (n/2+1) nodes are active

#### Roles
1. System can be designed to have all nodes with single role or separate distinct role
2. Consensus algorithms raft/paxos uses distinct role (master / slave)
3. During normal operations, one is master and rest are acceptors/slave
4. leader is elected at the start and during a failover

#### Epochs
1. A period of normal operation is called Epoch in paxos and term in raft
2. During an epoch, election takes place and leader is designated
3. if not leader is elected, the epoch ends immediately
4. partitioned nodes will have smaller epoch time than current ones and their commands are ignored.
