## Agent Hub – Web Analysis Stack (Competitive Intelligence Engine v1)

### Overview
Agents below form the external surveillance mesh for MAOS. Each agent owns a discrete slice of the Competitive Intelligence Engine and publishes Contract-compliant payloads into Reflex → Teamwork → Client Portal pipelines.

### Assignment Matrix
| Agent | Primary Surface | Trigger Types | Core Responsibilities | Escalation |
| --- | --- | --- | --- | --- |
| `surveillance_agent` | Public websites, pricing pages, feature pages | Cron (hourly), manual override, webhook (pricing alert) | Snapshot DOM/content, detect SKU/pricing deltas, capture metadata fingerprints | Escalate to `content_diff_agent` if checksum drift > 2% |
| `research_agent` | Blogs, press releases, filings, analyst notes | RSS diff, newsroom API, manual brief request | Extract launches, partnerships, regulatory news; summarize market impact context | Escalate to `competitor_watch_agent` for multi-client relevance |
| `competitor_watch_agent` | Client-specific portfolios | Reflex hook when CDM updates or Research flags | Map deltas to Client Matrix, derive Threat/Opening rating, recommend impacted KPIs | Escalate to Reflex DAG if Threat ≥ 3 or Opening ≥ 3 |
| `web_crawler_agent` | Deep crawl of product docs, changelogs, help centers | Weekly crawl, ad-hoc | Versioning of technical docs, detect feature toggles, API changes | Escalate to `signal_harvester_agent` when API diff touches regulated data |
| `signal_harvester_agent` | Social, ad libs, SEO/PPC telemetry, sentiment feeds, LLM ecosystem | Streaming, webhook, keyword spike | Monitor campaign spend, targeting overlaps, sentiment inflection, algorithm reactions | Escalate to War-Room (Teamwork board) if sentiment delta > 25% negative within 6h |
| `content_diff_agent` | Output QA + normalization | Triggered by upstream diff flag | Run structured diff, de-duplicate noise, stamp confidence + custodian hash, hand off to Impact Scoring | Escalate to Governance custodian if confidence < 0.5 but severity high |

### Trigger Logic
- **Cadence Grid:** Each agent has default cron plus event-driven triggers (pricing webhook, RSS update, keyword spike). Schedules stored alongside CDM entry; override via Codex command.
- **Reflex Hooks:** When internal Action Matrix emits “Awaiting external validation,” Reflex can call `competitor_watch_agent` or `signal_harvester_agent` immediately.
- **Manual Overrides:** Custodians can issue `/maos agent run <agent> <client|competitor>` which drops a job onto the queue and tags Custodian Hash.

### Output Contract
All agents publish JSON payloads (delivered through message bus or shared datastore) with the following shape:
```json
{
  "agent_id": "surveillance_agent",
  "engine_version": "cie-v1",
  "client_id": "acme_saas",
  "competitor_id": "rival_cloud",
  "artifact": {
    "type": "pricing_page",
    "uri": "https://rival.example.com/pricing",
    "snapshot_id": "sha256:...",
    "diff_summary": "Pro plan +12%, added AI Assist tier"
  },
  "analysis": {
    "threat_score": 4,
    "opening_score": 1,
    "kpi_targets": ["ARR", "ACV"],
    "market_impact": "Mid",
    "strategic_implication": "Opportunity to upsell AI bundle before rival adoption stabilizes",
    "recommended_actions": [
      "Update competitive battlecard",
      "Launch targeted nurture to price-sensitive cohorts"
    ]
  },
  "governance": {
    "confidence": 0.82,
    "custodian_hash": "custodian:delta:2025-11-24T05:00Z",
    "sources": ["DOM_snapshot", "Wayback_cache"],
    "alert_level": "red",
    "notes": "Confirmed across 2 regions"
  },
  "routing": {
    "reflex_intent": "competitor_price_change",
    "teamwork_template": "TW-competitor-price-adjust",
    "portal_views": ["competitor_intel", "risk_levels"]
  },
  "timestamp": "2025-11-24T05:12:00Z"
}
```

### Confidence & Governance
- Confidence tiers: **A ≥ 0.9** (multi-source, verified), **B 0.7–0.89** (single-source with corroborating signals), **C < 0.7** (speculative – requires custodian review before automation).
- Every payload carries Custodian Hash (Custodian ID + ISO timestamp + agent). Stored in Codex and visible in Client Portal compliance tab.
- Governance notes require provenance lines (URL, capture method, credential scope). Agents auto-tag sensitivity levels for legal review.

### Routing Rules
- **Teamwork:** Each `routing.teamwork_template` maps to a Reflex automation that creates tasks with owner, SLA, severity tags. Example: `competitor_keyword_collision` → assign to Paid Media lead with 24h SLA.
- **Client Portal:** Portal widgets subscribe to filtered feeds (per-client). Red-level alerts display in Risk Levels + push to Heartbeats digest; Yellow-level stay in Trend Watch queue.
- **Strategy Sprints:** Accepted deltas spawn Sprint inputs; backlog items auto-link to the originating agent payload.

### Monitoring & Telemetry
- Agents emit health metrics (uptime, queue depth, scrape success) to Heartbeats.
- Failure auto-retry with exponential backoff; after 3 failures, alert Custodian + Ops.
- Diff noise threshold configurable per client; stored in CDM (see template).

### Next Actions
1. Load Competitor Definition Map with client → competitor mappings + monitoring assets.
2. Configure agent credentials + schedules following matrix above.
3. Stand up message bus topic `cie.v1.agent-output`.
4. Wire Reflex intents to Teamwork templates and Client Portal feeds.
