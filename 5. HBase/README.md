---
title: HBase
updated: 2023-10-31 04:09:50Z
created: 2023-10-31 03:28:57Z
---

# Overview
1. Why we needed HBase?
2. What is Apache HBase?
3. Difference between HDFS and HBase
4. HBase Storage
5. features of HBase
***
# 1. Why we needed HBase?
1. Initially we relied on sequential databases, as the data could only be accesssed in a sequential manner (RDBMS)
2. A huge dataset will be processed sequentially in Hadoop.
3. The limitation was that if we stored a huge dataset over Hadoop, even to query a small part of the data. We had to scan through the entire HDFS to fetch for the smallest record. 
4. Hadoop did not provide random access to the databases. 
5. To solve this problem HBase was introduced. 
6. HBase is similar to Database Management Systems but it provides us the capability to access data in a random way. 
***
# 2. What is HBase?
HBase is a distributed column oriented database built on top of Hadoop. It is horizontally scalable. 

- HBase is very much similar to Google's Big Data Table. 
- Designed to provide quick random access to huge amounts of structured data. 
- It leverages the fault tolerance provided by Hadoop File System and is a part of Hadoop ecosystem that provides random real time read/write access to the data in HDFS.

"**HBase** is an open-source non-relational distributed database written in Java. It is developed as part of Apache Software Foundation's Apache Hadoop and runs on top of **HDFS**"
***
# 3. Differences between HBase and HDFS

| Feature | HDFS | HBase |
|---------|------|-------|
| Type | Distributed file system | NoSQL database |
| Use case | Storage of large files | Real-time read/write access to large datasets |
| Data Model | File system | Columnar |
| Storage | Stores data as files | Stores data in tables |
| Scalability | Highly scalable for large files | Highly scalable for large datasets |
| Consistency | Provides strong consistency | Eventual consistency |
| Access | Batch processing | Random real-time read/write access |
| Access Control | Basic access controls | Access control lists and cell-level security |
| Data Storage | Designed for efficient data storage | Optimized for real-time read/write operations |
| Fault Tolerance | Replicates data across multiple nodes | Provides fault tolerance through replication |
| Operations | Suitable for sequential read/write operations | Suitable for random read/write operations |
| Use in Hadoop ecosystem | Core component of Hadoop | Integrates with Hadoop for real-time data processing |
***
# 4. Apache HBase Storage Mechanism
HBase, as a column-oriented database, offers a distinct data organization model that contrasts with traditional row-oriented databases. In HBase, data is stored in a manner that facilitates the efficient retrieval of specific columns across multiple rows. Here's an expanded explanation of HBase's column-oriented approach and its table schema:

1. **Column-Oriented Database:** Unlike row-oriented databases, where data is stored and retrieved by rows, HBase stores data in a column-oriented fashion. This means that values within a column are stored contiguously on disk, allowing for faster read and write operations, especially when there's a need to access a specific column across multiple rows.

2. **Sorted by Row:** In HBase, tables are sorted by row keys. This sorting mechanism allows for efficient data retrieval based on the row key, as HBase is optimized for the fast lookup of individual rows or ranges of rows.

3. **Table Schema and Column Families:** HBase's table schema is defined primarily by column families. A column family is a group of columns that share the same configuration settings and are stored together on disk. Each column family can have multiple columns. The key-value pairs in HBase are organized into column families, and each column family can contain an arbitrary number of columns.

4. **Dynamic Column Addition:** HBase allows dynamic addition of columns within a column family without modifying the existing data or schema. This feature enables the flexible addition of new data fields without any structural changes, making it suitable for applications with evolving data requirements.

5. **Sparse Data Storage:** HBase's column-oriented storage mechanism is particularly well-suited for handling sparse data, as it can efficiently store and retrieve data with many null or missing values. This capability makes it advantageous for scenarios where the data is not fully populated and can vary significantly between rows.

Overall, HBase's column-oriented approach, combined with its schema flexibility and support for sparse data, makes it a powerful tool for handling large-scale datasets, especially in use cases that demand quick access to specific data points within a distributed and scalable environment.

![456f1f1d63bb641032fa3646c83f13eb.png](/_resources/456f1f1d63bb641032fa3646c83f13eb.png)


Apache HBase, a distributed, scalable, and non-relational database built on top of the Hadoop Distributed File System (HDFS), employs a unique storage mechanism to handle vast amounts of sparse data. Its storage mechanism revolves around a few key components:

1. **HFile:** HBase uses HFiles, which are immutable and sorted data files that store data in the form of key-value pairs. These files are the fundamental storage unit in HBase and are organized based on a block-index structure to facilitate efficient data retrieval.

2. **MemStore:** In HBase, the MemStore serves as an in-memory write cache for incoming data before it gets flushed to disk. It accumulates data in memory and periodically flushes it to the underlying HFiles, reducing the frequency of disk writes and enhancing overall performance.

3. **WAL (Write-Ahead Log):** HBase relies on the Write-Ahead Log, which records all data modifications before they are applied to the MemStore. This log provides durability by ensuring that data changes are not lost in the event of a system failure.

4. **HBase Master and Region Servers:** The HBase architecture includes the HBase Master, responsible for managing metadata and coordinating the cluster, and the Region Servers, which handle read and write requests for specific regions of data. These components work in conjunction to distribute and manage data across the cluster.

By employing this storage mechanism, HBase optimizes data storage, retrieval, and management, making it suitable for applications requiring real-time read and write access to large-scale datasets, especially in use cases involving time-sensitive data analysis, monitoring, and complex querying.
***
# 5. HBase Features
1. Linearly Scalable
2. Automatic Failure Support
3. Consistent Read/Write
4. Hadoop Integration
5. JAVA  API Support
6. Data Replication
***
# 6. HBase Architecture
![a1626ee2338279f37ca4d0c92fb19fdf.png](/_resources/a1626ee2338279f37ca4d0c92fb19fdf.png)

3 basic components:
1. A client library 
3. A master server
	-  Assigns regions to the region servers and takes help of Apache Zookeper
	-  Handles load balancing of the regions across region servers.
	-  Maintains the state of the cluster by negotiating the load balancing.
5. Region servers
	- Regions are nothing but the tables which are split up and spread across the region servers	
	- Communicate with the client and handle data-related operations. 
	- Handle read/write requests for all the regions under it
	- Decide the size of the region by following the region size thresholds.

## Regions
![04869e38511925d4c6a81e40c95341e3.png](/_resources/04869e38511925d4c6a81e40c95341e3.png)

Mem-store is like cache memory - anything that is entered into the HBase is automatically stored as it is. Later the data is transferred and saved into Hfiles as blocks and the Mem Store is erased out. 

## Zookeeper
1. Zookeeper is an open-source project that provides services like maintaining configuration.
2. Zookeeper has ephemeral nodes representing different region servers.
3. Clients communicate with region servers via zookeeper.
***
# Let's go!!

1. Launch HBase in Cloudera terminal
`hbase shell`
2. `status` This gives us info about master, backup master, region servers and many more. In Standalone mode you'll see that you have 1 active master and 1 active server.
3. `version` return the version of HBase we are using
4. `table_help` which will show us various operations one can perform using the table
5. `whoami` returns info related to local system
***
# Major commands
Hbase_exercise.pdf