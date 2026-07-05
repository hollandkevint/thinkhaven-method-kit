---
name: codify
version: 1.0.0
description: >
  Capture a just-made decision into one durable record so the reasoning survives
  the session. Writes a single markdown file to the project's decisions/ folder:
  the call, the reasoning, the weakest assumption, what would change the verdict,
  and when to reuse the pattern. Checks for an existing record on the same decision
  and updates it instead of creating a near-duplicate. Activates on "codify this
  decision", "record this decision", "save the decision record", "make this decision
  durable", or right after a kill-go verdict, decision-brief, or /da:decide session.
user-invocable: false
---

# Codify

Use this immediately after a decision is made — a kill-go verdict, a finished decision-brief, the end of a `/da:decide` session, or any call the user just committed to. The job is to make the decision durable: one file, one decision, written so a reader six months from now understands what was decided, why, and when to reuse the pattern.

Clarity compounds only if it's captured. A decision that lives in chat history is a decision the team will re-litigate.

## The Write Procedure

1. **Anchor the location.** Records go in `decisions/` at the root of the user's project repo — not the current working directory if that's a subdirectory, and never inside this plugin. Create `decisions/` on first use.
2. **Slug the decision.** Derive a short kebab-case filename from the decision itself: `decisions/kill-freemium-tier.md`, `decisions/pivot-to-eval-service.md`.
3. **Check before creating — always.** List `decisions/` and read the closest-matching existing record before writing anything. (No directory-listing tool available? Ask the user to name any existing record on this decision — do not skip the check silently.) If an existing record covers the same decision (same call being made or revisited, even under a different name), update that file: revise the fields that changed and add a dated addendum line under Context. Create a new file only when no record covers this decision. Near-duplicate records are the failure mode this step exists to prevent — if the folder accumulates two records for one decision, that drift is the signal this check needs tightening.
4. **Fill the template honestly.** Record the actual confidence and the genuinely weakest assumption, not a sanitized version. If the decision was a kill, say kill and say why. The record is for truth, not for looking decisive.

## The Record Template

```markdown
# Decision: [the call, stated as a decision, not a topic]
Date: [YYYY-MM-DD]

## Context
[2-3 sentences: what prompted this decision, what was at stake]

## The Call
[What was decided. One or two sentences, no hedging.]

## Reasoning
[Why this call over the alternatives. The strongest case that won.]

## Weakest Assumption
[The assumption most likely to be wrong, stated plainly]

## What Would Change This
[Specific evidence or events that would reopen the decision]

## Reuse This Pattern When
[The situations where this decision's logic applies again]

## Confidence
[High / Medium / Low — matched to evidence quality, not enthusiasm]
```

## Discoverability Check

After writing the record, check whether the project's AGENTS.md or CLAUDE.md points agents at `decisions/`. If neither file mentions it, **propose** the one-line addition to the user — state the exact line in your response and ask before writing it. For example: "Durable decision records live in `decisions/` — read the relevant record before revisiting a past decision." Never silently edit the user's context files; they own them. If neither file exists, say so in one line and move on.

## Anti-Sycophancy Note

Do not soften the record to make the decision look better than it was. A record that says "Confidence: High" over thin evidence is worse than no record — it will be trusted and it will be wrong. If the user pushes to upgrade the confidence or bury the weakest assumption, ask: "What evidence moved since the session?" No new evidence, no upgrade.

## Next step

The loop is closed — the decision is durable. Start the next one with `/da:decide`, or test this record's weakest assumption before it tests you.

---

Part of the [ThinkHaven Method Kit](https://github.com/hollandkevint/thinkhaven-method-kit) by Kevin Holland.
Full Board of Directors experience: [ThinkHaven](https://thinkhaven.co/try)
