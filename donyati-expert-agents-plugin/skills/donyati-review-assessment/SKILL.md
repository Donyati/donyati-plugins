---
name: donyati-review-assessment
description: Have an expert critique an assessment TEMPLATE — its dimensions, questions, maturity levels and recommendations — before it is published. For assessment content authors, not for scoring a client's answers.
---

# /donyati-review-assessment — Expert Review of an Assessment Template

Gets domain-expert feedback on an assessment's structure and content — is a dimension missing,
is a question ambiguous, does a maturity level actually distinguish maturity. This is content
authoring, not client work: it reviews the **template**, before anyone answers it.

Not to be confused with `/donyati-assessment` (drafts a client-specific assessment proposal from
engagement discovery material) or `/donyati-demo-assessment` (scores a vendor demo transcript).

## Usage

```
/donyati-review-assessment [paste the assessment snapshot JSON] — expert: <slug>
```

## How it works

Calls the `review_assessment` tool with the assessment's snapshot (dimensions, questions,
maturity levels, recommendations, as JSON) and an `expertId`. Returns a one-line summary plus a
numbered list of suggestions, each tagged with impact.

## Options

- **expertId** (required) — a vendor platform slug (`oracle-epm`, `sap`, `snowflake`, …) or an
  industry expert (`insurance`, `manufacturing`, `financial-services`, `healthcare`,
  `energy-utilities`, `retail`, `consumer-products-goods`, `life-sciences`, `high-tech`,
  `transportation-logistics`, `wholesale-distribution`, `communication-media-entertainment`,
  `engineering-construction`, `higher-ed`, `public-sector`, `professional-services`). Use
  `list_agents` or `list_platforms` to resolve one.
- **expertName** — optional display name for the expert in the reply.

## Getting the snapshot

The snapshot is the assessment's dimensions, questions, maturity levels and recommendations as
one JSON object — export it from **Admin → Assessments** on the assessment you're revising, or
build it from the assessment's admin API response.

## Examples

```
/donyati-review-assessment [paste snapshot] — expert: oracle-epm
/donyati-review-assessment [paste snapshot] — expert: insurance
```
