# Daily Pipeline — Anonymized Reference

A production pipeline that runs **once per day** and produces **N fully-prepared items**, each ready for human validation. The names, domain, and data below are anonymized; the *structure* is real and reusable.

## Mission
Produce N qualified, personalized items per day, prepared end-to-end, ready for the human to validate and send.

## Target (example shape)
- **Roles:** Manager · Operations · Revenue Operations · Project Management.
- **Zone:** metro area + remote.
- **Floor comp:** band expressed as a range.
- **No-go:** pure cold outreach as the core of the role (exceptions: minor part, or large enterprise).

## Execution order

| # | Agent | Inputs | Output | Status set |
|---|-------|--------|--------|-----------|
| 1 | **Sourcing** | sources, inbox, targets, no-go rules, learned prefs | up to N items, scored + deduped | `to qualify` |
| 2 | **Enrichment** | each item of the day | decision-maker (name + role + public profile) | `enriched` |
| 3 | **Build** | master profile + research | 3 "why me" + 3 "why this fits" + context | `built` |
| 4 | **Generate** | master template | tailored deliverable, ready to download | `deliverable ready` |
| 5 | **Audit** | live link + record | active? consistent? invented? | `verified` / `expired` |

## Caps & thresholds
- **Max N items/day.** Keep only items scoring ≥ threshold.
- Fewer than N good items → produce fewer. Never fill to fill.

## Learning loop
- Human sets `not interesting` + **reason** in a journal.
- Orchestrator summarizes the learned rule into memory (`learned preferences`).
- Sourcing re-reads that section each run → filters better.

## Human role
Agents stop at `deliverable ready`. Human reviews, adjusts, **acts**, then sets `done` + date.

## Why this pattern scales
- One failure = one agent re-runs, not the whole chain.
- The audit gate makes outputs trustworthy enough to act on.
- The memory loop means the system gets cheaper and better every day.
