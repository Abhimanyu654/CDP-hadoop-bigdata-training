# Driver and Executors

Driver builds the application plan and schedules tasks. Executors run tasks and may cache partitions or hold shuffle data. Executor failure triggers task retry/reallocation depending on configuration and available resources.

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
