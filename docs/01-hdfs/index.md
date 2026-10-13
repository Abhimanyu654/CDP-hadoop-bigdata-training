# HDFS: Architecture, Components, Dependencies, and Operations

## 1. Purpose
HDFS is the distributed file system used to store large files across DataNodes with fault tolerance. The NameNode manages namespace metadata; DataNodes store block replicas.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **NameNode** | Maintains HDFS namespace metadata, file-to-block mappings, permissions, and block-location knowledge; does not store file payload bytes. |
| **Standby NameNode (HA)** | Tracks shared namespace edits and maintains a synchronized namespace so it can take over during failover. |
| **DataNode** | Stores and serves HDFS block replicas, sends heartbeats/block reports, and participates in replication and write pipelines. |
| **JournalNode (QJM HA)** | Persists shared edit-log transactions for NameNode HA; the Active writes to a quorum and Standby tails the edits. |
| **ZKFC** | Monitors local NameNode health and participates in automatic failover coordination and fencing. |
| **checkpoint role (Secondary NameNode/CheckpointNode/Standby depending on architecture)** | Creates checkpoints by combining namespace image state with later edits; a Secondary NameNode is not automatically a hot standby. |
| **HDFS client.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
2. NameNode metadata RPC
3. block locations
4. direct client/DataNode read, or client
5. NameNode create request
6. DataNode write pipeline
7. acknowledgements
8. close.

## 5. Dependencies and integrations
HDFS clients, YARN/Spark/Hive/Impala and other services use HDFS for data. HA commonly uses JournalNodes for shared edits and ZooKeeper/ZKFC for failover coordination. Ranger and Ranger KMS may be integrated for authorization and encryption.

## 6. Failure scenarios
- Data unavailable
- under-replication
- NameNode/Standby failover
- DataNode disk pressure
- safe mode
- JournalNode quorum loss
- encryption key access failure.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does HDFS solve?
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
