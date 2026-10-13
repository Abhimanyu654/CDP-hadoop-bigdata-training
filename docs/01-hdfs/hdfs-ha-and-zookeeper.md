# HDFS HA and ZooKeeper

```text
ZKFC on Active host ----\
                         +---- ZooKeeper ensemble (coordination)
ZKFC on Standby host ---/

Active NameNode ---- writes edits ----> JournalNode quorum
Standby NameNode <--- tails edits ----- JournalNode quorum
```

- **Active NameNode:** serves namespace requests and writes edits.
- **Standby NameNode:** stays synchronized and can take over.
- **JournalNodes:** shared edits in QJM-based HA.
- **ZKFC:** monitors NameNode health and coordinates failover.
- **ZooKeeper:** coordination and leader-election primitives.
- **Fencing:** prevents the old Active from continuing to serve as Active after failover.

One ZooKeeper down in a three-node ensemble normally leaves quorum (2/3). Two down means no majority, so coordination-dependent behavior is impaired. ZooKeeper and JournalNode quorum are separate concerns.
