# Ranger, Ranger KMS, and Ranger RMS: Architecture, Components, Dependencies, and Operations

## 1. Purpose
Ranger centralizes authorization policies and auditing via service-specific plugins. Ranger KMS manages encryption keys for supported integrations. RMS is a Cloudera/Ranger-related service role whose exact expansion and behavior must be verified for the deployed release.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **Ranger Admin** | Central policy administration interface/service. |
| **service plugins** | Retrieve/cache policies and enforce authorization inside supported services. |
| **Usersync** | Synchronizes users/groups from configured identity sources into Ranger, depending on deployment. |
| **Tagsync** | Synchronizes tag/classification information for tag-based policies when configured. |
| **audit destination** | Receives authorization audit events according to the configured audit setup. |
| **policy store** | Persists Ranger policy/configuration state. |
| **Ranger KMS API/key provider** | Provides key-management operations to supported clients/services under configured authorization. |
| **key store/backing configuration** | Persists key material/metadata according to the selected KMS configuration. |
| **Ranger RMS (release-specific role).** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

## 3. Conceptual architecture
```text
Client / workload
       |
       v
Service entry point / coordinator
       |
       +---- metadata, scheduling, or policy services
       |
       v
Workers / executors / storage nodes
       |
       v
Data, results, logs, or audit events
```

## 4. End-to-end flow
1. Admin defines policy
2. plugin retrieves/caches policies
3. plugin enforces access in service
4. audit event is emitted. For encryption, client/service requests key operations from KMS according to policy and key configuration.

## 5. Dependencies and integrations
Ranger plugins integrate with target services such as HDFS/Hive/Impala/Kafka depending on support. KMS integrates with HDFS encryption zones and key provider configuration. Kerberos is a common security prerequisite in CDP.

## 6. Failure scenarios
- Policy not refreshed
- plugin fails to contact Admin
- audit backlog
- KMS unavailable for key operations
- key permission denied
- key rotation planning errors.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does Ranger, Ranger KMS, and Ranger RMS solve?
2. Explain the architecture and the responsibility of each major role.
3. What services does it depend on, and which integrations are optional?
4. What happens if a key role stops?
5. How would you troubleshoot a slow or failed request?

## 9. Interview summary
Explain the service's purpose, its main roles, request flow, core dependencies, and one realistic failure scenario in 60–120 seconds.

## 10. Version and references
- Target scope: CDP Private Cloud Base / Cloudera Runtime 7.x, unless a lesson says otherwise.
- Verify role names, supported versions, and dependency status in Cloudera Manager and official documentation.
- Do not treat this overview as a production runbook.
