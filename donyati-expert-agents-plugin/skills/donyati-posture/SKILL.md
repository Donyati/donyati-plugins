---
name: donyati-posture
description: Query Donyati's cloud security & compliance posture (Prowler/CSPM findings) — failing checks by severity, framework (CIS, SOC2), and provider. Admin-only.
---

# /donyati-posture — Cloud Compliance Posture (Admin)

Query the latest cloud security & compliance scan findings for Donyati's own tenants. **Admin keys/accounts only** — everyone else gets a polite denial from the server.

## Usage

```
/donyati-posture [query] [severity] [framework] [provider]
```

## How it works

Calls the `search_compliance_posture` tool with your filters and presents findings grouped by severity, with the check id, framework control, and affected resource.

## Examples

```
/donyati-posture severity: critical
/donyati-posture framework: CIS-2.0 provider: azure
/donyati-posture public storage buckets
```

## Note

This reads Donyati-internal scan results (Prowler agent). It is not a client-facing tool.
