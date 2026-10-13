# CDP Service Dependency Map

This is a conceptual guide, not a universal startup order. Use Cloudera Manager's dependency view and the service dependency table for the exact CDP release deployed.

## Common data-engineering path

```text
Data source
    |
    v
Ingestion (Kafka / NiFi / Sqoop where deployed)
    |
    v
HDFS (NameNode metadata + DataNode block storage)
    |
    +-------------------------+
    |                         |
    v                         v
Hive + Metastore           Spark on YARN
    |                         |
Tez / YARN                 YARN + HDFS
    |
    v
Impala (interactive SQL, shared Hive metadata)
    |
    v
BI / analyst / application

Cross-cutting:
Kerberos (authentication) | Ranger (authorization/audit) |
Ranger KMS (encryption keys) | Atlas (metadata/lineage) |
ZooKeeper (coordination for services that use it) |
Cloudera Manager (deployment, configuration, monitoring)
```

## Typical dependencies and integrations

| Service | Common core dependency | Other common/optional integrations | Important nuance |
|---|---|---|---|
| HDFS | Hadoop runtime | ZooKeeper for HA coordination; Ranger; Ranger KMS for encryption zones | ZooKeeper is not where HDFS block metadata is stored |
| YARN | HDFS in common CDP deployments | ZooKeeper for HA; Ranger | Scheduler, NodeManagers, and applications have separate roles |
| MapReduce | YARN; HDFS for input/output | JobHistory Server for history | JobHistory is not the live scheduler |
| Spark on YARN | YARN | HDFS; Spark History Server; Hive/Atlas integration depending on use | Spark can use other cluster managers in other deployments |
| Hive | HDFS | Hive Metastore, YARN/Tez, Ranger, Atlas, optional integrations | HiveServer2 and the Metastore are distinct roles/services |
| Hive-on-Tez | HDFS, Hive, Tez | YARN; Ranger/Atlas integrations | Tez executes DAGs; YARN allocates resources |
| Impala | HDFS and Hive metadata integration | Ranger, Atlas, Kudu, ZooKeeper/YARN depending on release/features | Impala has its own execution engine; HiveServer2 is not its query engine |
| Ranger | Service plugins and policy store; deployment-specific supporting services | LDAP/identity sync, audit destinations, Solr in some layouts | Plugins enforce policies in target services |
| Ranger KMS | Key provider and its backing key store/configuration | HDFS encryption zones; Ranger policies | Availability is critical for operations needing key access |
| Atlas | Metadata store and configured supporting services | Hive/HBase/Kafka hooks; Solr; Ranger integration | Lineage depends on enabled hooks and supported flows |
| ZooKeeper | None as a consumer service | Used by specific services such as HDFS HA, HBase, legacy Kafka | Losing quorum affects coordination-dependent features, not every service equally |
| Kafka | Broker quorum system | ZooKeeper in legacy deployments; KRaft in modern deployments | Do not assume every Kafka version uses ZooKeeper |
| Spark History Server | Event logs in configured storage | Spark applications write event logs | History server is for completed/running app UI history, not required to execute Spark jobs |

## How to read dependencies
- **Required** means the documented service configuration expects that dependency for the specified deployment.
- **Optional** means an integration or feature is enabled only when configured.
- **Runtime dependency** means a workload may fail when a service is unavailable.
- **Management integration** means a service may continue running but lose policy updates, lineage, audits, or UI functionality.

Always verify with the official service dependency documentation for the exact release: https://docs.cloudera.com/cdp-private-cloud-base/7.3.2/cdp-private-cloud-base-installation/topics/cdpdc-service-dependencies.html
