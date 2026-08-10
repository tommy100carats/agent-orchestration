# Agent Topology

How the multi-agent system is structured so each agent has clear ownership and the whole thing stays auditable.

## Principles

1. **One agent = one responsibility.** No agent does "everything." Each owns a single pipeline stage.
2. **Written handoffs.** Agents pass structured data (a note/file with typed fields), not prose chatter.
3. **A single human gate.** Nothing leaves the system until the human reviews and acts.
4. **Isolated sessions.** Each agent runs in its own context. Cross-contamination is avoided by design.

## Topology

```
                   ORCHESTRATOR (human architect)
                   - defines plan, no-go rules, thresholds
                   - routes models, validates output
                            │
        ┌──────────┬────────┴────────┬──────────┬──────────┐
        ▼          ▼                 ▼          ▼          ▼
  [1] SOURCING  [2] ENRICH    [3] BUILD   [4] GENERATE  [5] AUDIT
   search       research      structure   produce     verify
   score        decision-     content     tailored    active?
   dedup        makers                   deliverable consistent?
        └──────────┬────────┴────────┬──────────┬──────────┘
                   ▼                  ▼          ▼
            SHARED STATE (per-item record, typed fields)
                   ▼
            HUMAN-IN-THE-LOOP GATE  →  deploy / send / done
```

## Session model

| Concern | Rule |
|---------|------|
| Project isolation | One session per project. Never two projects in one context. |
| Memory | Per-project `MEMORY.md`, updated end of session. |
| Controller files | Short (< 200 lines). Heavy detail in pointed annexes. |
| Model routing | Plan + final review on strong model; execution on fast model; heavy extraction in sub-agent. |

## State model

Each item flowing through the pipeline is a **record with typed properties** (status, score, links, flags). The `status` field drives both tracking and the learning loop.

Status lifecycle example:

```
to qualify → qualified → built → deliverable ready → verified → [done | expired]
                                        │
                                   HUMAN GATE
```

## Failure modes this design prevents

- **Hallucination propagation** — the audit agent runs fresh and checks every field.
- **Silent drift** — the learning loop captures rejections *with reasons*, not just a binary no.
- **Context bloat** — progressive disclosure keeps each agent's context lean.
- **One-big-prompt fragility** — if one stage fails, only that agent re-runs.
