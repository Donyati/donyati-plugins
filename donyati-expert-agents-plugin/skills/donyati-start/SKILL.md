---
name: donyati-start
description: Guided entry point for Donyati Expert Agents — tells you what you can do and routes you to the right workflow. Start here if you're new.
---

# /donyati-start — Start Here

The easiest way in. Describe what you're trying to do and the assistant routes you to the right tools.

## Usage

```
/donyati-start
/donyati-start <what you want to do>
```

## What it can route you to

1. **Ask a platform expert** (Oracle EPM, SAP, etc.) — `/donyati-ask`, `/donyati-compare`. Add an industry overlay with `— industry: <slug>` (run `/donyati-agents` to see available slugs)
2. **Review or summarize a document** — `/donyati-review`, `/donyati-summarize`, `/donyati-sow-review` (Havagi, the firm SOW reviewer)
3. **Look up what we know about a client** — `/donyati-client-search`, `/donyati-briefing`
4. **Generate deliverables** — `/donyati-deliverables` (RFP deck, exec summary, requirements, data source inventory, response repository)
5. **Set up a new client or project** — `/donyati-new-client`, `/donyati-new-project`
6. **Add knowledge or upload a client document** — `/donyati-add-knowledge`, `/donyati-upload`

## Examples

```
/donyati-start
/donyati-start RFP response for Acme
/donyati-start have the Oracle EPM expert critique this SOW
```
