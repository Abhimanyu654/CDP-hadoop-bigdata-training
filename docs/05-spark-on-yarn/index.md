# Spark on YARN and Spark History Server: Architecture, Components, Dependencies, and Operations

## 1. Purpose
Spark on YARN runs Spark applications using YARN for resource allocation. Spark History Server displays application event history.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **Spark submit client** | Validates submission settings and submits the application to YARN. |
| **Driver** | Coordinates Hive query lifecycle from parsing through planning, execution, and result delivery. |
| **ResourceManager** | Accepts the Spark application and coordinates YARN resource allocation. |
| **ApplicationMaster (role differs by deploy mode)** | Coordinates YARN-side application resources; driver placement and AM responsibilities vary by Spark deploy mode/version. |
| **NodeManagers** | Launch and monitor Spark driver/executor containers on worker nodes. |
| **Executors** | Run Spark tasks inside YARN containers. |
| **event log directory** | Stores Spark event logs for the History Server to reconstruct application UI. |
| **Spark History Server.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
1. spark-submit
2. YARN application
3. driver placement depends on client/cluster mode
4. executors allocated as containers
5. Spark tasks execute
6. event logs written
7. History Server renders UI.

## 5. Dependencies and integrations
Requires YARN for this deployment mode. HDFS is commonly used for staging, data, and event logs. History Server requires accessible event logs but is not the execution engine.

## 6. Failure scenarios
- YARN queue pending
- executor allocation failure
- driver unreachable
- event logs missing
- History Server down while computation may continue.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does Spark on YARN and Spark History Server solve?
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
