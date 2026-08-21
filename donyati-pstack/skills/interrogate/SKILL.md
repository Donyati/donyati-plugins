---
name: interrogate
description: "Use for \"interrogate\", \"adversarial review\", \"multi-model review\", \"challenge this\", \"stress test this code\", \"find blind spots\", or \"tear this apart\". Reviewers from different model families challenge a diff from independent angles via dispatch, then Claude synthesizes a verdict."
disable-model-invocation: true
---

# Interrogate

Send the same diff and rubric to reviewers from **different model families**, then synthesize.
The adversarial signal comes from family diversity, not assigned personas. Models differ in blind
spots, priors, and reasoning patterns. Agreement across families is high-confidence signal; a lone
finding is worth reading at lower confidence.

The deliverable is a synthesized verdict. **Do not auto-apply changes.**

## The one rule that makes this legitimate

**You are the lead. You are not a reviewer.** Never spawn Claude subagents to review Claude's own
work — that mostly reproduces the parent's blind spots at full fan-out cost, and the workspace
CLAUDE.md forbids it. Reviewers reach other families through `dispatch`. The value is the
disagreement, not a second opinion from yourself.

This is also why the upstream version of this skill does not work here. It spawns four `Task`
subagents pinned to `gpt-5.6-sol-max`, `grok-4.6-fast-xhigh` and two Claude tiers. Those first two
slugs do not resolve in Claude Code, so the panel silently collapses onto one Claude model: full
fan-out cost, zero cross-family signal, and a rule violation. `dispatch` is how you actually reach
another family from here.

## Step 1, Determine scope

- User points at files or a diff, use that.
- On a feature branch: `git diff <base>...HEAD`. Donyati repos branch off `dev` (or `main` for
  DonyatiDocs and the website), **not** `main` by default. Check before assuming.
- Write the diff to a file. Reviewers receive it with `--file` (fixed 2026-08-20; on an older `dispatch`, inline it yourself).

```bash
git diff azure/dev...HEAD > /tmp/interrogate/diff.patch
```

If the diff is large, also write a short context file naming the entry points and any file the
reviewer needs to make sense of it.

## Step 2, State the intent

One clear paragraph: what is this change trying to accomplish? Derive it from the user's message,
commit messages, the PR description, and the code. Reviewers challenge whether the work *achieves*
the intent, not whether the intent is correct. If you cannot state it confidently, ask before
spawning anything — a panel pointed at the wrong intent produces confident noise.

## Step 3, Run the panel

⚠ **`dispatch status` reports configuration, not readiness.** A provider can show ENABLED and be
unusable. Verified 2026-08-20: `grok` showed ENABLED and returned *"Error: Not signed in."* on
every call. Check the config, then prove a provider with one cheap live call before you rely on it.

```bash
dispatch status                                  # config only
dispatch ollama "reply OK"                       # cheapest readiness probe
```

**`--file` was broken for codex, grok and opencode — fixed 2026-08-20.** Root cause was two
bugs in the `dispatch` wrapper, not in the providers:

1. `dispatch_codex` and `dispatch_opencode` passed the raw `$PROMPT` instead of the inlined
   version, so attachments were silently dropped. codex answered *"Which code should I
   inspect?"* and still billed 12,727 tokens.
2. The inlined block was prefixed `--- File: ... ---`. A **leading `--` is parsed as a CLI
   flag**, so codex and grok returned usage errors. Delimiter is now `===== FILE: ... =====`.

Both fixed in `~/Claude/config/dispatch/dispatch`, along with a duplicate inlining
implementation in `dispatch_ollama` that had drifted from the shared one. Verified after the
fix: codex given `--file` replies `FOUND`. **If you are on an older `dispatch`, inline the diff
into the prompt yourself.**

| Reviewer | Command | Family | State 2026-08-20 |
|---|---|---|---|
| A | `dispatch codex "<prompt>" --file diff.patch` | OpenAI GPT | ✅ works, ⚠ **thin window ~15–80 msgs / 5 hr** |
| B | `dispatch ollama "<prompt>" --file diff.patch` | local Qwen | ✅ works, free |
| C | `dispatch grok "<prompt>" --file diff.patch` | xAI Grok | ⚠ needs `grok login --device-code` or `XAI_API_KEY` |
| — | `dispatch gemini` | Google | ❌ disabled, unsupported CLI OAuth tier |

⚠ **`dispatch status` reports configuration, not readiness.** grok shows ENABLED and returns
*"Error: Not signed in."* Prove a provider with one cheap live call before relying on it.

Do **not** pass `--heavy` to ollama. The 235b model is pulled (142 GB) but returned "No
response" on a 101-line diff; the default `qwen3:30b-a3b` handled the same input.

⚠ **Release the GPU when done.** `qwen3:30b-a3b` holds ~44 GB with a keep-alive window and
made a 64 GB workstation unusable mid-session. `ollama stop <model>` after every run.

With grok unauthenticated and gemini disabled, today's working panel is **codex + ollama** —
two families. Say that in the verdict rather than implying more.

Run reviewers as **parallel Bash calls in a single message**. Do not use `--background` while you
are orchestrating: it self-detaches and nothing tells you when it finished. Background is for jobs
that must outlive the session.

**Budget the codex window deliberately.** It is the strongest reviewer and the scarcest resource,
and hitting its cap is disruptive. For a small or low-stakes diff, run B and C only and say in the
verdict that A was skipped to preserve the window. Never silently drop a reviewer — an
unreported skip reads as coverage that did not happen.

**Cascade on failure:** codex capped or erroring → `grok` → `ollama`. Report which reviewer
actually ran, not which one you intended to run.

Fill the prompt from [`references/reviewer-prompt.md`](references/reviewer-prompt.md) with:

1. The stated intent from Step 2
2. The rubric from [`references/rubric.md`](references/rubric.md)
3. The code-quality lens from [`references/code-quality-review.md`](references/code-quality-review.md)

The same filled template goes to every reviewer, so each family applies the same lens. Ask each for
structured findings as the template describes.

**Tell reviewers the review bar explicitly.** Report anything that could cause incorrect behavior, a
test failure, or a misleading result, each tagged with confidence and estimated severity; omit pure
style and naming nits. Never say "only report high-severity issues" — models follow that literally
and under-report. Finding is coverage; filtering is your job in Step 5.

## Step 4, Synthesize

1. **Parse all findings.**
2. **Identify consensus.** Raised independently by 2+ families is the highest signal this skill
   produces.
3. **Identify lone findings.** Worth reading, weighted lower.
4. **Deduplicate.** Different families describe the same issue differently. Merge and record who
   raised it.
5. **Note disagreements.** One family flagging what another explicitly clears is useful context.

## Step 5, Lead judgment

You are a pragmatic senior engineer, not a neutral aggregator. Read
[`references/lead-judgment.md`](references/lead-judgment.md). Reviewers saw a slice; you have the
goal, the constraints, and which tradeoffs were already settled. Use that context aggressively.

Bucket every finding:

- **Act on.** Real correctness, security, or maintainability issues given the actual goals. Would
  block a real PR.
- **Consider.** Legitimate, but the cost of addressing it now may not be worth it.
- **Noted.** Valid, not actionable. Context-dependent or low-impact at this stage.
- **Dismissed.** Wrong, nitpicky, or missing context. Say why in one line.

⚠ **Verify before escalating.** Roughly a third of findings from an unverified review pass do not
hold up, and severities skew high. Check a finding against the code before it reaches "Act on", and
never file a work item or tell a colleague a system is broken on a reviewer's say-so alone.

For each finding record which family raised it, the bucket, and a one-line rationale.

## Output format

### Intent
> [the paragraph from Step 2]

### Reviewers
- Reviewer A: codex (GPT), N findings — or `skipped: preserving window`
- Reviewer B: grok, N findings
- Reviewer C: ollama qwen3:235b, N findings

### Act on
[Description, which families raised it, why it matters.]

### Consider
[Description, families, the tradeoff.]

### Noted
[Brief list.]

### Dismissed
[With one-line rationale each, so the user can override your filtering.]

### Agreement map
Where families agreed, where they diverged, and what that pattern suggests. Consensus across three
families on one line of code is worth more than any single confident reviewer.

## Cleanup

**Stop the local model.** ollama keeps a model resident on the GPU after a run — `qwen3:30b-a3b`
holds ~44 GB with a keep-alive window, which is enough to make a workstation unusable. This has
already bitten us mid-review. Always:

```bash
ollama ps                          # what is resident
ollama stop qwen3:30b-a3b          # release it
rm -rf /tmp/interrogate
```

`ollama serve` itself is a small idle daemon and can stay. It is the loaded *model* that costs.
