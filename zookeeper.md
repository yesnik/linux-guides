# Apache Zookeeper

[ZooKeeper](https://zookeeper.apache.org/) is an open-source server for highly reliable distributed coordination of cloud applications. 
It is used in many distributed systems, such as Kafka, Hadoop.

Some of the prime features of Apache ZooKeeper are:

- *Reliable System*: This system is very reliable as it keeps working even if a node fails.
- *Simple Architecture*: The architecture of ZooKeeper is quite simple as there is a shared hierarchical namespace which helps coordinating the processes.
- *Fast Processing*: ZooKeeper is especially fast in "read-dominant" workloads (i.e. workloads in which reads are much more common than writes).
- *Scalable*: The performance of ZooKeeper can be improved by adding nodes.

Main concepts:

- ZooKeeper is like a file system. It internal data structure is like a tree
- Each node is called a *zNode*
- Each *zNode* has a path: /app/orders, app/clients
- Each *zNode* can be *persistent* (it stays alive all the time) or *ephemeral* (it goes away when your app disconnects)
- Eeach *zNode* can store data
- We can't rename *zNode*
- Each *zNode* can be watched for changes
