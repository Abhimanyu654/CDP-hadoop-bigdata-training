# Kafka: Architecture, Components, Dependencies, and Operations

## 1. Purpose
Kafka is a distributed event streaming platform storing ordered records within partitions and replicating them across brokers.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **Producer** | Publishes records to topic partitions. |
| **broker** | Stores partitions and serves producer/consumer requests. |
| **topic** | Logical stream name whose data is split into partitions. |
| **partition** | Ordered append-only log; ordering is guaranteed within a partition. |
| **leader/follower replicas** | Leader serves partition traffic; followers replicate data. |
| **ISR** | Set of replicas considered in sync with the partition leader. |
| **consumer group** | Coordinates consumers so partitions are assigned across group members. |
| **group coordinator** | Manages consumer group membership and offset coordination. |
| **controller/quorum** | Manages Kafka cluster metadata/control-plane state; KRaft uses controller quorum, while legacy clusters used ZooKeeper. |
| **ZooKeeper in legacy architecture or KRaft quorum in modern architecture.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
1. Producer sends record
2. partition leader appends
3. replicas replicate according to configuration
4. acknowledgement returned
5. consumer group reads records and commits offsets.

## 5. Dependencies and integrations
Uses ZooKeeper only in older ZooKeeper-based Kafka architectures; modern Kafka uses KRaft. Ranger, Schema Registry, and monitoring integrations are optional.

## 6. Failure scenarios
- Broker failure
- under-replicated partitions
- ISR shrink
- consumer lag
- rebalance
- disk full
- controller/quorum issues.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does Kafka solve?
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
