# Methodology

The operating system behind every pipeline. These rules are what make multi-agent work *reliable* rather than demoware.

## 1. Plan before execute
- Every run starts from a written plan: target, no-go rules, sources, thresholds.
- Agents do not improvise scope.

## 2. Atomic decomposition
- Break the work into the smallest units that can fail independently.
- Each agent does one thing and writes its output even if a field is missing — it marks the gap as `to verify` rather than guessing.

> Rule: **never invent.** Missing data is flagged, never fabricated.

## 3. Fresh-context self-review
- The final audit runs in a *clean* context (no prior conversation), checking:
  - is the item still live / valid?
  - is everything consistent?
  - is anything invented?
- Hallucinations introduced upstream are caught here.

## 4. Progressive disclosure
- Short controller files (< 200 lines) carry the strategy.
- Heavy detail lives in annexes, loaded only when needed.
- Keeps every agent's context lean and fast.

## 5. Capitalize
- Every deliverable becomes a reusable asset: template, prompt, workflow, skill.
- The system compounds instead of restarting each time.

## 6. Quantified options
- Decisions come with trade-offs and numbers.
- Example threshold rule: keep only items scoring ≥ X; cap at N per day; produce fewer rather than pad.

## The learning loop
- Human rejects an item **with a reason** → orchestrator summarizes the rule into project memory.
- Agents re-read that memory section every run → filter better over time.
- This is **heuristic memory learning**, not model fine-tuning.

## Human-in-the-loop
- Agents stop at `ready`.
- Human reviews, adjusts, acts, then marks `done` and logs the date.
- Judgment calls (fit, tone, risk) stay with the human — agents handle volume and structure.
