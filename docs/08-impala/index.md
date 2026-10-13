# Impala: Architecture, Components, Dependencies, and Operations

## 1. Purpose
Impala is a distributed SQL engine optimized for interactive analytics. It has its own query execution engine and commonly shares table metadata with Hive through the Hive Metastore.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **impalad (can act as coordinator and executor)** | Long-running Impala daemon; a query coordinator plans and coordinates work, while daemons also execute fragments. |
| **catalogd** | Maintains and distributes Impala catalog metadata and coordinates metadata updates with the Hive Metastore. |
| **statestored** | Distributes cluster membership and daemon state updates to impalad processes. |
| **Hive Metastore** | Shared table metadata service used by Impala for schemas, partitions, and locations. |
| **Impala shell/JDBC/ODBC clients** | Submit SQL to an Impala coordinator. |
| **admission control components depending on configuration.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

## 3. Conceptual architecture
```text
Client / workload
       |
       v
Service entry point / coordinator
       |
       +---- metadata, scheduling, or policy services
       |
       v
Workers / executors / storage nodes
       |
       v
Data, results, logs, or audit events
```

## 4. End-to-end flow
1. Client
2. coordinator parses/optimizes
3. metadata from local catalog cache/catalogd and HMS as needed
4. fragments distributed to impalad executors
5. HDFS/Kudu scan
6. partial results
7. coordinator returns result.

## 5. Dependencies and integrations
HDFS and Hive metadata integration are core for HDFS tables. Ranger/Atlas/Kudu and ZooKeeper/YARN integrations depend on configuration and release. HiveServer2 is not Impala's execution engine.

## 6. Failure scenarios
- Coordinator/executor failure
- stale metadata
- admission queue
- memory limit
- slow scan
- HMS/catalog outage
- statestore outage.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does Impala solve?
2. Explain the architecture and the responsibility of each major role.
3. What services does it depend on, and which integrations are optional?
4. What happens if a key role stops?
5. How would you troubleshoot a slow or failed request?

## 9. Interview summary
Explain the service's purpose, its main roles, request flow, core dependencies, and one realistic failure scenario in 60–120 seconds.

## 10. Version and references
- Target scope: CDP Private Cloud Base / Cloudera Runtime 7.x, unless a lesson says otherwise.
- Verify role names, supported versions, and dependency status in Cloudera Manager and official documentation.
- Do not treat this overview as a production runbook.
