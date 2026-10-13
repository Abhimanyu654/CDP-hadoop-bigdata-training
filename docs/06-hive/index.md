# Hive and Hive Metastore: Architecture, Components, Dependencies, and Operations

## 1. Purpose
Hive provides SQL/data warehouse functionality. Hive Metastore stores table schemas, partitions, locations, and other metadata; it does not store the table's actual rows.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **HiveServer2** | Client-facing JDBC/ODBC endpoint that manages sessions and query operations. |
| **session/operation manager** | Tracks Hive client sessions and query operations. |
| **Driver** | Coordinates Hive query lifecycle from parsing through planning, execution, and result delivery. |
| **parser/compiler** | Checks SQL and translates it into a logical execution plan. |
| **optimizer** | Improves the plan using available metadata/statistics and transformations such as predicate/partition pruning. |
| **execution engine** | Submits/coordinates the chosen execution framework such as Tez. |
| **Hive Metastore service** | Provides table/database/partition metadata APIs to Hive and integrated engines. |
| **metastore backing RDBMS** | Persists Metastore records; it is separate from the HDFS table data. |
| **Beeline/JDBC clients.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
2. HiveServer2
3. parse/compile/optimize
4. consult Metastore
5. choose engine such as Tez
6. YARN allocates resources
7. engine reads/writes HDFS
8. result to client.

## 5. Dependencies and integrations
HDFS is common storage. Hive Metastore needs a supported backing database. Hive-on-Tez uses Tez and commonly YARN. Ranger/Atlas integrations are optional/configuration-dependent. Impala shares Hive metadata.

## 6. Failure scenarios
- HS2 down
- Metastore unavailable
- DB connection pool exhausted
- stale stats
- YARN queue full
- incompatible schema/format
- permissions denied.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does Hive and Hive Metastore solve?
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
