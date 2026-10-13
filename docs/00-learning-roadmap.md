# Learning Roadmap

## Recommended sequence

| Phase | Module | What to master |
|---|---|---|
| 1 | HDFS | NameNode, DataNode, blocks, replication, read/write, HA, JournalNodes, ZKFC, FSImage, EditLog, checkpoints |
| 2 | YARN | ResourceManager, Scheduler, NodeManager, ApplicationMaster, containers, queues, recovery |
| 3 | MapReduce + JobHistory | Map/Reduce, shuffle/sort, MRAppMaster, task attempts, JobHistory Server |
| 4 | Spark | Driver, executors, DAG, jobs/stages/tasks, shuffle, memory, dynamic allocation |
| 5 | Spark on YARN | Client vs cluster mode, application submission, executor allocation, history server |
| 6 | Hive + Metastore | HiveServer2, Driver, compiler, optimizer, HMS, managed/external tables, partitions |
| 7 | Tez | DAG, AM, vertices, tasks, Hive-on-Tez execution |
| 8 | Impala | impalad, coordinator/executor, catalogd, statestored, metadata, admission control |
| 9 | Kerberos | KDC, principals, tickets, keytabs, renewal, delegation tokens |
| 10 | Ranger/KMS/RMS | Admin, plugins, policies, audits, encryption keys, release-specific RMS role |
| 11 | Atlas | metadata, entities, classifications, lineage, hooks, governance |
| 12 | ZooKeeper | ensemble, quorum, leader/follower, znodes, sessions, watches, failure cases |
| 13 | Kafka | brokers, topics, partitions, replicas, ISR, consumer groups, KRaft vs ZooKeeper |
| 14 | Operations | service dependencies, logs, monitoring, disk pressure, incidents, recovery |
| 15 | Supporting services | Oozie, Sqoop, Hue, Solr, HBase, Livy, NiFi, Ozone, Kudu, Zeppelin |

## Study method
For each module:
1. Draw its architecture from memory.
2. Explain every component's responsibility.
3. Trace a successful request from client to storage/compute and back.
4. Explain what fails if each critical component stops.
5. Run safe lab commands and record expected output.
6. Practice the interview questions aloud.
7. Note the exact product version and official reference used.
