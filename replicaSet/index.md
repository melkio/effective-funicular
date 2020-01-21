# Replication

## Redundancy and Data Availability

Replication provides redundancy and increases data availability. With multiple copies of data on different database servers, replication provides a level of fault tolerance against the loss of a single database server.

*In some cases*, replication can provide increased read capacity as clients can send read operations to different servers. Maintaining copies of data in different data centers can increase data locality and availability for distributed applications.

A replica set in MongoDB is a group of `mongod` processes that provide redundancy and high availability. The members of a replica set are:

*Primary*: The primary receives all write operations.  
*Secondaries*: Secondaries replicate operations from the primary to maintain an identical data set. 
*Arbiter*: An arbiter does not have a copy of data set and cannot become a primary. However, an arbiter participates in elections for primary.

You can configure a secondary member for a specific purpose. You can configure a secondary to:

- Prevent it from becoming a primary in an election, which allows it to reside in a secondary data center or to serve as a cold standby. (Priority 0) 
- Prevent applications from reading from it, which allows it to run applications that require separation from normal traffic. (Hidden)
- Keep a running “historical” snapshot for use in recovery from certain errors, such as unintentionally deleted databases. See (Delayed)

## ReplicaSet Oplog

The oplog (operations log) is a special capped collection that keeps a rolling record of all operations that modify the data stored in your databases.  
Each operation in the oplog is idempotent. 

To view oplog status, including the size and the time range of operations, issue the `rs.printReplicationInfo()` method.

## ReplicaSet data sync

In order to maintain up-to-date copies of the shared data set, secondary members of a replica set sync or replicate data from other members. MongoDB uses two forms of data synchronization: initial sync to populate new members with the full data set, and replication to apply ongoing changes to the entire data set.


## Write Concern Specification

Write concern can include the following fields:

```
{ w: <value>, j: <boolean>, wtimeout: <number> }
```