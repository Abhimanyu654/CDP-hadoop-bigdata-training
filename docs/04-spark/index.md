# Spark: Architecture, Components, Dependencies, and Operations

## 1. Purpose
Apache Spark is a distributed processing engine for batch, SQL, streaming, and analytics workloads.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **Driver** | Coordinates Hive query lifecycle from parsing through planning, execution, and result delivery. |
| **SparkSession/SparkContext** | Application entry points for Spark SQL and lower-level Spark execution/context configuration. |
| **DAG Scheduler** | Splits jobs into stages at shuffle boundaries and tracks stage dependencies. |
| **Task Scheduler** | Schedules individual tasks on available executors and handles task-level scheduling/retries. |
| **Executors** | Run Spark tasks inside YARN containers. |
| **tasks** | Small units of work executed against data partitions. |
| **shuffle service where configured** | Can support external shuffle preservation when executors are removed; exact behavior depends on Spark/YARN configuration. |
| **event logger.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
1. Application starts
2. driver builds logical/physical work
3. DAG splits work into stages at shuffle boundaries
4. tasks run on executors
5. results or output are returned/written.

## 5. Dependencies and integrations
Uses a cluster manager such as YARN in this course; reads/writes HDFS or other supported storage; can integrate with Hive catalog and Atlas where configured.

## 6. Failure scenarios
- Driver failure
- executor loss
- skew
- shuffle spill
- OOM
- long GC
- insufficient parallelism
- excessive executor allocation.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does Spark solve?
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
