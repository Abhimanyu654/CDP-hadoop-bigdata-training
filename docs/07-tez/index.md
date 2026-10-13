# Tez: Architecture, Components, Dependencies, and Operations

## 1. Purpose
Tez is a DAG execution framework commonly used by Hive-on-Tez to run stages of data processing more efficiently than launching separate MapReduce jobs for every stage.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **Tez client/library** | Builds and submits Tez DAG execution requests. |
| **Tez DAG** | Graph of computation vertices and dependency edges. |
| **vertices** | Groups Tez tasks that perform a processing step. |
| **edges** | Define data movement and dependency between DAG vertices. |
| **Tez ApplicationMaster** | Coordinates DAG execution and requests YARN containers for tasks. |
| **task containers** | Execute tasks for vertices on allocated worker resources. |
| **Tez UI/history integrations depending on deployment.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
1. Hive submits a Tez DAG
2. YARN launches Tez AM
3. AM requests containers
4. tasks execute vertices and exchange data along edges
5. final output is written.

## 5. Dependencies and integrations
YARN schedules containers; HDFS stores input/output; Hive compiles queries into Tez DAGs in Hive-on-Tez deployments.

## 6. Failure scenarios
- AM startup delay
- container allocation
- task retries
- shuffle failures
- vertex failure
- Tez library/config mismatch.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does Tez solve?
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
