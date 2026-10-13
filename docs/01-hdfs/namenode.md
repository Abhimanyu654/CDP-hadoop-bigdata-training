# NameNode

## Role
The NameNode is the HDFS namespace manager. It tracks directories, file metadata, permissions, file-to-block relationships, and known block locations. It does not store the actual user file bytes.

## Related state
- **FSImage:** persisted namespace snapshot.
- **EditLog:** namespace-changing transactions after the checkpoint.
- **Block reports:** DataNode-provided inventory used to maintain block-location knowledge.
- **Heartbeats:** signals that DataNodes are alive.

## Request flow
Client asks to open/create/list a path -> NameNode checks namespace and permissions -> returns metadata/block locations or records a namespace transaction -> client communicates directly with DataNodes for file bytes.

## HA
In an HA setup, Active serves namespace requests and writes edits to shared edits (commonly JournalNodes via QJM). Standby applies those edits to remain ready. ZKFC and ZooKeeper coordinate failover; fencing prevents an old Active from continuing to act as Active.

## Failure scenarios
- Active down: failover may promote Standby if synchronization, quorum, and fencing are healthy.
- Standby down: Active may continue but HA redundancy is reduced.
- NameNode storage full/corrupt: high-severity metadata risk; follow recovery runbooks and preserve metadata copies.
- DataNode down: NameNode marks it dead after missed heartbeats and may schedule re-replication.

## Interview question
**Does the NameNode store the actual file data?** No. It manages namespace metadata and block locations; DataNodes store the blocks.
