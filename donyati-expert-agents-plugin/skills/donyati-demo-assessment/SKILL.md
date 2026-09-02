---
name: donyati-demo-assessment
description: Score a software vendor's demo against a client's requirement areas — evidence tier and a verified transcript citation behind every score, with the deferred items called out rather than counted as zero. For vendor selections, platform bake-offs, and demo debriefs.
---

# /donyati-demo-assessment — Vendor Demo Requirements Fit

Turn a demo transcript into a defensible scorecard. Calls the `assess_vendor_demo` tool.

## Usage

```
/donyati-demo-assessment <client> <vendor>
```

## Why this beats asking a chatbot the same question

1. **Evidence tiers.** Every capability is scored as `demonstrated`, `discussed`, `claimed`,
   `not-addressed`, or `deferred`. Only demonstrated capability can score the full 5 — anything
   the vendor merely described or asserted is capped at 3. "We can build that" never outranks
   working software, and the cap is applied in code after the model answers, not requested in
   a prompt.
2. **A verified citation per score.** Each scored row carries the transcript excerpt it rests
   on, and that excerpt is checked against the source text before anything is saved. A row
   whose quote cannot be found is recorded as a claim and flagged, rather than quietly kept.
3. **A deferral is pending; an absence is a gap.** `deferred` means the vendor said in so many
   words that they would cover it separately — that is excluded from the score and listed under
   Pending Validation, because scoring it zero would misrepresent a vendor who never claimed to
   have shown it. `not-addressed` means the capability was required and the demo never evidenced
   it, and that **scores**, capped at 1. A deferral has to be quotable like any other citation:
   an unverifiable one is recorded as `not-addressed` and starts costing the vendor points.
4. **Per-area coverage.** Alongside each area's score you get how many of its capabilities
   carried real evidence — "1 of 6 evidenced". Below half, the verdict is marked
   *(partial coverage)*. A mean says how good the evidence was and nothing about how much of
   the area it covered, which is how a single strong capability can otherwise present as an
   area the vendor meets.
5. **Weighting you control.** Each requirement area is `must-have` (×3), `important` (×2) or
   `nice-to-have` (×1). Both the weighted and the plain average come back, so you can see what
   the weighting did.

## How it works

1. Resolve the client with `list_organizations` and the project with `list_projects`.
2. Call `assess_vendor_demo` with just `organizationId` and `projectId` — it lists the
   client's documents with their ids so you can pick the demo transcript. Documents whose
   text has not been extracted yet are marked; those cannot be scored.
3. Call it again with `transcriptContentId`, `vendorLabel`, and `areas`:

   ```json
   {
     "organizationId": 36,
     "projectId": 71,
     "transcriptContentId": 4120,
     "notesContentId": 4121,
     "vendorLabel": "Anaplan",
     "incumbentLabel": "Oracle Planning",
     "areas": [
       { "name": "Workforce planning", "weight": "must-have" },
       { "name": "Reporting", "weight": "important", "detail": "Self-service report building" },
       { "name": "Integration", "weight": "must-have" }
     ]
   }
   ```

4. You get the scorecard, the per-area evidence with citations, the Pending Validation list,
   and follow-up questions to send the vendor — plus an assessment id and a link.
5. Pass that id back as `assessmentId` any time to re-read the scorecard.

## Arguments

| Argument | Notes |
|---|---|
| `organizationId` | Required. From `list_organizations` |
| `projectId` | Required to generate. From `list_projects` |
| `transcriptContentId` | The demo transcript. Omit to list the client's documents |
| `notesContentId` | Optional consultant notes. Citations may quote these too |
| `vendorLabel` | The vendor being evaluated, e.g. `Anaplan` |
| `incumbentLabel` | Optional — what to write the positioning section against |
| `areas` | The requirement areas, in report order. `weight` defaults to `important`. See below on `detail` |
| `list` | `true` lists the scorecards already generated for this client/project |
| `assessmentId` | Re-read an existing scorecard, or correct it when `operation` is set |
| `operation` | `edit-row`, `reweight` or `set-status` — see Correcting a scorecard |
| `additionalInstructions` | Extra scoring guidance |

## `detail` is what makes the scorecard specific

Each area takes an optional `detail` string, and it does more work than it looks like it does.
Given a bare area name the scorer picks three to six capabilities to check on its own. Given the
capabilities the client actually enumerated, it produces **one scored row per capability**, in the
client's wording:

```json
{
  "name": "Cash forecasting",
  "weight": "must-have",
  "detail": "Direct cash forecasting; 13-week cash view; bank feed integration; scenario modelling"
}
```

If you have the client's requirements document to hand, read the capabilities out of it and pass
them here. The web form can do this for you — Deliverables → Vendor Demo Assessment →
**Extract areas from a requirements document…** reads a document on the project and prefills the
editable area list. That extraction is web-only; from the connector, pass the `detail` yourself.

## Getting the transcript in

The transcript has to be a document on the client's project first. Either upload it in the
Document Workspace, or — if the demo was a recorded call — import it with
`/donyati-upload` or the `ingest_readai_meeting` tool, then run this.

## Correcting a scorecard

Scores are stored as rows, so a scorecard is correctable rather than a one-shot answer — and
most of the real work happens here, after the first answer lands. Corrections run from this
tool, without leaving the conversation: pass the `assessmentId` plus an `operation`.

| `operation` | With | Does |
|---|---|---|
| `edit-row` | `rowId`, and any of `score`, `evidenceTier`, `assessment`, `citation`, `followUp` | Correct one capability row and recompute the scorecard around it |
| `reweight` | `areaId`, `weight` | Change one area's weight; the weighted total moves, the plain mean does not |
| `set-status` | `status: "final"` or `"draft"` | Finalize the scorecard, or reopen a final one for correction |

Row and area ids come off the scorecard the tool returns. To find an assessment you generated
earlier, call the tool with `list: true` and the client/project — it lists every scorecard with
its id, vendor, status and weighted score.

```json
{ "organizationId": 36, "projectId": 71, "assessmentId": 12,
  "operation": "edit-row", "rowId": 340, "score": 5, "evidenceTier": "demonstrated",
  "citation": "you can see the 13-week view rebuilding as I change the assumption" }
```

**Follow-up questions are yours to reword.** The scorer writes a question for each deferred,
never-evidenced or low-scoring capability, and those questions go out to the vendor and into
the Word export. Pass `followUp` on `edit-row` to replace the wording. Passing `null` restores
the default question — it does **not** remove the row from the follow-up list, which is what
you want, since the row still needs an answer.

**A correction is held to the same evidence standard as the original.** Moving a row to
`demonstrated`, `discussed` or `deferred` re-checks its citation against the transcript and
notes. If the excerpt is not there, the row is stored one tier down — `claimed` for the first
two, `not-addressed` for a deferral — and the reply opens with a ⚠️ notice saying so. The fix
is to paste the real wording from the source, not to assert the tier again.

**Deleting** an assessment is web-only, by design: it cascades to every row, and every row
quotes the client's transcript verbatim. Open the link the tool returns, under Document
Workspace → Deliverables.

## Availability

The connector points at **production** (`expert-agents.donyati.com`), where
`assess_vendor_demo` is available to plugin users.

## Examples

```
/donyati-demo-assessment Trean Anaplan
/donyati-demo-assessment Acme — score the OneStream demo on close, consolidation and reporting
```
