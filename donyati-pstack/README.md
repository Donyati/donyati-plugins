# donyati-pstack

Engineering and writing discipline skills for Donyati IP products. Fourteen skills adapted from
the open-source [pstack](https://github.com/cursor/plugins/tree/main/pstack) set for Claude Code
and Azure DevOps.

**Install:** `/plugin install donyati-pstack`

**Do not install all fourteen everywhere.** Run the triage in
[Choosing and Applying pstack Skills on a Donyati IP Product](https://dev.azure.com/Donyati-Devops)
first. Most products need four to eight of these. A skill with no routing rule in your product's
`CLAUDE.md` will not fire.

---

## What's here

### Always applicable, any stack

| Skill | Use it when |
|---|---|
| `technical-writing` | Writing or reviewing docs, RFCs, READMEs, PR descriptions, commit messages. Picks a Diátaxis mode first, then Google developer style. |
| `unslop` | Cleaning AI tells out of **repo-internal engineering text**. See routing below. |

**Writing routing — this matters.** `unslop` is scoped to repo-internal engineering text.
Client-facing prose (email, Teams, memos, proposals, decks, deliverables) routes to
`avoid-ai-writing` instead. **Never run both on one artifact.** `unslop` rule 28 pushes toward
one idea per sentence; `avoid-ai-writing` identifies uniform sentence rhythm as the strongest
machine-written signal. They fight.

### TypeScript products only

| Skill | Use it when |
|---|---|
| `typescript-best-practices` | Auto-fires on any `.ts` / `.tsx`. Discriminated unions over optional-field bags, branded primitives, `satisfies` over `as`, exhaustiveness via `never`. |
| `principle-type-system-discipline` | The reasoning behind it. Make illegal states unrepresentable. |

Both are noise in a Python repo. Skip them for Integration Maestro, Close Assist, and DonyatiDocs.

### Where CI is not a real gate

| Skill | Use it when |
|---|---|
| `create-verification-skill` | Generates a project-local `.claude/skills/verify-<app>/` that knows how to launch, health-check, drive, and capture evidence from the real app. |
| `principle-prove-it-works` | Verify against the real artifact. "It compiles" is not proof, and neither is a green pipeline that runs no tests. |

Check first:

```bash
grep -iE 'vitest|npm test|pytest|npx tsc|playwright' azure-pipelines*.yml
```

No output means your pipeline builds and deploys without gating on tests. As of 2026-08-19 that
is true of **DDO** and **DonyatiDocs**. The other four IP repos gate on tests, and the workspace
rule against verification scaffolding stands there unchanged.

### Review and unattended work

| Skill | Use it when |
|---|---|
| `blast-radius` | A change looks small and you don't trust it. Find the one fact its safety depends on, then prove that fact by running code. |
| `show-me-your-work` | Long or unattended runs someone reviews after you step away. Append-only decision log. |

### Reasoning principles

Cited by the agent when a decision engages them, not invoked directly. About 190 lines total.

`principle-minimize-reader-load` · `principle-model-the-domain` · `principle-boundary-discipline` ·
`principle-laziness-protocol` · `principle-build-the-lever` · `principle-sequence-verifiable-units`

Plus `principle-type-system-discipline` and `principle-prove-it-works`, listed above.

---

## What was excluded, and why

Thirty of the 44 upstream skills are not here. The reasons are structural, not stylistic.

**Graphite and GitHub machinery.** `shipping`, `babysit`, `autopilot-stack`, `autopilot-full`,
`orchestrate`, and the `watch-pr` tool are built on `gt` and the GitHub API. Donyati runs Azure
DevOps, and `file-pr` and `babysit-pr` already cover this ground. Porting them would produce a
worse duplicate of tools we maintain.

**Cursor host features.** `setup-pstack` writes `~/.cursor/rules/pstack-models.mdc`, a file Claude
Code never reads. `swarm` hardcodes `environment: "cloud"`, which does not exist here.
`no-comments` depends on a `Comment Sicko` subagent persona that is not defined anywhere in the
upstream bundle.

**Model-panel skills, deferred not rejected.** `interrogate`, `arena`, `architect`, `how`, and
`why` default to `gpt-5.6-sol-max` and `grok-4.6-fast-xhigh`. Those do not resolve in Claude Code,
so the four-model panel silently collapses onto one Claude model — full fan-out cost, no
cross-family signal. `interrogate` is the highest-value skill in the upstream set and is planned
for v1.1 once its reviewer table is rewired onto `dispatch codex|gemini|ollama`.

**Already covered here.** `recall` (auto-memory plus `WORK_LOG.md` persist better), `reflect`
(`claude-md-management:revise-claude-md`), `tdd` (`superpowers:test-driven-development`), `swarm`
(`superpowers:dispatching-parallel-agents`), `automate-me`, `figure-it-out`.

**`poteto-mode` itself** is not vendored. It routes into 22 playbooks, six of which are Graphite
or GitHub based, and depends on a separate `cursor-team-kit` plugin for `deslop`, `control-cli`,
and `control-ui`.

---

## Modifications from upstream

Four files differ. See [`NOTICE`](NOTICE) for exact detail and MIT attribution.

- `unslop` — added `disable-model-invocation: true` plus a routing note. Upstream fires on nearly
  every turn.
- `blast-radius` — dropped references to unvendored skills, repointed at `az repos pr` and
  `dispatch`.
- `create-verification-skill` — output path moved to `.claude/skills/`.
- `show-me-your-work` — transcript path moved to `~/.claude/projects/`.

## Publishing

The marketplace serves plugins from the GitHub mirror of this repo, while Azure DevOps is
canonical for PRs. Publishing is a separate release step:

```bash
ORG=Donyati tools/sync-plugin-public.sh
```

**A stale mirror serves stale skills.** If a product owner reports a skill behaving like an older
version, check the mirror before debugging the skill.
