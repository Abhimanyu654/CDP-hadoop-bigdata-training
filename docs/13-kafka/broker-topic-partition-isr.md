# Broker, Topic, Partition, Replicas, and ISR

A topic is split into partitions; each partition has an ordered log and a leader. Replicas copy partition data; ISR is the in-sync replica set. Producer acknowledgement settings and replication configuration determine durability behavior.

## Administrator checklist
- Confirm the role's status and host in Cloudera Manager.
- Check logs, health tests, recent configuration changes, and upstream/downstream dependencies.
- Verify behavior against the exact CDP/Runtime version.
- Capture a sanitized lab example and one failure scenario.

## Interview questions
1. What is this component responsible for?
2. Which components communicate with it?
3. What happens if it becomes unavailable?
4. What metrics/logs would you check first?
