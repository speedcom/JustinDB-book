# JustinDB

### Foundation

The following specification is the recipe for our database. We will try to meet every criteria that are covered here.

** High level architecture:**
* decentralized - each node is equal to other
* symmetry - every node has the same responsibility
* easy to scale out - addition of new node shouldn't take a lot effort on the whole system
* heterogeneous - proper delegation of work to nodes with different computing power

**Techniques:**
* partitioning (Consistent Hashing)
* replication
* versioning records (Vector Clocks)
* data synchronization (Merkle Trees)
* failured detection (Hinted Handoff)

**Data model:**
* Key-Value

**Persistence:**
* Pluggable Backends (Akka Persistence Module)

**Communication protocol:**
* REST/HTTP
* Protocol Buffers Client

**Technology:**
* Scala 
* Akka
