---
name: donyati-case-study
description: Write up a Donyati engagement as a case study — drafts the internal, client-named account and the sanitized version the expert agents can quote, ready to paste into the Expert Agents platform.
---

# /donyati-case-study — Write Up an Engagement

Turn what you remember about an engagement into the two documents the Expert Agents platform
wants. You bring the raw material — a transcript, a Teams thread, a repo, your own recollection.
This drafts both faces and checks them before you paste.

## Usage

```
/donyati-case-study <what happened, or a path to notes / a transcript>
```

## What you are producing, and why it is two documents

A case study on the platform is a **pair** of linked articles:

| Face | Who reads it | Rule |
|---|---|---|
| **Internal account** | Donyati staff with the Case Studies capability | Names the client. Confidential, excluded from retrieval by construction. Never reaches an expert agent. |
| **Sanitized version** | Everyone, via any expert agent | This is what gets retrieved and quoted, sometimes into work that reaches a client. It must teach the same lesson without identifying who it happened to. |

**Write the internal account first, in full, then sanitize it into the second.** That order is
not a style preference: a vague version written from scratch reads like nothing happened, while
a sanitized version derived from a complete account keeps the detail that makes it worth
reading. Do not draft them in parallel.

## Step 1 — interview before drafting

Do not write from a one-line prompt. Ask for what is missing, a few questions at a time, and
stop asking once you can fill every section below. The questions that matter most, because
they are the ones people leave out:

- **What actually broke, in specifics.** Error codes, log lines, row counts, the exact
  behaviour. "The load failed" is not a case study.
- **What you tried that did not work**, and the theory that turned out to be wrong. A wrong
  theory that was reasonable at the time is one of the most useful things you can record.
- **How you knew it was fixed.** What was re-run, what was compared, what the client confirmed.
- **What it cost and what it saved.** Hours spent, rows recovered, what the manual equivalent
  would have been.
- **Where it fell short.** What is still open, what you would do differently, what you found and
  did not fix. Every seeded case study has this section. A write-up with no shortcomings reads
  as marketing and gets skipped.
- **Whether the client has been asked for reference permission.** Usually the answer is no —
  see Step 5.

## Step 2 — draft the internal account

Use `##` headings. The seeded case studies use these; take the ones that fit and drop the rest:

`## Situation` · `## Symptom` · `## What made it hard` · `## Root cause` · `## What we did` ·
`## How it was verified` · `## Outcome` · `## Where it fell short` ·
`## Platforms and products touched` · `## Reusable pattern`

`## Reusable pattern` is the section that earns the article its place in the corpus — it is the
part another consultant applies to a different client. Write it as instructions, not as a
summary of what you did.

Hold nothing back on this face. If you are hedging here, you are writing in the wrong pane.

## Step 3 — derive the sanitized version

Same problem, same fix, same lesson, told so another consultant could repeat it. Rewrite the
internal account paragraph by paragraph rather than deleting sentences from it.

**Must not contain:**

- The client name, their subsidiaries, or their internal project names
- Names of people at the client — a sentence naming an individual passes an org-name scan
- Hostnames, file paths, database names, process IDs, ticket numbers
- Exact figures precise enough to fingerprint one engagement
- Any detail combination that only fits one client, even with the name removed

