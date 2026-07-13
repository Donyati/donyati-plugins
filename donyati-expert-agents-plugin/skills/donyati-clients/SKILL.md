---
name: donyati-clients
description: List Donyati client organizations — useful for finding the right client before searching, adding knowledge, or pulling a briefing.
---

# /donyati-clients — List Clients

List the client organizations Donyati tracks.

## Usage

```
/donyati-clients [name filter]
```

## How it works

Calls the `list_organizations` tool. Pass an optional substring to filter by name or domain.

## Examples

```
/donyati-clients
/donyati-clients joy
/donyati-clients meridian
```

## When to use

- Before `/donyati-client-search`, `/donyati-add-knowledge`, or `/donyati-briefing` when you're unsure of the exact client name
- To grab a client's `organizationId` for other commands

Pair with `/donyati-projects <client>` to drill into a specific engagement.
