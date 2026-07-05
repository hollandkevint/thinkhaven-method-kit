# ThinkHaven Method Kit

Open decision architecture for people who need sharper judgment before they build, pitch, publish, or advise.

This is the public method layer behind [ThinkHaven](https://thinkhaven.co): prompts, skills, playbooks, and frameworks for turning messy ideas into defensible decisions. Run the methods manually from this repo, or use ThinkHaven when you want the hosted version with adaptive facilitation, memory, artifacts, and a personal board of advisors.

## Quickstart

**Install:**

```bash
/plugin marketplace add hollandkevint/thinkhaven-method-kit
```

Or copy individual files from `skills/`, `prompts/`, and `frameworks/` into your own AI workspace.

**Then pick your entry point by situation — copy a block and run it:**

```text
Vet a B2B idea, angle, or offer   →  b2b-idea-vetting        (20 min, all-in-one)
Run a full decision, guided       →  /da:decide              (facilitator runs the whole loop)
Stuck in the middle of one        →  problem-framing → grill-me → kill-go
Pressure-test before you ship     →  preflight → grill-me → decision-brief
Size up a market or industry      →  arbitrage-audit
```

New here and just want one thing to try: run **[`b2b-idea-vetting`](skills/b2b-idea-vetting/SKILL.md)**. It's a 20-minute session that takes any B2B idea, positioning angle, offer, content thesis, or client recommendation and returns a decision artifact — decision under review, strongest case, weakest assumption, evidence gaps, recommended next move, confidence level.

## The flow

Every skill in the kit is a stage in one loop. You can run the whole thing with `/da:decide` (the facilitator threads it for you), or run stages by hand:

```text
FRAME  →  PRESSURE  →  MAP  →  DECIDE  →  CODIFY
```

| Stage | What happens | Skills / prompts |
|-------|--------------|------------------|
| **Frame** | Name the real decision and the failure modes before touching a solution. | `preflight`, `problem-framing`, `pull-framework` |
| **Pressure** | Attack the idea. Surface what you're assuming, get pushback instead of validation. | `grill-me`, `grill-with-docs`, `b2b-idea-vetting`, `board-of-directors` |
| **Map** | Extract and rank the assumptions; find what to test first. | `assumption-mapper`, `arbitrage-audit` |
| **Decide** | Make the call and write it down so it holds up later. | `kill-go`, `decision-brief` |
| **Codify** | Capture the decision into a durable record in `decisions/` so the reasoning survives the session. | `codify` |

The output is a short, shareable decision brief you keep. Anti-sycophancy runs through every stage — the kit is built to tell you to kill an idea when the evidence says kill. See [PHILOSOPHY.md](PHILOSOPHY.md) for why.

## What this is

ThinkHaven Method Kit is for the moment before commitment:

- before you build the feature
- before you sell the offer
- before you publish the point of view
- before you recommend the strategy to a client
- before you walk into the meeting where the decision has to hold up

The kit is intentionally lightweight. It does not try to be a self-hosted ThinkHaven clone. It exposes the method: the questions, pressure tests, briefs, and anti-sycophancy patterns that make decisions harder to fool yourself about.

## What's inside

### Skills

| Skill | What it does |
|-------|-------------|
| **[b2b-idea-vetting](skills/b2b-idea-vetting/SKILL.md)** | 20-minute pressure test for a B2B idea, positioning angle, offer, content thesis, or client recommendation. Produces a concrete decision artifact. |
| **[preflight](skills/preflight/SKILL.md)** | 7-question exercise before any AI session. Forces clarity on outcomes, failure modes, and hard parts. Based on Nate Jones' Preflight. |
| **[grill-me](skills/grill-me/SKILL.md)** | Decision tree exploration. "Interview me relentlessly about every aspect of this plan." Non-coding variant of Matt Pocock's viral skill. |
| **[grill-with-docs](skills/grill-with-docs/SKILL.md)** | Work an idea through with the agent, grounded in your own documents. Builds shared language and captures decisions inline. The collaborative counterpart to grill-me. |
| **[kill-go](skills/kill-go/SKILL.md)** | 5 pointed questions that produce a viability score and a proceed/pivot/kill recommendation. |
| **[assumption-mapper](skills/assumption-mapper/SKILL.md)** | Extract and rank assumptions from any idea. Identifies what to test first. |
| **[pull-framework](skills/pull-framework/SKILL.md)** | Project/Urgent/List/Limitations. Structured problem discovery before solution design. |
| **[decision-brief](skills/decision-brief/SKILL.md)** | One-page decision brief that passes the Simple/Surprising/Easy test. |
| **[arbitrage-audit](skills/arbitrage-audit/SKILL.md)** | 3 diagnostic questions: What inefficiency is this built on? How fast can AI close it? What new gap does closure create? |
| **[ai-graduation](skills/ai-graduation/SKILL.md)** | 7-move decision gate for AI deployment: contain, instrument, verify, prove, narrow, widen, standardise. |
| **[problem-framing](skills/problem-framing/SKILL.md)** | 9-step systematic diagnosis. Define, diagnose, root cause, force analysis, solution generation, evaluation. |
| **[innovation-diagnostic](skills/innovation-diagnostic/SKILL.md)** | Lightweight innovation assessment. Strategic context, market analysis, disruption identification, go/no-go gates. |
| **[design-thinking-lite](skills/design-thinking-lite/SKILL.md)** | 7-step human-centered design: context, empathize, define, ideate, prototype, test, iterate. |
| **[codify](skills/codify/SKILL.md)** | Capture a just-made decision into one durable record in `decisions/`. Update-over-duplicate; the reasoning survives the session. |

### Agents

| Agent | What it does |
|-------|-------------|
| **decision-facilitator** | Threads one decision across stages: framing intake, pressure tests, assumption mapping, and decision brief. Anti-sycophancy first. Invoke via `/da:decide` or `@decision-facilitator`. |

### Commands

| Command | Output |
|---------|--------|
| `/da:decide` | Runs the decision-facilitator agent end-to-end. Decision-brief output. |
| `/da:run-preflight` | `preflight-brief-<name>.md` |
| `/da:challenge-idea` | `challenge-report-<name>.md` |
| `/da:map-arbitrage-gaps` | `gap-map-<name>.md` |
| `/da:diagnose-problem` | `problem-diagnosis-<name>.md` |
| `/da:validate-innovation` | `innovation-assessment-<name>.md` |
| `/da:codify` | `decisions/<slug>.md` — durable decision record |

### Prompts

Works with any LLM:

- **anti-sycophancy-system-prompt** — forces pushback instead of validation
- **board-of-directors** — multi-perspective prompt with advisors who disagree
- **grill-me-non-coding** — decision tree exploration for strategy, hiring, GTM, or any non-code decision
- **arbitrage-gap-diagnostic** — 3 questions for evaluating any market or industry

### Frameworks

Reference documents for decision architecture practice:

- **four-layer-decision-engineering** — Dashboard Craft > Decision Context > Decision Intent > Decision Infrastructure
- **cognitive-extensions-taxonomy** — Maps cognitive modes to decision session types

## ThinkHaven vs. the Method Kit

| Use this repo when... | Use ThinkHaven when... |
|-----------------------|------------------------|
| You want a transparent method you can inspect, fork, and run manually. | You want a guided session that adapts to your answers. |
| You are testing whether a prompt or playbook helps your thinking. | You want memory, artifacts, and board-style pressure over time. |
| You want to bring the method into your own AI workspace. | You need a decision you can defend and share. |

ThinkHaven is the hosted execution layer. This repo is the open method.

## The thesis

When building cost approaches zero, the scarce resource is knowing what to build. These skills structure the path from "I have an idea" to "I know what to do." See [PHILOSOPHY.md](PHILOSOPHY.md) for the full argument.

## Examples

- [`jonathan-style-content-angle.md`](examples/jonathan-style-content-angle.md) — vetting a B2B content/positioning angle
- [`product-manager-feature-bet.md`](examples/product-manager-feature-bet.md) — vetting a product feature bet

## Related

- **[ThinkHaven](https://thinkhaven.co/try)** — The hosted Board of Directors experience: AI advisors with distinct worldviews who pressure-test your strategy
- **[BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)** — Full agent framework with 21+ specialized agents and 50+ workflows
- **[data-product-operator](https://github.com/hollandkevint/data-product-operator)** — Skills for data product managers

## Credits

- Nate Jones — Preflight framework, Arbitrage Gap taxonomy
- Matt Pocock — grill-me skill pattern
- Frederick P. Brooks — "The Design of Design" decision tree concept
- Stuart Winter-Tear — AI Graduation Path
- Alex MH Smith — Creative brief test: Simple, Surprising, Easy
- Pavel Samsonov — Problem framing philosophy
- BMAD Creative Intelligence Suite — Problem-solving, innovation strategy, design thinking workflows

## License

MIT
