# Agent Orchestration — Multi-Agent Workflows for Ops & Revenue

> **Archived (October 2026).** This repository is an early draft from August 2026 and is no longer maintained.
> The system that actually runs in production, with tested code, CI and documentation, is
> **[tommy100carats/cdi-agents](https://github.com/tommy100carats/cdi-agents)**.

> How a non-developer runs a 5-agent daily pipeline in production — architecture, methodology, and reusable patterns.

I build and operate **multi-agent systems** to run recurring business operations end-to-end: sourcing, enrichment, content generation, and quality audit. I'm not a coder — I'm the **architect and orchestrator**: I design the system, delegate execution to coding agents (Claude Code), and stay in the human-in-the-loop for judgment calls.

This repo documents the **methodology and architecture** I use. It contains no client data, no credentials, and no private CRM — only the reusable scaffolding.

> The n8n workflow that used to live in `workflows/` was removed: it did not run as published. See `cdi-agents` for the maintained, tested version of these principles.

---

## What this repo is

A reference for how to design, structure, and operate a multi-agent ops pipeline:

- **Architecture** — agent topology, session isolation, model routing.
- **Methodology** — plan-before-execute, atomic decomposition, fresh-context self-review.
- **Pipeline pattern** — a fully worked 5-agent daily workflow (anonymized).
- **Prompts** — generic, reusable templates (not client-specific).
- **Results** — what the system actually delivers (anonymized metrics).

## What this repo is NOT

- Not a mirror of any private notes or CRM.
- Not client deliverables, leads, or personal data.
- Not the business/agency side — this is the **engineering & ops** layer only.

---

## The core idea

Most "AI automation" stops at a single prompt. The leverage comes from **orchestrating specialized agents** that each own one stage of a pipeline, with a human gate before anything goes live.

```
        ┌─────────────────────────────────────────┐
        │            ORCHESTRATOR (Tom)            │
        │   design · routing · validation · deploy │
        └─────────────────────────────────────────┘
                          │
       ┌──────────┬───────┴───────┬──────────┬──────────┐
       ▼          ▼               ▼          ▼          ▼
  [1 SOURCING] [2 ENRICH]   [3 BUILD]  [4 GENERATE] [5 AUDIT]
   search+score  research    structure   produce      verify
   +dedup        decision-    content     tailored     active?
                makers                  deliverable   consistent?
       └──────────┴───────┬───────┴──────────┴──────────┘
                          ▼
                 HUMAN-IN-THE-LOOP GATE  →  deploy / send
```

---

## Methodology

| Principle | What it means in practice |
|-----------|---------------------------|
| **Plan before execute** | Every run starts from a written plan + explicit no-go rules. |
| **Atomic decomposition** | Each agent does one thing, writes even if a field is missing (`to verify`), never invents. |
| **Fresh-context review** | The final audit runs in a clean context, checking for hallucinations and consistency. |
| **Progressive disclosure** | Short controller files (< 200 lines); heavy detail lives in pointed annexes. |
| **Capitalize** | Every deliverable becomes a reusable asset (template, skill, workflow). |
| **Quantified options** | Decisions come with trade-offs and numbers, not vibes. |

### Session & context hygiene
- **One session = one project.** Never load two projects into the same context.
- **Per-project memory** updated at end of each session (decisions, returns, assets).
- **Model routing** (indicative): planning + final review on the strong model; execution on a fast model; heavy extraction in a sub-agent.

---

## Pipeline pattern (anonymized)

A daily ops pipeline I run in production. Target: **N qualified, fully-prepared items per day**, each ready for human validation.

### Execution order
1. **Sourcing** — reads sources + targets + no-go rules, searches, scores, de-duplicates. Creates up to N items (status `to qualify`). Stops at N; never fills to fill.
2. **Enrichment** — for each item, identifies the right contact (name + role + public profile). Public data only, no private emails.
3. **Build** — fills the structured content for each item from the master profile + research.
4. **Generate** — produces the tailored deliverable (e.g. adapted CV/document) from a master template.
5. **Audit** — verifies the item is still live, consistent, nothing invented. Marks `verified` or `expired`.

### Caps & thresholds
- **Max N items/day.** Keep only items scoring ≥ threshold.
- If fewer than N good items exist, produce fewer — never pad.

### Learning loop ("training the agents")
- When the human rejects an item **with a reason**, the orchestrator summarizes the learned rule into project memory.
- The sourcing agent re-reads that section every run → filters better over time.
- **Heuristic memory learning, not fine-tuning.**

### Human role (human-in-the-loop)
- Agents stop at `ready`.
- Human reviews, adjusts, **acts** (candidates / sends), then flips status to `done` and logs the date.

---

## Why this matters for AI/Ops roles

- I can **design and operate** a production multi-agent system, not just prompt one model.
- I treat agents like a team: clear ownership, interfaces, gates, and a feedback loop.
- I optimize for **reliability and auditability** (no invention, verified outputs) over demo flashiness.

---

## Structure

```
agent-orchestration/
├── README.md
├── architecture/        # agent topology, session model, model routing, diagrams
├── methodology/         # the rules above, expanded
├── pipelines/           # the anonymized 5-agent pipeline
├── prompts/             # generic, reusable agent prompt templates
├── results/             # anonymized metrics the system delivers
```

## System diagram

See [`architecture/DIAGRAM.md`](architecture/DIAGRAM.md) for the Mermaid topology (agent graph + unattended automation).

## Stack
- Orchestration: Claude Code (parallel sessions)
- No-code / glue: n8n
- Knowledge base: structured markdown + database-style views
- Host: Windows (GUI-first), units: metric

---

*This is the engineering & ops layer of how I work. The business context stays private.*
