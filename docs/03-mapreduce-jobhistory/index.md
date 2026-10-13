# MapReduce and JobHistory Server: Architecture, Components, Dependencies, and Operations

## 1. Purpose
MapReduce is a batch processing framework with map, shuffle/sort, and reduce phases. JobHistory Server serves historical job details after completion.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **MR client** | Submits a MapReduce job and its configuration, jar, and input/output details. |
| **ResourceManager** | Accepts the Spark application and coordinates YARN resource allocation. |
| **MRAppMaster** | Coordinates one MapReduce application, schedules map/reduce tasks, and manages retries and task state. |
| **map tasks** | Read input splits and emit intermediate key/value records. |
| **reduce tasks** | Consume shuffled/sorted intermediate records and produce final output. |
| **NodeManager** | Runs on worker nodes, launches and monitors containers, manages local resources/logs, and reports node health/resources. |
| **JobHistory Server** | Serves historical MapReduce job, task, counter, and attempt information; it is not the live scheduler. |
| **history logs.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
1. Submit job
2. YARN launches MRAppMaster
3. map tasks read input
4. shuffle/sort transfers and groups intermediate keys
5. reducers write output
6. history is persisted for UI/API.

## 5. Dependencies and integrations
YARN schedules tasks; HDFS commonly stores input/output and job staging; JobHistory Server is a separate history service and not needed for every task's execution.

## 6. Failure scenarios
- Failed task attempts
- shuffle failures
- container memory errors
- history UI unavailable while jobs continue
- output commit failures.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does MapReduce and JobHistory Server solve?
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
