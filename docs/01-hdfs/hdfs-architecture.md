# HDFS Architecture

## Purpose
HDFS stores large files across multiple DataNodes. It splits files into blocks and replicates blocks to provide fault tolerance. The NameNode manages the namespace and block-location metadata.

## Architecture
```text
                           HDFS client
                               |
                   metadata RPC to NameNode
                               |
                 +-------------+-------------+
                 |                           |
            Active NameNode             Standby NameNode
                 |                           ^
                 | writes edits              | tails shared edits
                 v                           |
           JournalNode quorum (JN1/JN2/JN3 in a typical HA setup)

HDFS client -------------- data stream --------------> DataNodes
                                                    DN1 DN2 DN3
                                                    block replicas

ZKFC on NameNode hosts <---- coordination ----> ZooKeeper ensemble
```

## Components
| Component | Responsibility | Failure significance |
|---|---|---|
| NameNode | Namespace, inode metadata, file-to-block mapping, block locations, permissions | Namespace service unavailable unless HA failover succeeds |
| DataNode | Stores and serves block replicas; heartbeats and block reports | Replicas may become unavailable; HDFS may re-replicate |
| Standby NameNode | Tracks edits and maintains namespace state for failover | No ready standby if it falls behind or fails |
| JournalNode | Shared edits in QJM HA | Loss of quorum can prevent safe namespace edits |
| ZKFC | Monitors NameNode and participates in automatic failover | Failover automation can be impaired |
| ZooKeeper | Coordination/leader-election primitives for HA | Quorum loss affects coordination |
| Checkpoint role | Merges FSImage and edits into a checkpoint, depending on architecture | Checkpoint lag can grow metadata recovery time |

## HDFS read flow
1. Client requests file metadata/block locations from NameNode.
2. NameNode returns locations for the file's blocks.
3. Client chooses a suitable replica, typically favoring network locality.
4. Client reads bytes directly from DataNode.
5. Client verifies checksums and can retry another replica if a read fails.
6. Client requests locations for later blocks as needed.

## HDFS write flow
1. Client requests file creation from NameNode.
2. NameNode checks permissions, path, and namespace rules.
3. NameNode selects a DataNode pipeline for a block.
4. Client streams packets to the first DataNode; it forwards packets down the pipeline.
5. Acknowledgements return through the pipeline.
6. On block completion, the client obtains a pipeline for the next block if needed.
7. Client closes the file; NameNode records the final namespace state.

## Key distinction
The NameNode does not normally proxy file bytes. The client exchanges metadata with the NameNode and streams data directly to/from DataNodes.

## Commands
```bash
hdfs dfs -ls /data
hdfs dfs -put ./sample.csv /data/
hdfs dfs -get /data/sample.csv .
hdfs dfs -df -h /
hdfs dfsadmin -report
hdfs fsck /data -files -blocks -locations
hdfs haadmin -getServiceState nn1
```
Check command availability and impact in the target release. Scope `fsck` carefully on large production filesystems.
