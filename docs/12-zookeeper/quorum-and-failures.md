# Quorum and Failure Scenarios

A majority of voting servers must be available to form quorum. In a 3-node ensemble, 2 are a majority: one failure usually leaves service available; two failures lose quorum. Observers, if configured, do not count as voting members.

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
