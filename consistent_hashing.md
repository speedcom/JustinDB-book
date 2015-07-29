# Consistent Hashing - Partitioning

One of our goal is to scale out. 
Without proper mechanism of horizontal partitioning it is impossible to achieve. 
That is why we are going to explore Consistent Hashing algorithm and different variations on it in next paragraphs.

Consistent Hashing is the algorithms that based on entry key determines which node of the system keeps information connected to it. Keys are always the sequence of bytes and because of that we use hashing function.

In basic variation of the algorithm every node has assigned one random value on the ring. To determine which node responds to key we count its hash and then find the nearest node on the ring clockwise. In this version of algorithm every node has its dedicated range of key-hashes.

[rys. 3.2]

