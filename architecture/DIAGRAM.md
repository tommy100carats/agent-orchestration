# System Diagram

```mermaid
flowchart TD
    HUMAN[Orchestrator — human architect] -->|design · route · validate| PLAN[Written plan + no-go rules + thresholds]

    PLAN --> S1[Sourcing agent]
    PLAN --> S2[Enrichment agent]
    PLAN --> S3[Build agent]
    PLAN --> S4[Generate agent]
    PLAN --> S5[Audit agent]

    S1 -->|search · score · dedupe| STATE[(Per-item record<br/>typed fields)]
    S2 -->|decision-maker<br/>public profile| STATE
    S3 -->|structured content| STATE
    S4 -->|tailored deliverable| STATE
    S5 -->|verify active · consistent · real| STATE

    STATE --> GATE{Human-in-the-loop gate}
    GATE -->|review · adjust · act| DONE([Done + logged])

    MEM[Project memory<br/>learned preferences] -.->|re-read each run| S1

    classDef human fill:#2563EB,stroke:#1e40af,color:#fff;
    classDef agent fill:#1E293B,stroke:#334155,color:#F1F5F9;
    classDef state fill:#0B0F1A,stroke:#3B82F6,color:#F1F5F9;
    class HUMAN,GATE human;
    class S1,S2,S3,S4,S5 agent;
    class STATE,PLAN,MEM state;
```

## Execution layer (unattended automation)

The methodology also drives fully automated workflows (no human at runtime):

```mermaid
flowchart LR
    T[Schedule 08:00] --> C[Pick city]
    C --> G[Source API]
    G --> F[Format + enrich]
    F --> D{Dedupe vs CRM}
    D -->|new| W[Write to CRM]
```

See [`workflows/lead-intake-n8n.json`](../workflows/lead-intake-n8n.json) for a runnable example.
