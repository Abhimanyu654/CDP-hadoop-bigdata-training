# Atlas: Architecture, Components, Dependencies, and Operations

## 1. Purpose
Atlas provides metadata governance, classification, and lineage for supported data assets and processing flows.

## 2. Main components and roles

| Component / role | Responsibility |
|---|---|
| **Atlas server/API** | Accepts metadata operations and serves search/governance requests. |
| **entity/type system** | Defines metadata entity types and relationships. |
| **graph/metadata store** | Persists Atlas metadata and relationships in the configured backing store. |
| **search index (often Solr in common deployments)** | Indexes metadata for search where configured. |
| **hooks/connectors** | Capture supported service events and send metadata/lineage information to Atlas. |
| **notification/lineage processing** | Processes metadata events and builds supported lineage relationships. |
| **UI.** | Core responsibility depends on the configured role and CDP release; verify the role description in Cloudera Manager and the official versioned guide. |

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
1. Service hook/connector captures supported event
2. Atlas creates/updates entities and relationships
3. classifications/tags can be applied
4. users search metadata and inspect lineage.

## 5. Dependencies and integrations
Integrations vary by service and version; common deployments use supporting metadata/search stores and hooks for Hive, HBase, Kafka, or other assets. Ranger tag-based policies may integrate where configured.

## 6. Failure scenarios
- Hook not installed
- notification backlog
- search index unavailable
- entity missing
- lineage incomplete because an operation is unsupported.

## 7. Operations checklist
- Check service health and role status in Cloudera Manager.
- Review recent alerts, configuration changes, and affected hosts.
- Inspect the service logs and metrics for the exact deployed release.
- Check dependencies before restarting the service.
- Confirm the impact scope and use the approved runbook before production changes.
- After recovery, verify a representative client request and review audit/monitoring signals.

## 8. Interview questions
1. What problem does Atlas solve?
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
