# JournalNodes

## Purpose
In the common Quorum Journal Manager (QJM) HA design, JournalNodes hold the shared NameNode edit-log stream.

## Write/read relationship
- Active NameNode writes namespace edits to a JournalNode quorum.
- Standby NameNode reads/tails edits and applies them to its namespace state.
- A typical small HA design uses three JournalNodes, requiring a majority for quorum operations.

## Failure examples
- One of three JNs down: two remain, so quorum usually remains.
- Two of three JNs down: quorum is lost; Active may be unable to commit namespace edits safely.
- JournalNodes are not DataNodes and do not store user HDFS blocks.

  ## **Table**
