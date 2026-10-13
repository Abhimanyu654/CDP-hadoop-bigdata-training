# CDP & Hadoop Big Data Training

A structured, version-aware learning repository for Cloudera Data Platform (CDP) Private Cloud Base / Cloudera Runtime and the Hadoop ecosystem.

**Goal:** explain each service from an administrator's perspective: purpose, architecture, components/roles, request flow, dependencies, configuration, monitoring, failure behavior, recovery, and interview questions.

> **Version note:** CDP service composition and dependencies vary by Cloudera Runtime release and enabled features. These notes use CDP Private Cloud Base 7.x as the main reference point. Always verify exact role names and dependencies against your deployed release and Cloudera Manager.

## Start here
- [Training home](docs/index.md)
- [Learning roadmap](docs/00-learning-roadmap.md)
- [Service dependency map](docs/00-service-dependency-map.md)
- [Component lesson template](docs/99-lesson-template.md)
- [GitHub publishing and website guide](docs/98-publish-website.md)
- [Training business model](docs/97-training-business.md)

## Curriculum
1. HDFS
2. YARN
3. MapReduce and JobHistory Server
4. Spark
5. Spark on YARN and Spark History Server
6. Hive and Hive Metastore
7. Tez
8. Impala
9. Kerberos
10. Ranger, Ranger KMS, and Ranger RMS
11. Atlas
12. ZooKeeper
13. Kafka
14. Production troubleshooting
15. Additional ecosystem services

## What each full service guide covers
- Service purpose and use cases
- Architecture diagram
- Component/role inventory and function
- End-to-end flow
- Required and optional dependencies
- Key configuration properties
- CLI / admin commands
- Metrics and logs
- Failure scenarios and troubleshooting
- Security considerations
- Interview questions and a spoken-answer summary

## Safe use
Use synthetic examples. Never commit production credentials, keytabs, tokens, private keys, customer data, internal IPs/hostnames, or confidential architecture. Commands must be checked against the exact CDP/Hadoop release and tested in a lab before production use.
