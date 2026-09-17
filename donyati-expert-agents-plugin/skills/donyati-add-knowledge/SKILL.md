---
name: donyati-add-knowledge
description: Record new facts into a client or project knowledge base — meeting notes, decisions, stakeholder details, requirements, risks, and assessment results with their lineage.
---

# /donyati-add-knowledge — Add Client Knowledge

Capture what you just learned about a client (from a call, email, or meeting) into the shared knowledge base so the whole team benefits.

## Usage

```
/donyati-add-knowledge <client> <notes / facts to record>
```

## How it works

1. Resolve the client with `list_organizations` to get its `organizationId`.
2. (Optional) Resolve a project with `list_projects` to attach the facts to a `projectId`.
3. Break the notes into discrete facts — each with a short `category`, `subject`, and `detail`.
4. Call the `add_client_knowledge` tool to store them.

Items are saved as **drafts pending admin review**, matching the web upload flow. They become
searchable via `/donyati-client-search` once an admin confirms them.

## Recording an assessment result

Facts that came from an assessment can carry their lineage, so they attach to the right
engagement and supersede the score they replace instead of sitting alongside it. These are the
same four fields `POST /api/v1/knowledge/ingest` takes:

| Field | What it is |
|---|---|
| `engagementExternalId` | Another system's key for the project — DoEvolve writes `assessments-<id>`. Prefer `projectId` when you have it; resolved inside the client you named, never across clients. |
| `assessmentRef` | Stable slug of the assessment, e.g. `epm-maturity`. Rows sharing a ref supersede each other. |
| `assessmentName` | Display name, e.g. `EPM Maturity Assessment`. |
| `validAsOf` | ISO 8601 timestamp the facts were true as of, e.g. `2026-09-01T12:00:00Z`. |

Set `assessmentDimension` on a fact to say which dimension it scores. If the
`engagementExternalId` matches no project in that client, the reply says so and the facts land
at organization level rather than disappearing.

## Examples

```
/donyati-add-knowledge Joyson CFO wants to go live before fiscal year end; current close takes 12 days
/donyati-add-knowledge Meridian — project: Discovery — they use Workday for HR and SAP for finance; main pain is manual reconciliation
```

## Good categories

`stakeholder`, `requirement`, `risk`, `decision`, `tech-stack`, `timeline`, `pain-point`, `commercial`

## Note

Facts you add are authoritative the moment they're saved — keep them accurate and concise.
