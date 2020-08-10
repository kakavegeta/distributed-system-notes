#### Linearizability

A history if linearizable if exists total order of operations, matches real time, reads see preceding write in the order. 

No matter how many replicas a system has, it should behave like it only has one copy of data.

strong consistency is almost same as serializable. 



#### WHY ZOOKEEPER?

1)  API for general purpose for coordination service. 

2) could n servers reach n times performance? 



#### Basics

- ZK's performance increase dramatically as you add more servers 

- replicas can serve read-only client requests from their local data, then the total read capacity would be O(#servers) instead of O(1)(read only from leader)

- reads from followers might be not linearizable, and not always yield fresh data:

  replica may not be in majority, so may not have seen a completed write;

  replica may not yet have seen a commit for a completed write;

  replica may be entirely cut off from the leader;

  But Linearizability forbids stale reads!!!

- ZK is not obliged to provide fresh data to reads, it does not provide linearizability

#### ZK guarantees

- Linearizable writes

- FIFO client order

  writes -> client specified order

  reads -> 

  

