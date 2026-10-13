# DataNode

## Role
A DataNode stores HDFS block replicas on local disks and serves read/write requests. It reports liveness through heartbeats and inventories through block reports.

## Responsibilities
- Store block files and associated metadata/checksums.
- Serve client reads.
- Participate in write pipelines.
- Replicate blocks when instructed.
- Delete replicas when instructed.
- Report disk/storage health and block inventory.

## Failure behavior
If a DataNode becomes unavailable, clients may read from another replica. The NameNode detects missed heartbeats and may schedule re-replication if valid replicas remain and capacity/policies permit it. If all replicas of a block are lost, the data is unavailable.

## Operations
```bash
hdfs dfsadmin -report
hdfs fsck /path -files -blocks -locations
```
Use Cloudera Manager host and DataNode health metrics for disk failures, volume failures, capacity, and heartbeat status.
