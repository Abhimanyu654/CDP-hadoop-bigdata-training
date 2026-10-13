# Client Mode vs Cluster Mode

In client mode the driver runs in the submitting client process; in cluster mode the driver runs inside the YARN-managed application environment. Exact role/AM responsibilities depend on Spark version and deploy mode.

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
