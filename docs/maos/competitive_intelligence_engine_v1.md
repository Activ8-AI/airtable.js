## Competitive Intelligence Engine (v1)

### Purpose
Give MAOS persistent external awareness so internal signals (Reflex DAG → Teamwork → Action Matrix) are automatically balanced against competitor and industry movement. The engine continuously collects, diffs, scores, and routes competitive deltas into the Charter-governed systems (Client Portal, Codex, Teamwork, Heartbeats, KPI mapping, Strategy Sprints, Reflex responses).

### Capability Stack
- **Surveillance coverage:** websites, pricing pages, blogs, ad libraries, feature pages, help centers, press feeds, changelogs, social channels, SEO/PPC telemetry, sentiment streams, LLM ecosystem updates.
- **Comparative analytics:** evaluates threats, openings, positioning shifts, value gaps, category moves per client.
- **Actionable deltas:** outputs specific Reflex-ready tasks (no passive summaries).
- **Charter write-ups:** automated briefs include Summary, Market Impact, Strategic Implication, Recommended Actions, Governance Notes, Confidence Score, Custodian Hash.
- **Portal visualization:** feeds Competitor Intelligence tab, Industry Radar, Trend Watch, Risk Levels, Action Recommendations, Revenue Impact models.

### Data Flow (Critical Path)
1. **Target Resolution:** Competitor Definition Map (CDM) resolves which entities, assets, keywords, and campaign handles belong to each client portfolio.
2. **Collection:** Web Analysis Agents (surveillance_agent, research_agent, competitor_watch_agent, web_crawler_agent, signal_harvester_agent, content_diff_agent) harvest raw artifacts on cadence/trigger.
3. **Normalization:** Content is fingerprinted, diffed, enriched with metadata (source, timestamp, confidence, relevance, sensitivity).
4. **Delta Detection:** Compare against prior baseline + client matrices to flag material change (pricing delta, SKU launch, targeting overlap, messaging shift, policy change, etc.).
5. **Impact Scoring:** Quantify Market Impact, Revenue/KPI exposure, Urgency, Confidence. Stamp Custodian Hash for audit, attach Governance hooks.
6. **Action Routing:** 
   - Reflex: create Competitor Delta intents (e.g., “Competitor raised prices 12% on Core SKU – adjust positioning playbook”).
   - Teamwork: auto-generate tasks with owners, SLAs, linkage to Strategy Sprint or Action Matrix lane.
   - Codex: persist write-up, cross-link to Charter section + custodian.
   - Client Portal: update Competitor tab widgets, Trend watchlines, alert badges (red/yellow) and push to Heartbeats.
7. **Feedback Loop:** Execution outcomes + Heartbeat telemetry push back into CDM to refine monitoring scope and prioritization weights.

### Integration Points
- **Client Portal:** Embed new `Competitor Intelligence`, `Industry Radar`, `Trend Watch`, `Risk Levels`, `Action Recommendations`, `Revenue Impact` views backed by engine outputs (GraphQL/API feed or shared datastore).
- **Codex:** Store every brief with Governance Notes + Confidence Score + Custodian Hash; expose queries for auditors and Strategy leads.
- **Teamwork:** Use Reflex → Task automation to open/close loops (“Competitor targeting Client Keyword Set — TAKE ACTION”). Includes SLA tagging and escalation logic.
- **Heartbeats:** Surface major deltas in leadership daily digest; heartbeat severity ties to Risk Levels.
- **KPI ↔ Revenue Mapping:** Link impact scoring to KPI models so finance/sales sees projected revenue exposure/opportunity per delta.
- **Strategy Sprints:** Each sprint retro references the latest competitor deltas; new plays seeded directly from prioritized write-ups.
- **Reflex DAG:** Add Competitor Delta node family so internal triggers can be suppressed/accelerated based on external context (e.g., hold price test if competitor dumped price).

### Charter Governance Hooks
- Custodian assignments per client vertical with rotation policy.
- Confidence scoring rubric (A: verified multi-source, B: single-source corroborated, C: speculative).
- Red/Yellow alert thresholds (Red = revenue impact ≥ 10% or regulatory exposure; Yellow = directional change needing watch).
- Mandatory Governance notes capturing data provenance + decision trace.
- Audit log (Custodian Hash + timestamp + agent pipeline version) stored in Codex + mirrored to Client Portal compliance tab.

### Implementation Plan (v1)
1. **Week 0-1 – Foundation**
   - Stand up Competitor Definition Map (CDM) populated from Client Matrix + onboarding + CRM.
   - Register monitoring targets per competitor (URLs, feeds, keywords, ad accounts, social handles).
   - Configure agent credentials, rate limits, and schedule grid.
2. **Week 1-2 – Agent Wiring**
   - Implement assignment/trigger logic described in `agents.md`.
   - Pipe normalized payloads into storage (e.g., vector store + relational change log).
   - Stand up diffing and impact-scoring microservice with Governance hooks.
3. **Week 2-3 – Reflex + Teamwork Integration**
   - Define Competitor Delta schema (action verb, client, severity, KPI link).
   - Connect Reflex DAG to open Teamwork tasks automatically with templated descriptions and owners.
4. **Week 3-4 – Client Portal Surfaces**
   - Build Competitor Intelligence tab modules (Delta feed, Radar heatmap, Trend watchlines, Risk dial, Actions, Revenue model).
   - Wire alerts into Heartbeats + KPI dashboards.
5. **Week 4+ – Optimization**
   - Add LLM ecosystem signal ingestion.
   - Expand to predictive modeling (detect probable upcoming launches).
   - Automate Sprint seeding and KPI auto-adjustments.

### Deliverables Created in Codebase
- `docs/maos/competitor_definition_map_template.md` – schema + workflow.
- `agents.md` – Agent Hub assignments, triggers, outputs, governance.
Use these as living guides while wiring the engine into MAOS.
