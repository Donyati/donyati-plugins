---
name: donyati-add-knowledge
description: Record new facts into a client or project knowledge base — meeting notes, decisions, stakeholder details, requirements, risks. Items are confirmed immediately and become searchable.
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

Items are **confirmed immediately** and searchable right away (via `/donyati-client-search`).

## Examples

```
/donyati-add-knowledge Joyson CFO wants to go live before fiscal year end; current close takes 12 days
/donyati-add-knowledge Meridian — project: Discovery — they use Workday for HR and SAP for finance; main pain is manual reconciliation
```

## Good categories

`stakeholder`, `requirement`, `risk`, `decision`, `tech-stack`, `timeline`, `pain-point`, `commercial`

## Note

Facts you add are authoritative the moment they're saved — keep them accurate and concise.
