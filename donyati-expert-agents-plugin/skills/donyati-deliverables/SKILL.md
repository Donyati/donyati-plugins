---
name: donyati-deliverables
description: List and generate Donyati deliverables for a client — RFP/proposal decks, executive summaries, client briefings — returned as links (web deck or download).
---

# /donyati-deliverables — Generate a Deliverable

See what the connector can produce for a client and generate it.

## Usage

```
/donyati-deliverables [client] [slug]
```

## How it works

1. (If a client is named) resolve it with `list_organizations`, and `list_projects` for the project.
2. Call `list_deliverables` to see the options.
3. Call `generate_deliverable` with the chosen slug, organizationId, and (when needed) projectId.
4. You get back a link — a web deck (`/presentations/{id}`) or a download URL.

## What you can generate today

| Slug | Output |
|---|---|
| `rfp` | RFP / proposal deck (PowerPoint download) — requires a project |
| `executive-summary` | Executive summary web deck |
| `client-briefing` | Client briefing web deck |
| `assessment-summary` | Assessment synthesis web deck |
| `custom-summary` | Free-form deck from a prompt |
| `requirements` | Requirements document (Word) — requires a project |
| `data-source-inventory` | Data source inventory (Word) — requires a project |
| `response-repository` | Org-wide Q&A index from prior proposals (Word) |

Web decks open in the interactive presentation viewer (`/presentations/{id}`); Word documents download via a sign-in-protected link.

## Grounding a summary deck in an industry

The four summary slugs (`executive-summary`, `client-briefing`, `assessment-summary`,
`custom-summary`) take an optional `industry`, which pulls in that industry expert's curated
knowledge — terminology, standard metrics, the structure a reader in that industry expects:

`insurance` · `manufacturing` · `financial-services` · `healthcare` · `energy-utilities` ·
`retail` · `consumer-products-goods` · `life-sciences` · `high-tech` ·
`transportation-logistics` · `wholesale-distribution` · `communication-media-entertainment` ·
`engineering-construction` · `higher-ed` · `public-sector` · `professional-services`

Omit it to use the client's own industry when the platform has one recorded; pass `none` to
turn it off. The industry corpus is background only — it never overrides the client's source
content, and nothing in it is stated as a fact about the client.

## Examples

```
/donyati-deliverables Acme
/donyati-deliverables Meridian rfp
/donyati-deliverables Joyson client-briefing   (grounded in manufacturing)
```

For the full RFP flow use `/donyati-rfp-response`.
