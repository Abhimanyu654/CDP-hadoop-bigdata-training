# HDFS Read Architecture

```text
Client                 NameNode                   DataNodes
  | -- open(path) -------> |
  | <-- block locations -- |
  | ---------------- read block 0 -------------> DN2
  | <--------------- data bytes ---------------- |
  | ---------------- read block 1 -------------> DN5
  | <--------------- data bytes ---------------- |
  v
Application receives file bytes
```

1. Client requests file metadata and block locations.
2. NameNode returns replica locations, ordered according to placement/locality logic.
3. Client selects a replica and reads data directly.
4. Client verifies checksums.
5. If a replica fails, client may retry another replica and report the bad replica.
6. NameNode is not in the data path for the file bytes.

Interview point: NameNode outage can prevent new metadata lookups, even though existing established DataNode streams may behave differently depending on timing/client state. Do not claim all active reads instantly stop in every case.
