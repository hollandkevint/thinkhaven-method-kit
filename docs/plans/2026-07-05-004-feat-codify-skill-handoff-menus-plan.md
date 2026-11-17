---
title: "feat: Add codify skill and per-skill handoff menus"
date: 2026-07-05
artifact_contract: ce-unified-plan/v1
artifact_readiness: implementation-ready
execution: code
product_contract_source: ce-plan-bootstrap
origin: docs/plans/2026-07-05-003-repo-best-practices-upgrade-brief.md
---

# feat: Add codify skill and per-skill handoff menus

## Summary

Implement P2 of the repo upgrade brief: close the decision loop. Add a `codify` skill that writes one durable decision record per decision to `decisions/`, a `/da:codify` command entry point, an AGENTS.md/README discoverability pointer, and a short handoff block at the end of each of the 13 existing SKILL.md files so no skill exits silently.

---

## Problem Frame

PHILOSOPHY.md claims "clarity compounds," but nothing in the kit persists a decision — briefs are produced in-chat and evaporate. And the 13 skills are dead ends: each produces output and stops, so the user must remember the flow themselves. The reference projects (compound-engineering's `/ce-compound`, BMAD's document-mediated handoff) both solve these with (a) a codify step writing one durable file and (b) mandatory next-step routing.

## Requirements

