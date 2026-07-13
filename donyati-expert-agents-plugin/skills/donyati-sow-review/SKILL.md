---
name: donyati-sow-review
description: Havagi — Donyati's CIO-style SOW reviewer. Audits a Statement of Work or RFP response against the firm's approved-SOW corpus, flagging scope creep, unrealistic timelines, missing deliverables, and commercial risk with severity + confidence per finding.
---

# /donyati-sow-review — Havagi SOW Review

Run Donyati's firm SOW reviewer (Havagi) on a document before it goes out. Unlike `/donyati-review` (general expert review), Havagi is grounded in Donyati's own approved SOWs and reviews like a CIO protecting the firm's commercial interests — and it learns from every SOW it approves.

## Usage

```
/donyati-sow-review   (paste or attach the SOW / RFP response)
```

## How it works

Calls the `review_sow` tool, which returns:

1. An overall verdict + summary
2. Section-by-section findings with severity (critical / warning / suggestion)
3. A confidence rating per finding (based on how many historical SOWs informed it)

## Options

- **content_type** — `sow` (default) or `rfp`
- **verbosity** — `detailed` (default) or `concise` (critical + warning findings only)

## Examples

```
/donyati-sow-review [paste SOW]
/donyati-sow-review [paste RFP response] — content_type: rfp
/donyati-sow-review [paste SOW] — verbosity: concise
```

## Tip

Run this on every SOW **before** client review. For general documents (proposals, architecture docs), use `/donyati-review` instead.
