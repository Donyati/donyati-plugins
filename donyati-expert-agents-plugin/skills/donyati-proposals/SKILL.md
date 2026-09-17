---
name: donyati-proposals
description: Answer an RFP or questionnaire question by question from Expert Agents — list proposals, read the question tree, draft grounded answers with sources, accept answers, export to Word.
---

# /donyati-proposals — RFP response workspace

Works on proposals created in Expert Agents at /proposals. Drafts are grounded in the client's own documents and knowledge, curated platform knowledge, and published case studies and consented references; every draft returns its sources, and nothing is written when no source supports an answer.

## Usage

```
/donyati-proposals                       # list proposals you can reach
/donyati-proposals <client>              # list for one client
/donyati-proposals open <proposalId>     # read the question tree
/donyati-proposals draft <proposalId> <questionId>
/donyati-proposals accept <proposalId> <questionId> "<answer text>"
/donyati-proposals export <proposalId>
```

## How it works

1. `list_proposals` (optionally after `list_organizations` for an `organizationId`).
2. `get_proposal` returns every section and question with its id, status and answer.
3. `draft_proposal_answer` drafts one question. Pass `overwrite: true` only when the user confirms replacing an accepted answer. `mode: "shorter"` halves the length.
4. `update_proposal_answer` writes the user's own text (marks it answered) or changes status; `answerStatus: "unanswered"` clears an answer. It also needs `overwrite: true` to replace an accepted answer.
5. `export_proposal` files a Word response under the client's Deliverables and returns the download path.

## Rules

- Never paste an answer from another client's proposal into this one. The tools only ground on this client and firm-wide knowledge by design.
- Show the sources with every draft. A draft with no sources does not exist.
- Uploading and re-extracting an RFP is done in the web app, not here.

## Note

This skill reaches the **production** MCP server. Tools added on `dev` are unavailable until promoted to `main`.
