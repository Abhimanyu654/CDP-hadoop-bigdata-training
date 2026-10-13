# YARN: Architecture, Components, Dependencies, and Operations

## 1. Purpose
YARN manages cluster compute resources and schedules application containers.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **ResourceManager** | Accepts the Spark application and coordinates YARN resource allocation. |
| **Scheduler** | ResourceManager subcomponent that allocates containers according to configured queues, capacities, priorities, labels, and policies. |
| **ApplicationsManager** | Handles application submission/admission and manages ApplicationMaster lifecycle at the ResourceManager level. |
| **NodeManager** | Runs on worker nodes, launches and monitors containers, manages local resources/logs, and reports node health/resources. |
| **ApplicationMaster per application** | Coordinates one application's execution and negotiates containers from ResourceManager. |
| **Container** | A resource allocation on a NodeManager, typically including memory, vcores, and a launch context. |
| **JobHistory is separate for MapReduce history.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
1. Client submits application
2. ResourceManager accepts
3. ApplicationMaster starts
4. negotiates containers
5. NodeManagers launch tasks/executors
6. application reports status.

## 5. Dependencies and integrations
Commonly uses HDFS for application data and staging; Spark/Tez/MapReduce use YARN in these deployment modes. ZooKeeper can be used for ResourceManager HA; Ranger may integrate for authorization.

## 6. Failure scenarios
- Pending applications
- queue capacity
- NodeManager lost
- AM restart
- container killed for memory
- ResourceManager failover.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does YARN solve?
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
