# Production Incident Runbook Template

> Never use a generic template as permission to restart production services. Follow approved change control and service-specific runbooks.

## Incident
- Start time:
- Affected service/roles:
- User impact:
- Recent changes:
- Severity/owner:

## Triage
1. Confirm scope and whether the issue is ongoing.
2. Check Cloudera Manager health and alerts.
3. Check dependency health (HDFS, YARN, Kerberos, KMS, ZooKeeper, metastore, etc. as relevant).
4. Review recent logs and metrics.
5. Identify the safest mitigation and required approvals.
6. Validate recovery with a representative request.
7. Record timeline, root cause, and prevention tasks.

## Commands
Add only commands tested for the target release and explain whether they are read-only or mutating.

## Post-incident
- Root cause:
- Contributing factors:
- Recovery:
- Preventive actions:
- Documentation changes:
