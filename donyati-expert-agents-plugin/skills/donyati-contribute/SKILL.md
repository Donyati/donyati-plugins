---
name: donyati-contribute
description: Contribute to the Donyati knowledge base from Claude — submit an accelerator, an internal doc or a case study for review, resubmit one a reviewer sent back, or check where your submissions stand.
---

# /donyati-contribute — Submit an Accelerator, Internal Doc or Case Study

The same contribution flow as **Knowledge › Contribute** (`/contribute`) on the web, run from
Claude. Nothing you submit is published for everyone until a reviewer approves it.

## Usage

```
/donyati-contribute [accelerator | doc | case-study | status]
```

## How it works

1. **Pick the type** if the user has not. Ask one question:
   - **Accelerator**: a reusable script, template, workbook or toolkit from delivery work.
   - **Internal doc**: a runbook, guide or reference for Donyati staff.
   - **Case study**: an engagement write-up other consultants can learn from.
   - **Status**: call `list_my_submissions` and stop.
2. **Gather the fields** for that type (below). Ask only for what is missing, all in one message.
3. **Read the files the user named** and base64-encode them. One call carries at most 10 MB of
   file content in total. For anything larger, send the user to `/contribute` on the web.
4. **Call the tool**, then report what it returned in plain words, including any id it gives.

## Accelerator: `submit_accelerator`

- New: `name`, `category` (`script` | `template` | `workbook` | `toolkit` | `document`),
  `summary`, `files` (`[{ filename, contentBase64 }]`, a `.zip` is fine). Optional: `platform`
  (a `list_platforms` slug), `languages`, `tags`, and where it came from: `sourceEngagementId`
  (from `list_projects`, and it must be a project you can access), `sourcePocId` (`list_pocs`).
- New version of one you own: `acceleratorId`, `semver` (e.g. `1.1.0`), `files`, `changelog`.

**The upload scan decides what happens next. Do not work around it.**

- **Blocked** (a credential, connection string or private key): nothing is submitted. Show the
  findings, tell the user to remove them, and upload a new version with `acceleratorId`. You
  cannot override a block from here, and neither can the contributor on the web.
- **Warnings** (a hostname, an email, a possible client name): nothing is submitted yet. Show
  every finding and ask the user to confirm each one is intended or a false positive. **Only
  after they say yes**, call `submit_accelerator` again with `acceleratorId`, `versionNo` and
  `acknowledgeWarnings: true`. Never set `acknowledgeWarnings` on the first call just to skip
  this step.
- **Clean**: submitted for review straight away.

## Internal doc: `submit_internal_doc`

- New: `title`, `category`, `audience`, `file`, and optionally `description`.
  - `category`: Operations & Runbooks, Security, Access & RBAC, Product Guides, Onboarding,
    Architecture, Client-Facing Collateral, or Reference.
  - `audience`: `all` (all Donyati staff), `security` or `finance`. The user can only choose an
    audience they belong to; the tool says which ones they may choose if they pick another.
- Resubmit after a reviewer asked for changes: `docId` (from `list_my_submissions`), plus a
  revised `file`. Without a file, the returned version goes back to review unchanged.

## Case study: `submit_case_study`

Call it first **without** `domainId` to get the list of domains, and have the user pick one.
Then send `title`, `domainId` and **both versions**:

- `internalContent`: the full account. It may name the client, and it stays confidential.
- `publishedContent`: the sanitized version a reviewer publishes. It must not name the client
  or anyone who works there. Offer to draft it from the internal version, and have the user
  check it before you submit.

To submit a draft already written on the web, pass `caseStudyId` instead.

## What gets refused

- **A `dea_` API key.** Every contribution is owned by a person, so the tools need the
  connector's Microsoft 365 sign-in.
- **A missing permission.** Each type has its own Contribute permission. The refusal names it;
  an administrator grants it in Roles & Permissions.
- **Someone else's item.** You can only add versions to, resubmit or submit your own
  contributions.

Report a refusal as the rule it names. It is not an error to retry.
