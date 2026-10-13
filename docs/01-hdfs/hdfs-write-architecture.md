# HDFS Write Architecture

```text
Client             NameNode                DN1             DN2             DN3
  | -- create(path) --> |
  | <-- pipeline ------- |
  | ---- packet -------> | DN1 --forward--> DN2 --forward--> DN3
  | <--- ACK chain ------|<------ ACK <------- ACK <---------|
  | -- next block request when needed
  | -- close ------------>
```

1. Client asks NameNode to create a file.
2. NameNode checks permissions and namespace constraints.
3. For each block, NameNode chooses target DataNodes using replication and placement policies.
4. Client streams packets to the pipeline's first DataNode.
5. Each DataNode writes and forwards packets; acknowledgements return upstream.
6. If a pipeline node fails, the client can recover/reconfigure the pipeline according to HDFS protocol.
7. File close finalizes the write state.

The NameNode chooses metadata and targets; the file payload moves through DataNodes.
