# CDP & Hadoop Big Data Training

Welcome to a practical guide to Hadoop and the Cloudera Data Platform ecosystem.

## Learn by service
Start with storage and resource management, then learn the engines that use them, followed by security, governance, streaming, and production operations.

- [Learning roadmap](00-learning-roadmap.md)
- [Service dependency map](00-service-dependency-map.md)
- [HDFS](01-hdfs/index.md)
- [YARN](02-yarn/index.md)
- [MapReduce and JobHistory Server](03-mapreduce-jobhistory/index.md)
- [Spark](04-spark/index.md)
- [Spark on YARN and Spark History Server](05-spark-on-yarn/index.md)
- [Hive and Hive Metastore](06-hive/index.md)
- [Tez](07-tez/index.md)
- [Impala](08-impala/index.md)
- [Kerberos](09-kerberos/index.md)
- [Ranger, Ranger KMS, and Ranger RMS](10-ranger-kms-rms/index.md)
- [Atlas](11-atlas/index.md)
- [ZooKeeper](12-zookeeper/index.md)
- [Kafka](13-kafka/index.md)
- [Production troubleshooting](14-production-troubleshooting/index.md)
- [Supporting services](15-supporting-services/index.md)

## How to study
For each component, be able to answer: What does it do? Where does it run? What depends on it? What happens if it stops? How do I check health? What logs/configurations matter? How do I explain its architecture in two minutes?

## Version caveat
This is a learning guide, not a substitute for Cloudera's support matrix or operational runbooks. Dependencies differ by release, deployment type, and enabled features.


                 YOUR LEARNING WEBSITE
                         │
              ┌──────────┴──────────┐
              │                     │
          Big Data                DevOps
              │                     │
       Hadoop / Spark          AWS / K8s
       Kafka / Hive            Terraform
       Kerberos                CI/CD
              │                     │
              └──────────┬──────────┘
                         │
                    Free Learning
                         │
                  Future Premium
