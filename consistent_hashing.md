# Consistent Hashing - Partitioning

One of our goal is to scale out. 
Without proper mechanism of horizontal partitioning it is impossible to achieve. 
That is why we are going to explore Consistent Hashing algorithm and different variations on it in next paragraphs.

Consistent Hashing is the algorithms that based on entry key determines which node of the system keeps information connected to it. Keys are always the sequence of bytes and because of that we use hashing function.

In basic variation of the algorithm every node has assigned one random value on the ring. To determine which node responds to key we count its hash and then find the nearest node on the ring clockwise. In this version of algorithm every node has its dedicated range of key-hashes.

[first version - rys. 3.2]

Basic variant has a number of disadvantages. One of it is potentially not fair distribution of keys between nodes. There is no support for environment heterogeneous - we can't set bigger range of keys to particular more powerful node. Because of that it is quiet common scenario to introduce "virtual nodes" for every single node - we just set additional places on the ring for every node (it's typical that they number it much higher that overall number of nodes). Thanks do that distribution of keys should be more fair. Unfortunately this variation of algorithm does not behave good in practice. While trying to add new node to the system every keys that belong to the node of the selected keys-range has to be migrated. To do that properly we need to traverse every keys within that range and decide which one should be send and which should stay in the file. What is more we also need to synchronize Merkel Tree (we will talsk about them in one of the next chapter). That kind of operation has to be done behind the scene to not break the rules of the database. To make it working properly we need to separate mechanism of data partitioning from making the decision which node will be the host of them.

[second version - rys. 3.3]

In third version of algorithm keys space is divided in advance to a predetermined number of intervals S. Generally this number is much higher that overall number of system nodes N (S >> N). We assign random interval to next node and in the same time make sure that every has S/N. Thanks do that records that belongs do different intervals might have to be persisted in other files. In case of the addition new node to the system we are able to just migrate the file without unnecessary traversal of keys within node.

[third version - rys. 3.4]