**Should contain:** descriptive stand-ins ("a manufacturing client", "one legal entity", "a
China-based entity"), rounded figures, and the reusable pattern in full.

The four moves, from the first case study in the corpus:

| Move | Internal | Sanitized |
|---|---|---|
| Name the client → describe them | "Summit Polymers, a long-standing Oracle EPM client, reported that one legal entity had stopped loading in their monthly CMS to FCC import. Entity 03 (Summit Plastics Nanjing)." | "A long-standing manufacturing client reported that one legal entity had stopped loading in their monthly consolidation import." |
| Exact figures → approximate | "Process 16078 loaded 84,929 rows and rejected 1,251." | "The import loaded roughly 85,000 rows and rejected about 1,250, yet reported success." |
| Keep the wrong theory, anonymized | "The client's working theory was that Chinese characters were breaking the pipe-delimited file. That was reasonable and it was wrong: Entity 43 is also a China entity and loads cleanly." | "The working theory on site was that non-Latin characters were breaking the delimited file. That was reasonable and it was wrong: another entity in the same region loaded cleanly." |
| Vendor internals stay, client internals go | "Byte 17 of that block is 0x0A. When the EPM Integration Agent writes the extract file, that byte ends the line partway through the record, so FDMEE reads one 16-column record as a 5-field fragment and a 12-field fragment." | "One byte of that block is a line feed. When the integration agent writes the extract file, that byte ends the line partway through the record, so the loader reads one 16-column record as two malformed fragments." |

Note what the last row does **not** do: it keeps the mechanism. Sanitizing is not vagueness.

**Oracle EPM naming, in both faces.** The published face goes into the corpus that expert agents
quote, so use Oracle's current names: **FCC** not FCCS · **Planning** not EPBCS/PBCS ·
**Account Reconciliation (ARC)** not ARCS · **EDM** not EDMCS · **NR** not Narrative Reporting ·
**EPCM** not PCMCS. Verbatim client quotes are the exception.

## Step 4 — check it before handing it over

Re-read the sanitized face against these and report what you find rather than quietly fixing it:

- Does any sentence name a company, a person, or a place specific enough to identify one?
- Would someone who worked the engagement recognise it? That is fine. Would someone who did
  **not** be able to work out who it was? That is not.
- Does it still teach the lesson, or did sanitizing hollow it out? If the second, go back to the
  internal account and rewrite the paragraph rather than deleting it.
- Are the two faces telling the same story, with the same outcome and the same shortcomings?

**Say plainly that this is a first pass.** The real gate runs at publish time on the platform:
it scans the sanitized text against every client name in the live database and against the
reference contacts you record, and it refuses to publish on a hit. You cannot run that scan from
here, and you should not imply you have.

## Step 5 — the reference record

The sanitized version never names the client, so it is safe to publish either way. The reference
record answers a different question: **may we name them somewhere else** — in a partnership deck,
on the website, or by introducing a prospect. Ask which of these is true:

- **Not requested** — nobody has asked. This is the honest default and the usual answer. It means
  the client may not be named anywhere external.
- **Requested** — asked, waiting. Record what was asked for.
- **Declined** — they said no. Worth recording so nobody asks again by accident.
- **Approved** — they said yes. **The author cannot set this**; recording a client's consent needs
  a reviewer.

The four permissions — name the client, use their logo, take reference calls, quote them — are
separate on purpose. Clients routinely allow an anonymized story but not their logo, or a
reference call but no written quote.

Also collect **contacts**: who at the client would be asked, their title, and who is primary.
This is not optional detail. Whoever chases the reference in six months needs a name, and the
publish check uses these names to catch a person surviving into the sanitized text.

## Step 6 — hand it over

There is no MCP tool for case studies, so this skill does not submit. Output the two faces as
separate markdown blocks, plus the reference answers from Step 5, and tell the author to paste
them into:

**https://expert-agents.donyati.com/case-studies/new** — also reachable from **Knowledge →
Case Studies** in the nav.

Then tell them what happens next, because it is not obvious and it catches people out:

1. Saving creates both faces as **drafts**. Nothing is served to an expert agent yet.
2. **Submitting freezes editing.** That is deliberate — otherwise a reviewer publishes text the
   author changed after they read it. Get it right before you submit.
3. A **reviewer** publishes, not the author, and **the author cannot publish their own case
   study** even if they hold the approve capability. The published words are the reviewer's call.
4. Once published, the author may **withdraw** it but not edit it, and delete is refused.

## Related

- `/donyati-add-knowledge` — client facts and meeting notes, not engagement write-ups
- `/donyati-knowledge` — search what is already in the corpus before writing; if the pattern is
  already there, add to it rather than filing a near-duplicate
