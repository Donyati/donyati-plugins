---
name: donyati-review
description: Have an expert agent critically review a document before you send it — surfacing findings, gaps, risks, and recommendations. For proposals, SOWs, deliverables, and responses.
---

# /donyati-review — Expert Document Review

Get a critical review of a document with Donyati domain expertise. Unlike `/donyati-summarize` (neutral recap), this looks for problems and gives recommendations.

## Usage

```
/donyati-review <paste or attach the document>
```

## How it works

Calls the `review_document` tool, which returns:

1. A one-paragraph overall assessment
2. Key findings
3. Gaps or risks
4. Concrete recommendations

## Options

- **platform** — slug for domain context; auto-detected if omitted. Either a vendor platform
  (`oracle-epm`, `snowflake`, `sap`, …) or an **industry expert** (`insurance`,
  `manufacturing`, `financial-services`, `healthcare`, `energy-utilities`, `retail`,
  `consumer-products-goods`, `life-sciences`, `high-tech`, `transportation-logistics`,
  `wholesale-distribution`, `communication-media-entertainment`, `engineering-construction`,
  `higher-ed`, `public-sector`, `professional-services`) when the document is best judged
  against how that industry actually runs its close and reporting
- **focus** — narrow the review, e.g. `scope risk`, `technical accuracy`, `completeness`

## Examples

```
/donyati-review [paste SOW] — focus: scope risk
/donyati-review [paste architecture doc] — platform: snowflake
/donyati-review [paste close design doc] — platform: insurance
/donyati-review [paste RFP response] — focus: completeness
```

## Tip

Run this on a deliverable **before** it goes to the client. Use `/donyati-summarize` instead when you just need a neutral recap.