- **R1** — A `codify` skill exists at `skills/codify/SKILL.md` that captures a just-made decision into one markdown record at `decisions/<slug>.md`.
- **R2** — The record template carries: decision, date, context, reasoning, weakest assumption, what would change the call, and when to reuse this pattern.
- **R3** — Update-over-duplicate: before writing, the skill checks existing `decisions/*.md` for the same decision (topic overlap) and updates that file instead of creating a near-duplicate.
- **R4** — Discoverability, two audiences: (a) this repo's AGENTS.md documents the `decisions/` convention for contributors (ships in this change); (b) at runtime the skill checks the *consuming project's* AGENTS.md/CLAUDE.md for a `decisions/` pointer and — because that file is user-owned — *proposes* the one-line addition rather than silently appending.
- **R5** — A `codify` command at `commands/codify.md` invokes the skill directly. (Documented as `/da:codify`, matching the repo's existing convention — but see Open Questions: the namespace is unverified.)
- **R6** — Every one of the 13 existing SKILL.md files ends with a short "Next step" handoff block routing to 1-2 named next skills, placed before the standard footer.
- **R7** — All new/edited files follow AGENTS.md conventions: frontmatter shape, imperative voice, no persona, the ThinkHaven footer on skills.
- **R8** — README's flow table and skills table include `codify` so the shipped skill is discoverable.

## Key Technical Decisions

- **KTD1 — Records live in the *user's* project at `decisions/`, not in this repo.** The skill instructs the agent to write to `decisions/` in the user's working directory (creating it on first use), mirroring `/ce-compound` writing to the consuming repo's `docs/solutions/`. This repo ships the method, not the storage.
- **KTD2 — Overlap check is instruction-level, not tooling.** The skill tells the agent: list `decisions/`, read candidate matches, update on strong topic overlap, create otherwise. No scripts, no scoring rubric beyond a plain judgment instruction. (Ponytail: CE's 5-dimension parallel-subagent scorer earns its keep at their scale, not at one-file-per-decision.)
- **KTD3 — Handoff blocks are static routing derived from the README loop** (FRAME → PRESSURE → MAP → DECIDE → codify), 2-3 lines each, not adaptive menus. The routing map below is the authoritative content.
- **KTD4 — `codify` is the loop terminal.** Its own handoff points back to `/da:decide` for the next decision, not onward.

### Routing map (content for U4's handoff blocks)

| Skill | Routes to |
|-------|-----------|
| preflight | grill-me (pressure the plan) or problem-framing (problem still fuzzy) |
| problem-framing | assumption-mapper or grill-me |
| pull-framework | problem-framing (diagnose deeper) or preflight (ready to scope a session) |
| grill-me | assumption-mapper (extract what surfaced) or kill-go (ready for a verdict) |
| grill-with-docs | assumption-mapper or decision-brief |
| b2b-idea-vetting | kill-go (verdict still unclear) or codify (record the call) |
| assumption-mapper | kill-go (test-then-decide) or decision-brief (write it up) |
| arbitrage-audit | kill-go or decision-brief |
| kill-go | decision-brief (proceed/pivot: write it up) or codify (kill: record why) |
| decision-brief | codify (make the record durable) |
| ai-graduation | decision-brief or codify |
| innovation-diagnostic | kill-go or decision-brief |
| design-thinking-lite | assumption-mapper or decision-brief |
| codify (new) | done — `/da:decide` for the next decision |

---

## Implementation Units

### U1. Create the codify skill

**Goal:** `skills/codify/SKILL.md` capturing one decision into `decisions/<slug>.md` with update-over-duplicate and the AGENTS.md discoverability check.
**Requirements:** R1, R2, R3, R4, R7
**Dependencies:** none
**Files:** `skills/codify/SKILL.md`
**Approach:** Frontmatter matching siblings (`name`, `version: 1.0.0`, `description` with "Activates on…" triggers like "codify this decision", "record this decision", "save the decision record", `user-invocable: false`). Body sections: when to use (right after a decision session — kill-go verdict, decision-brief, /da:decide output); the record template (R2 fields, markdown code block, mirroring how kill-go/decision-brief embed their output templates); the write procedure (records anchored at the consuming *repo root's* `decisions/`, not the cwd; slug from the decision name; the pre-create check is unconditional — always list `decisions/` and read the closest match before creating; strong topic overlap → update in place with a dated addendum line, else create; name duplicate-drift as the failure signal that would justify tooling later); the discoverability check (if the consuming project's AGENTS.md or CLAUDE.md lacks a `decisions/` pointer, *propose* the one-line addition to the user — never silently edit their file; if neither file exists, say so in one line and skip); anti-sycophancy note (record the *actual* confidence and weakest assumption, not a sanitized version). End with KTD4 handoff + standard footer.
**Patterns to follow:** `skills/kill-go/SKILL.md` (embedded output template, protocol sections, footer), brief's P2 spec.
**Test scenarios:** Test expectation: none — markdown skill content, no executable behavior. Structural verification in U5 covers frontmatter validity, footer presence, and trigger phrasing.
**Verification:** File parses as valid YAML frontmatter + markdown; description contains activation triggers; footer matches the canonical two-line block used by 12 of 13 skills (`Part of the [ThinkHaven Method Kit]…` + `Full Board of Directors experience: [ThinkHaven](…)` (link removed 2026-11; the app was retired)).

### U2. Create the /da:codify command

**Goal:** `commands/codify.md` as the user-facing entry point.
**Requirements:** R5, R7
**Dependencies:** U1
**Files:** `commands/codify.md`
**Approach:** Mirror `commands/decide.md` shape (~17 lines): frontmatter (`name: codify`, `description`, `user-invocable: true`), 2-3 step body pointing at `skills/codify/SKILL.md`.
**Test scenarios:** Test expectation: none — command stub. Covered by U5 structural checks.
**Verification:** Frontmatter matches sibling commands; references the skill path that exists after U1.

### U3. Discoverability: AGENTS.md and README

**Goal:** The new skill and the `decisions/` convention are findable by agents and humans.
**Requirements:** R4 (initial pointer), R8
**Dependencies:** U1, U2
**Files:** `AGENTS.md`, `README.md`
**Approach:** AGENTS.md — add a `decisions/` note to the File Structure section (one line: consuming projects get a `decisions/` folder of durable records written by the codify skill; contributor documentation of the convention, not runtime discoverability — the skill's own check is what reaches consumers). README — add `codify` row to the skills table, add `/da:codify` to the commands table, and extend the flow: the README currently shows 4 stages (`FRAME → PRESSURE → MAP → DECIDE`); add `CODIFY` as the 5th stage with a Codify row in the stage table.
**Test scenarios:** Test expectation: none — docs. U5 checks the README references resolve.
**Verification:** README flow table has 5 stages; skills table row links to `skills/codify/SKILL.md`; AGENTS.md mentions `decisions/`.

### U4. Handoff blocks in all 13 existing skills

**Goal:** No skill exits silently — each SKILL.md ends with a "Next step" block per the routing map (KTD3), inserted before the footer.
**Requirements:** R6, R7
**Dependencies:** U1 (blocking — the codify skill must exist before handoff blocks can reference it)
**Files:** all 13 `skills/*/SKILL.md` (preflight, problem-framing, pull-framework, grill-me, grill-with-docs, b2b-idea-vetting, assumption-mapper, arbitrage-audit, kill-go, decision-brief, ai-graduation, innovation-diagnostic, design-thinking-lite)
**Approach:** One consistent shape, 2-3 lines, imperative voice, e.g. for kill-go: `## Next step` / `Proceed or pivot → run decision-brief to write it up. Kill → run codify to record why, so the lesson survives.` Insert immediately before the `---` + footer block. Content per skill comes verbatim from the routing map — do not invent alternate routes during implementation. Exception: `grill-with-docs` already carries a "Sits in the flow" routing sentence before its footer — replace that sentence with its Next step block rather than leaving two competing routings.
**Execution note:** Mechanical sweep; keep each block under 4 lines and do not restructure any other section (blast radius = append-only insertion, plus the one named replacement in grill-with-docs).
**Test scenarios:** Test expectation: none — markdown content. U5's sweep check verifies presence and placement in all 13.
**Verification:** `grep -L '## Next step' skills/*/SKILL.md` returns only nothing (codify included has its terminal variant); each block appears before the footer, not after.

### U5. Structural verification sweep

**Goal:** One runnable check that fails if the conventions break.
**Requirements:** R6, R7 (verification)
**Dependencies:** U1-U4
**Files:** none new (transient shell checks; per ponytail, no committed test harness for markdown content)
**Approach:** Run and report: (a) YAML frontmatter parses in all `skills/*/SKILL.md` + `commands/*.md` (python yaml on the `---` block); (b) every skill file contains both `## Next step` and the `Part of the [ThinkHaven Method Kit]` footer, in that order (use `grep -F` — the bracketed string is a regex character class otherwise); (c) every route target named in a Next step block exists as a skill directory, a command, or a `/da:*` command reference; (d) README links to `skills/codify/SKILL.md` resolve.
**Test scenarios:** The sweep IS the test. It must fail if: a frontmatter block is malformed, a handoff block is missing from any of the 14 skills, a block lands after the footer, or a route names a nonexistent skill.
**Verification:** All four checks pass; output pasted into the work summary.

---

## Scope Boundaries

**In scope:** the five units above.

**Deferred to Follow-Up Work:**
- Flipping the P2 status line in the origin brief (touch when shipping, trivial).
- P3 items (catalog index, methods CSV) — explicitly deferred by the brief.
- Example decision record in `examples/` — nice-to-have, add if a real session produces one.

**Out of scope:** scripts/tooling for overlap detection (KTD2), adaptive handoff menus, any restructuring of existing skill bodies, `decisions/` folder in *this* repo (records belong to consuming projects).

## Assumptions

Headless run — these inferred bets were not user-confirmed:
- **A1** — Handoff block heading is `## Next step` (singular, sentence case) placed before the footer; exact wording per skill is implementer's judgment within the routing map.
- **A2** — README gets the codify updates (R8) even though the brief listed only AGENTS.md; a shipped skill invisible in the README contradicts the discoverability principle the brief itself argues for.
- **A3** — `board-of-directors` is a prompt, not a skill, so it gets no handoff block.
- **A4** — Plugin version bump (`plugin.json` 1.1.0 → 1.2.0) happens at ship time in the commit, per the brief's P4 note; not a unit here.

## Open Questions

- **OQ1 (from doc review, P1, manual)** — The `/da:` command namespace is asserted throughout this repo but has never been live-verified. Claude Code namespaces plugin commands by the `plugin.json` name (`thinkhaven-method-kit`), so `/da:codify` may actually resolve as `/thinkhaven-method-kit:codify`. This plan follows the existing repo convention (`/da:codify` in docs) so it stays consistent either way, but the user should run the live install test (`/plugin marketplace add hollandkevint/thinkhaven-method-kit`, then list commands) — if the namespace differs, fixing the repo-wide `/da:` references is a separate follow-up change, not part of this plan.

## Definition of Done

- All five units complete; U5 sweep passes with output shown.
- 14 skills (13 existing + codify) each end with a Next step block + footer, in that order.
- `/da:codify` and the codify skill exist and cross-reference correctly.
- README and AGENTS.md updated; no other files touched beyond the listed set.
