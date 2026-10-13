# Supporting CDP Ecosystem Services

Not every cluster installs every service. Confirm the service list and role assignments in Cloudera Manager.

| Service | Purpose | Typical relationships |
|---|---|---|
| Cloudera Manager | Install/configure/manage services; monitor hosts and roles | Management plane for the cluster |
| Oozie | Workflow scheduling/orchestration in deployments that use it | YARN; may orchestrate Hive/Spark/Sqoop jobs |
| Sqoop | Bulk data transfer between relational databases and Hadoop in legacy deployments | JDBC, MapReduce/YARN, HDFS/Hive |
| Hue | Web UI for supported Hadoop services | HDFS, Hive, Impala, Oozie and others depending on configuration |
| Solr | Search/indexing service; also used as supporting infrastructure in some CDP layouts | HDFS/ZooKeeper depending on deployment; Atlas may use Solr |
| HBase | Distributed column-family database | HDFS and ZooKeeper in common architectures |
| Livy | REST service for Spark sessions/jobs | Spark on YARN and YARN |
| NiFi | Dataflow automation and data ingestion | Source/target connectors; clustered coordination varies by version |
| Ozone | Distributed object store for Hadoop ecosystems | Ozone Manager, Storage Container Manager, Ozone DataNodes |
| Kudu | Columnar storage engine for analytics | Kudu masters/tablet servers; often queried by Impala |
| Zeppelin | Notebook UI with interpreters | Spark/YARN/HDFS integrations depending on interpreter setup |
| Schema Registry | Schema management for supported streaming formats | Kafka ecosystem |
| Streams Messaging Manager | UI/management for supported Kafka clusters | Kafka and configured supporting services |
| Spark History Server | UI for Spark event logs | Event-log storage; not Spark execution engine |
| MapReduce JobHistory Server | History for completed MapReduce jobs | Job history files; separate from YARN ResourceManager |

Create a dedicated detailed lesson for any service that is installed in your cluster or is part of the training curriculum.
