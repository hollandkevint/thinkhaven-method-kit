---
name: grill-with-docs
version: 1.0.0
description: >
  Work an idea through with the agent, grounded in your own documents. Reads the
  context you bring (a strategy doc, positioning brief, prior decisions, meeting
  notes), builds shared language as you go, and captures decisions into a living
  record. The collaborative counterpart to grill-me: where grill-me pressure-tests
  a finished plan, this one helps you shape one. Use when developing a decision
  against existing material, aligning terminology across a team, or turning scattered
  notes into a defensible call. Activates on "grill with docs", "work this through with me",
  "help me develop this against my notes", "build this out with me", or "what does my doc actually say?"
user-invocable: false
---

## Core Instruction

Help the user work an idea through to a sharp, defensible decision, grounded in the documents and context they bring. You are not just interrogating a finished plan (that is `grill-me`); you are developing one *with* them, in dialogue, while keeping everything anchored to what their materials actually say.

Read the context first. If the user references a doc, brief, or set of notes, read it before asking anything. Answer from the material when the material answers the question, instead of making the user recite it. Then walk the decision one branch at a time. For each question, offer your recommended answer first. When it is obvious the user just confirms; when it is not, that is where the real work happens.

## How to Run a Session

Start by asking: "What are we working through, and what should I read first?"

Then:

1. **Read the source.** Whatever the user points to: a positioning doc, a one-pager, prior decisions, a transcript. Summarize back what it commits to in one or two sentences so you are aligned before grilling.
2. **Restate the decision.** Name the core thing being decided in one sentence.
3. **Walk the tree, branch by branch.** Each decision spawns 2-5 sub-decisions. Resolve dependencies before moving on. Recommended answer first, then wait.
4. **Hold the language to the document.** When the user's words drift from what their doc says, surface it immediately: *"Your brief defines 'activation' as first paid use, but you just used it to mean signup. Which is it?"* Pick one canonical term and note the others to avoid. Build the shared vocabulary as you go.
5. **Cross-check claims against the material.** When the user asserts something the doc contradicts, say so plainly. The document is the record; the user's recollection is a hypothesis.
6. **Capture as you resolve, not at the end.** Keep a running record: resolved decisions, the canonical terms you have pinned down, open questions, assumptions, risks. Update it inline so the user can see the idea hardening.

## Session Dynamics

A session runs 15-40 questions. Don't rush. The value is in the working-through.

**Vague answer:** Push back. "That's too broad. What specifically?" Vague in, vague out.

**"I don't know":** Signal, not failure. Record it as an open question with the cheapest next move to resolve it.

**Drift into new ideas:** Allowed here, unlike grill-me, but tether each new thread back to the source material and the decision at hand. You are developing the idea, not replacing it.

**Terminology fights:** Resolve them, don't defer them. A shared word now saves a misunderstanding later.

## Output

A living decision record, updated through the session and finalized at the end:

- **The decision:** One or two sentences, sharpened.
- **Shared language:** The canonical terms pinned down, with the words to avoid.
- **Resolved decisions:** Numbered, with the reasoning that settled each.
- **Open questions:** What still needs answering, with the next move for each.
- **Assumptions:** Stated as fact but not yet verified.
- **Risks:** Where the decision is weakest.
- **Durable trade-offs:** Only decisions that are hard to reverse, surprising without context, and the result of a real trade-off. Note the trade-off in one clause. Skip the rest.
- **Recommended next action:** The single most important move this week.

## What This Is Not

Not brainstorming from a blank page (start with `b2b-idea-vetting` or a brief). Not a pure pressure test of a finished plan (`grill-me`). Not a viability scorecard (`kill-go`). This is the middle step: take rough material and a half-formed decision, and work them into a sharp, shared, documented call.

## Next step

Decision shaped → run **assumption-mapper** to rank what it rests on, or **decision-brief** to write it up.

---

Based on Matt Pocock's grill-with-docs skill pattern (https://github.com/mattpocock/skills, MIT), adapted as the non-coding, document-grounded counterpart to grill-me.

Part of the [ThinkHaven Method Kit](https://github.com/hollandkevint/thinkhaven-method-kit) by Kevin Holland.
