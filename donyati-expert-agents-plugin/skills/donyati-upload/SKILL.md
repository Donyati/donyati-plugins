---
name: donyati-upload
description: Add a client document (RFP, proposal, transcript, notes) to a client or project knowledge base. Mines structured insights and indexes them for search.
---

# /donyati-upload — Add a Client Document

Drop a client's document into their knowledge base. The pipeline extracts structured insights and makes them searchable.

## Usage

```
/donyati-upload <client>   (then paste or attach the document)
```

## How it works

1. Resolves the client with `list_organizations` (and a project with `list_projects` if you name one).
2. Calls the `ingest_client_document` tool with the document's text.
3. Reports how many knowledge items were extracted. They are saved as **drafts pending admin
   review**, and become searchable via `/donyati-client-search` once an admin confirms them.

## Attaching a document to an assessment

`ingest_client_document` takes the same four lineage fields as `add_client_knowledge` and the
v1 ingest API — `engagementExternalId`, `assessmentRef`, `assessmentName` and `validAsOf`.
Supply them when the document is an assessment output, so its extracted insights attach to the
right engagement and assessment instead of landing loose at organization level.

## Examples

```
/donyati-upload Acme — project: RFP Response 2026   (attach the RFP PDF)
/donyati-upload Meridian   (paste the kickoff call transcript)
```

## Good to know

- **Text only:** pasted/attached text loses embedded images and complex table layout. For pixel-perfect source files, upload via the web app at https://expert-agents.donyati.com.
- Works well for RFPs, proposals, meeting transcripts, emails, and notes.
