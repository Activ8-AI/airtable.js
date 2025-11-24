## Competitor Definition Map (CDM) Template

The CDM is the source of truth linking every client to its competitors, monitored assets, alert thresholds, and governance metadata. Store the authoritative version in Notion (shared with Custodians) and mirror the structure in Codex for automation.

### Table Schema
| Field | Description |
| --- | --- |
| `client_id` | Codex ID / Client Matrix reference (e.g., `acme_saas`). |
| `client_vertical` | Industry / sub-vertical for routing to correct custodians. |
| `competitor_id` | Normalized slug (e.g., `rival_cloud`). |
| `competitor_aliases` | Comma-separated brand/product aliases for matching. |
| `priority_tier` | P0 (direct), P1 (adjacent), P2 (emerging). Drives cadence. |
| `segments_at_risk` | ICP slices or product lines exposed by this competitor. |
| `monitoring_assets` | Structured list of URLs/feeds/APIs with labels (`pricing_page`, `blog_rss`, `status_feed`, `ad_library`, `seo_keyword_api`). |
| `keywords` | Paid/organic keyword clusters to watch for targeting collisions. |
| `ad_accounts` | Platform + account IDs (Meta, Google Ads, LinkedIn) for spend monitoring. |
| `social_handles` | Handles/IDs for social/sentiment tracking. |
| `llm_signal_channels` | Forums, model cards, changelogs indicating LLM ecosystem shifts. |
| `pricing_model` | Notes on pricing architecture (seat-based, usage, hybrid). |
| `differentiators` | Key positioning claims to evaluate for value-gap analysis. |
| `alert_thresholds` | JSON blob: `{ "pricing_delta_pct": 5, "sentiment_drop": 0.25, "sku_addition": true }`. |
| `noise_tolerance` | % change before alerts fire (per agent). |
| `kpi_links` | ARR, ACV, churn, CAC, pipeline metrics affected. |
| `custodian_owner` | Primary + backup custodian responsible for approvals. |
| `governance_notes` | Compliance constraints, legal watch-outs, data handling notes. |
| `last_reviewed` | ISO timestamp for audit rotation. |

### Workflow
1. **Intake:** Populate CDM during client onboarding using Client Matrix, CRM, and existing briefs.
2. **Approval:** Custodian signs off on monitoring scope and alert thresholds; Codex logs Custodian Hash.
3. **Sync:** Automated job exports Notion table to Codex + data store powering agents.
4. **Versioning:** Every edit increments schema version; Reflex notified to refresh agent configs.
5. **Audit:** Monthly review ensures competitors, assets, and thresholds still valid; attach Governance note.

### Integration Hooks
- **Agents:** Each agent reads `monitoring_assets`, `keywords`, `noise_tolerance`, and `alert_thresholds` to configure cadence + triggers.
- **Reflex DAG:** Uses `priority_tier` + `kpi_links` to score task urgency and choose automation lane.
- **Teamwork:** Template selection uses `segments_at_risk` and `kpi_links` to pre-fill owners/SLAs.
- **Client Portal:** 
  - `Competitor Intelligence` tab lists all competitors from CDM with risk color derived from most recent deltas.
  - `Industry Radar` heatmap groups by `client_vertical` and `priority_tier`.
  - `Trend Watch` pulls keywords + LLM signals to show live coverage map.
  - `Risk Levels` card references `alert_thresholds` vs actual deltas to set red/yellow state.
  - `Revenue Impact` module ties `kpi_links` to financial exposure models.
- **Codex:** Mirror CDM table to maintain Charter-compliant audit with diff history and Custodian approval fields.

### Implementation Notes
- Store the machine-readable copy as JSON or YAML alongside Codex entries so automation can load it without scraping Notion.
- Use environment-specific overrides (e.g., sandbox credentials) but keep production CDM canonical.
- When new competitor detected by agents but absent in CDM, automatically open a Teamwork task “Add competitor to CDM” assigned to Custodian.
- Attach `custodian_hash` + `last_reviewed` to every export to satisfy Charter governance.
