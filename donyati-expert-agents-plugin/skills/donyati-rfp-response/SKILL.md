---
name: donyati-rfp-response
description: Generate an RFP / proposal response deck for a client project — assembled from the project's confirmed knowledge (modules, discovery, pain points) and returned as a PowerPoint download.
---

# /donyati-rfp-response — RFP Response Deck

Turn a project's captured knowledge into a branded RFP / proposal response deck.

## Usage

```
/donyati-rfp-response <client> [project]
```

## How it works

1. Resolves the client with `list_organizations` and the project with `list_projects`.
2. Calls `generate_deliverable` with slug `rfp` — which assembles the RFP request from the project's confirmed knowledge (suggested modules, discovery highlights, current systems, pain points) and renders the deck.
3. Returns a PowerPoint download link (expires in ~60 minutes).

## Before you run it

The deck is built from **confirmed** client knowledge. If the assembler reports missing inputs (e.g. no modules), add them first:
- `/donyati-upload <client>` — drop in the client's RFP or discovery docs
- `/donyati-add-knowledge <client>` — record modules, systems, pain points

## Examples

```
/donyati-rfp-response Acme — project: FCCS Implementation
/donyati-rfp-response Meridian — platform: Oracle EPM
```

## Note

Defaults to the Oracle EPM platform / EPM_Practice_USA. Pass a different platform if needed.
