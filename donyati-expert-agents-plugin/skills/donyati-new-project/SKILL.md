---
name: donyati-new-project
description: Create a new project/engagement under a client — the container for that pursuit's knowledge, documents, and deliverables.
---

# /donyati-new-project — Create a Project

Create an engagement under a client so knowledge and documents stay scoped to the right pursuit.

## Usage

```
/donyati-new-project <client> <project name>
```

## How it works

1. Resolves the client with `list_organizations` (creates it via `create_organization` if it doesn't exist).
2. Calls the `create_engagement` tool with that organizationId and the project name.
3. Reports the new `projectId`.

## Examples

```
/donyati-new-project Acme FCCS Implementation
/donyati-new-project Meridian RFP Response 2026
```

## Next step

Use the `projectId` with `/donyati-upload`, `/donyati-add-knowledge`, or `/donyati-client-search` to scope work to this engagement.
