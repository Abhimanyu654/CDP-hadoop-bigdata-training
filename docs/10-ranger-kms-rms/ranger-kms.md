# Ranger KMS

Key management service for supported encryption integrations. HDFS encryption zones use encryption keys and data-encryption-key operations; key access policies and service availability matter. It does not replace HDFS block replication.

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
