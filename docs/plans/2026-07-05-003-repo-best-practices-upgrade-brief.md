# Brief: Upgrade ThinkHaven Method Kit with OSS Plugin Best Practices

**Date:** 2026-07-05
**Author:** Kevin Holland (drafted with Claude)
**Sources reviewed:** [compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) (Every Inc / Kieran Klaassen), [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)

## The one-line

The kit has strong *content* (13 skills, a facilitator agent, good anti-sycophancy frontmatter) but ships it as a **catalog**, not a **flow** — newcomers can't tell there's an intended sequence, decisions don't compound into anything durable, and the install manifest is broken. Fix the flow, close the loop, fix the manifest.

## What the two reference projects do that we don't

Both projects converge on the same spine. It's not aesthetic — it's what makes a multi-skill kit usable by someone who didn't build it.

| Pattern | compound-engineering | BMAD-METHOD | We have it? |
|---|---|---|---|
| **Flow-first README** — lead with a copy-paste sequence, not a skill list | `/ce-brainstorm → /ce-plan → /ce-work → /ce-compound` printed up top | Named 4-phase spine (Analysis→Planning→Solutioning→Implementation) | ❌ catalog-first |
| **A named loop** the whole kit is organized around | Plan → Work → Review → Compound | Progressive: each phase's doc is the next phase's input | ⚠️ the facilitator encodes one, but it's undocumented |
| **"Start here" one-liner** | `/ce-setup` capability probe | `bmad-help` "what's next?" oracle | ⚠️ "start with b2b-idea-vetting", no sequence |
| **Codify / compound step** — the loop writes one durable file | `/ce-compound` → `docs/solutions/[cat]/*.md`, update-over-duplicate | Document-mediated handoff; each artifact persists | ❌ briefs are produced but not captured |
| **Discoverability catalog** — machine/human index mapping skill → phase → next | `docs/skills/README.md` grouped by role | `module-help.csv` (menu-code, phase, prev/next, output-location) | ❌ only README tables |
| **Handoff menus** — every skill routes to the next, no silent exit | completion contract in each SKILL.md | `preceded-by`/`followed-by` in the catalog | ❌ none |
| **"Use when…" trigger clauses** in frontmatter | every skill | trigger presence is a validation tier | ✅ we already do this ("Activates on…") |
| **Valid marketplace manifest** | `owner` + `plugins[]` w/ `source: "./"` | npm module registry | ❌ **broken** (see P0) |
| **Reusable elicitation backed by a method CSV** | `references/` stub-and-load | `advanced-elicitation` + `methods.csv` (71 methods) | ❌ inlined per skill |
| **Contributor + versioning hygiene** | release-please, `validate-frontmatter.py`, CONCEPTS.md | channels, CONTRIBUTING, lint-as-config | ⚠️ minimal |

## The roadmap (phased, by effort × leverage)

### P0 — Fix the bug (minutes, do first)

**`.claude-plugin/marketplace.json` is not a valid marketplace manifest.** It currently holds `{slug, category, featured}`. Claude Code's `/plugin marketplace add hollandkevint/thinkhaven-method-kit` (the README's install command) expects a manifest with a top-level `name`/`owner` and a `plugins[]` array pointing at the plugin source. As written, install likely fails.

Fix (modeled on compound-engineering's working file):
```json
{
  "name": "thinkhaven-method-kit",
  "owner": { "name": "Kevin Holland", "url": "https://github.com/hollandkevint" },
  "plugins": [{
    "name": "thinkhaven-method-kit",
    "source": "./",
    "description": "...",
    "tags": ["decision-making", "anti-sycophancy", "b2b", "product-management"]
  }]
}
```
**Verify by actually running `/plugin marketplace add` against the repo before trusting it.** (The old `{slug, category, featured}` object may have been a submission entry for an *external* registry — if so, keep it somewhere else, don't leave it as the marketplace.json.)

*Status: applied in this pass — needs your live-install test.*

### P1 — Make it a flow, not a catalog (hours) ← the high-leverage move

1. **README rewrite, flow-first** (*applied in this pass*): lead with the loop, a copy-paste quickstart, and scenario sequences; keep every catalog table. The loop the facilitator already runs, made explicit:

   > **Frame → Pressure → Map → Decide → (Codify)**
   > `preflight`/`problem-framing` → `grill-me`/`b2b-idea-vetting` → `assumption-mapper` → `kill-go`/`decision-brief`

2. **Scenario entry points** (CE's biggest onboarding win): 3-4 copy-paste blocks so a user self-selects by situation instead of guessing which of 13 skills to open — e.g. "vetting a B2B idea", "stuck mid-decision", "pre-launch pressure test", "post-mortem".

3. **Document the express lane:** `/da:decide` runs the whole loop via the facilitator. Say so at the top — it's the `/lfg` of this kit.

### P2 — Close the loop so decisions compound (½–1 day) ← the thesis, made real

The PHILOSOPHY claim is "clarity compounds" and "encoded judgment appreciates." Right now nothing encodes it. Add the missing step:

4. **A `codify` skill** (steal `/ce-compound` wholesale): after a decision, write ONE record to `decisions/[slug].md` — decision, reasoning, weakest assumption, what would change the call, and *when to reuse this pattern*. Adopt CE's **update-over-duplicate** (score overlap against existing records; update instead of drifting) and its **discoverability check** (if `AGENTS.md` doesn't point agents at `decisions/`, add the pointer). This is the single highest-value structural add — it turns the kit from a set of one-shot tools into a compounding system.

5. **Per-skill handoff menus:** end each SKILL.md with "next step" routing (e.g. `assumption-mapper` → "Rank done. Run `kill-go` for a proceed/pivot/kill call, or `decision-brief` to write it up."). No silent exits. Cheap, and it's what stitches the catalog into a path.

### P3 — Discoverability + reuse (1 day, do when skill count grows)

6. **A catalog index** — `docs/skills/README.md` (CE style: name + tagline + 2-4 bullets, grouped by loop stage). Optionally BMAD's machine-readable `catalog.csv` (skill, stage, preceded-by, followed-by, output) if you later add a `/da:next` "what's next?" oracle. Skip the CSV until you have ~20+ skills — proportionally too much overhead now.

7. **Shared elicitation method library** — a `methods.csv` of decision lenses (pre-mortem, red-team, first-principles, second-order effects, inversion) that skills select from, instead of re-inlining lens lists. BMAD's `advanced-elicitation` is near-turnkey to port. Grows without touching skill code.

### P4 — Hygiene (ongoing, low urgency)

8. `CONTRIBUTING.md` + a `CONCEPTS.md` glossary (four-layer model, arbitrage gap, anti-sycophancy, decision brief — so terms mean the same thing across skills).
9. A `scripts/validate-frontmatter.py` parser-safety check if you accept outside contributions.
10. Version discipline: bump `plugin.json` version on each shipped change; keep a short `CHANGELOG.md`.

## What NOT to copy

- **BMAD's step-file sharding** (one `step-NN.md` per workflow step) and its 3-tier validation harness earn their keep at 34+ workflows. At 13 skills the naming/validation overhead outweighs the token savings. Adopt the *pattern* (ordered, self-contained procedures) only if a specific skill gets long.
- **CE's multi-platform mirror dirs** (`.codex-plugin/`, `.cursor-plugin/`, …). AGENTS.md already makes prompts/frameworks tool-agnostic; don't maintain converted copies until there's demand.
- **A full expansion-pack / module registry.** One plugin, one method. Don't add module infrastructure for a kit this size.

## Recommended order

P0 now (bug) → P1 now (flow — the thing a newcomer hits first) → P2 next session (codify, the compounding payoff) → P3/P4 as the kit grows.

P0 and P1 are applied in this pass. P2+ await your go.
