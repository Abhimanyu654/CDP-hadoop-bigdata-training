# ZooKeeper: Architecture, Components, Dependencies, and Operations

## 1. Purpose
ZooKeeper is a coordination service offering a replicated namespace and primitives such as znodes, sessions, watches, and leader election.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **ZooKeeper server ensemble** | Replicated coordination service with voting members that establish quorum. |
| **leader** | Orders writes and coordinates replication among voting members. |
| **followers** | Replicate ordered transactions and serve supported client requests. |
| **optional observers** | Can serve reads but do not vote in quorum. |
| **client sessions** | Represent client connections/leases; ephemeral nodes are tied to session lifetime. |
| **znodes** | Hierarchical coordination records, not a general-purpose bulk data store. |
| **transaction logs** | Persist ZooKeeper transactions for recovery. |
| **snapshots.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
1. Client connects to ensemble
2. reads/writes coordination data
3. leader orders writes
4. quorum acknowledges
5. followers replicate
6. watches notify clients of changes.

## 5. Dependencies and integrations
Used by services that explicitly integrate with ZooKeeper, such as HDFS HA coordination, HBase, and legacy Kafka deployments. Modern Kafka may use KRaft instead.

## 6. Failure scenarios
- One server down in a 3-node ensemble: quorum usually remains. Two down: quorum lost. Impact is on ZooKeeper-dependent coordination
- not automatically every cluster service.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does ZooKeeper solve?
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
