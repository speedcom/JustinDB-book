# JustinDB

### Foundation

HLA:
* decentralized - each node is equal to other
* symmetry - every node has the same responsibility
* easy to scale out - addition of new node shouldn't take a lot effort on the whole system
* heterogeneous - proper delegation of work to nodes with different power

Data model:
* Key-Value

Techniques:
* partitioning (Consistent Hashing)
* replication
* data synchronization (Merkle Trees)
* versioning records (Vector Clocks)
* failured detection (Hinted Handoff)

Communication protocol:
* REST/HTTP
* Protocol Buffers Client
* no-authorisation

Persistence:
* Pluggable Backends (Akka Persistence Module)

Technology:
* Scala 
* Akka

Repository:
[GitHub](https://github.com/speedcom/JustinDB)
