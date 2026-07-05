---
name: codify
description: "Capture the decision just made into one durable record in decisions/. Checks for an existing record on the same decision and updates it instead of duplicating."
user-invocable: true
---

# Codify

Run the `codify` skill on the decision just made in this session.

## Steps

1. Identify the decision to record: the most recent kill-go verdict, decision-brief, `/da:decide` output, or the call the user names.
2. Follow `skills/codify/SKILL.md`: anchor to the project root's `decisions/`, check for an existing record first, update-over-duplicate, fill the template honestly.
3. Run the discoverability check: if the project's AGENTS.md/CLAUDE.md lacks a `decisions/` pointer, propose the one-line addition.

Skill definition: `skills/codify/SKILL.md`.
