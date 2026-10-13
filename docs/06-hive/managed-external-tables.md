# Managed and External Tables

Table type affects ownership and drop semantics, which vary with table format, Hive version, and configuration. Validate behavior before deleting production tables; never assume all external table drops preserve data in every configuration.

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
