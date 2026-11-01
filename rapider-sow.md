<!-- Rapider Official App SOW & Specification | App #096: Business Intelligence & Dashboards (bi-dashboards) | Domain: DAT -->

# #096 Business Intelligence & Dashboards (`bi-dashboards`) — Scope of Work (SOW)

- **App Number & Slug:** `#096` / `096-business-intelligence-and-dashboards`
- **Proposed Rapider App Key:** `bi-dashboards`
- **Suite / Domain Key:** `DAT` (Data, Analytics & Business Intelligence Suite — `Dat*` Canonical Prefix)
- **Representative Product (`P1`):** Microsoft Power BI (Microsoft Fabric)
- **Combined Feature Catalog Reference:** [../combined-features/096-business-intelligence-and-dashboards-features.md](../combined-features/096-business-intelligence-and-dashboards-features.md)

---

## 1. Executive Outcome & Target Audience

`bi-dashboards` delivers a single, governed Business Intelligence, Semantic Metrics, and Executive Dashboarding studio natively integrated across the Rapider suite ecosystem. Organizations routinely suffer from "spreadsheet metric anarchy," where Sales, Finance, Operations, and Support teams compute conflicting numbers for ARR, Gross Margin, Net Retention, and SLA Attainment using ungoverned SQL snippets and disconnected CSV exports. `bi-dashboards` eliminates metric drift by combining a versioned semantic and metrics layer (`DatSemanticModel`, `DatMetricDefinition`) with a high-velocity interactive visual canvas (`DatDashboard`, `DatDashboardTile`), self-service no-code exploration (`DatSavedExploration`), row-level security (`DatRowSecurityPolicy`), and threshold alerting (`DatMetricAlert`).

- **SME Fast-Path Defaults:** Small and mid-market teams can activate turnkey cross-suite starter packs (Sales Pipeline, Cash & P&L, Headcount & Attrition, Order Fulfillment, Support SLA) in under five minutes with zero SQL authoring, pre-wired to canonical Rapider collections (`Crm*`, `Fin*`, `Hr*`, `Ops*`, `Sup*`).
- **Enterprise Progressive Disclosure:** Data engineering and analytics teams unlock multi-fact semantic model versioning, `DRAFT -> PROMOTED -> CERTIFIED` steward workflows via Rapider Home `WorkItem`s, dynamic Row/Column-Level Security (`DatRowSecurityPolicy`), multi-currency fiscal normalization against `FinLegalEntity`, RLS-aware report bursting (`DatReportSubscription`), and signed embedded analytics (`DatEmbedConfig`).
- **Measurable Success Outcomes:**
  - 100% single-source definition reuse for certified KPIs across executive scorecards, operational dashboards, and conversational AI queries.
  - Sub-1.5s interactive cross-filtering and drill-down latency on cached semantic snapshots using non-blocking `findSWR`/`countSWR` reads.
  - Zero unauthorized row or sensitive-column leakage across live queries, scheduled PDF/CSV subscriptions, and embedded widgets.
- **Top 6 Global Benchmarks Synthesized:** `P1` Microsoft Power BI (Fabric), `P2` Salesforce Tableau, `P3` Google Looker, `P4` Qlik Cloud Analytics, `P5` SAP Analytics Cloud, and `P6` Zoho Analytics.

---

## 2. In-Scope, Out-of-Scope & Suite Ownership Boundaries

### In-Scope
1. Governed semantic models (`DatSemanticModel`) declaring entity relationships, join cardinalities, primary grains, and `LIVE` vs. `CACHED_SNAPSHOT` execution strategies.
2. Canonical calculated measures and KPI scorecards (`DatMetricDefinition`) with time-intelligence (YoY, QoQ, MoM, trailing windows), target polarity, and steward certification states.
3. Multi-tile responsive dashboard builder (`DatDashboard`, `DatDashboardTile`) with global filter bars, `#activeFilterTagsBar` chips, cross-highlighting, what-if sliders, hierarchical drill-downs, and deep-link record drill-through.
4. No-code self-service visual exploration workbench (`DatSavedExploration`) with pivot matrices, Top-N bucketing, personal filter bookmarks, and variance driver attribution.
5. Row-Level and Column-Level Security policies (`DatRowSecurityPolicy`), scheduled RLS-aware digest subscriptions (`DatReportSubscription`), threshold alerts (`DatMetricAlert`), and signed embedded widget configurations (`DatEmbedConfig`).
6. Grounded conversational BI ("Ask BI") strictly constrained to certified `DatSemanticModel` and `DatMetricDefinition` definitions via platform `projectChannelMessage` threads.

### Out-of-Scope
1. Event-stream clickstream SDK ingestion, session replay, and mobile funnel instrumentation — owned by `#097 Product Analytics` (`product-analytics`).
2. External SaaS/CDC ingestion connectors, ELT sync orchestration, and raw replication pipelines — owned by `#098 Data Integration / ETL` (`data-integration`).
3. Cloud SQL compute cluster provisioning, physical warehouse storage partitioning, and multi-statement SQL IDE worksheets — owned by `#099 Data Warehouse & Analytics Platform` (`data-warehouse`).
4. Enterprise-wide PII classification scanning, cross-system technical lineage graphs, and master glossary ownership — owned by `#100 Data Governance & Catalog` (`data-governance`).
5. Financial general-ledger journal posting or statutory accounting close — owned by `FIN` (`acc`, `budgeting`).

### Cross-Suite Ownership & Integration Contract

| Domain / App | Owns (Single Source of Truth) | Integration Contract with `bi-dashboards` (`#096`) |
| :--- | :--- | :--- |
| **`bi-dashboards` (`#096`)** | `DatSemanticModel`, `DatMetricDefinition`, `DatDashboard`, `DatDashboardTile`, `DatSavedExploration`, `DatMetricAlert`, `DatReportSubscription`, `DatRowSecurityPolicy`, `DatEmbedConfig` | Publishes certified KPI snapshots, alert breach events, and embed tokens to sibling Rapider apps. |
| **`#099` Data Warehouse (`data-warehouse`)** | `DatWarehouseTable`, `DatComputeCluster`, `DatDatabaseCatalog` | `DatSemanticModel` binds to governed `DatWarehouseTable` views/tables for analytical SQL pushdown without duplicating physical storage metadata. |
| **`#100` Data Governance (`data-governance`)** | `DatBusinessGlossaryTerm`, `DatCatalogAsset`, `DatClassificationTag` | `DatMetricDefinition` links to `DatBusinessGlossaryTerm`; column masking honors PII/confidential `DatClassificationTag` policies. |
| **`CRM`, `FIN`, `HR`, `OPS`, `SUP`, `APM` Suites** | Canonical operational entities (`CrmOpportunity`, `FinInvoice`, `FinBudget`, `FinLegalEntity`, `HrEmployee`, `HrDepartment`, `OpsCustomerOrder`, `SupTicket`, `ApmProject`) | Read-only analytical aggregation and record-level drill-through navigation (`window.rapiderApi.navigate`); never mutates operational suite records directly. |
| **Rapider Platform Services** | Identity/RBAC, Multi-tenant Workspace Context, Home `WorkItem`s, `projectChannelMessage`, Files/Drive, Kafka/OpenWhisk | Routes certification reviews and KPI breach tasks to Home `WorkItem`s; stores dashboard discussions and AI sessions in `projectChannelMessage`; exports bursts to Files/Drive. |

---

## 3. Primary Web Pages & Sidebar Navigation Architecture (12 Pages)

Every visible sidebar section strictly enforces the **Minimum 3 Items Rule (`>= 3` visible pages per section)**, while detail, create, and edit drawers/routes set `displayOnMenu: false`.

| # | Route | Page Title | Sidebar Section | Menu Visible | Key Components & UX |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `bi-home` | Executive Analytics Hub | `Analytics` | `true` (Order 1) | Pinned KPI scorecard ribbon, recently viewed dashboards, turnkey starter pack activator, `#activeFilterTagsBar`, Ask BI drawer launcher. |
| 2 | `dashboards` | Dashboards Directory | `Analytics` | `true` (Order 2) | Bulk `findSWR` catalog of `DatDashboard`s with collection/certification filter pills, `#activeFilterTagsBar` + `Clear all`, clickable dashboard title link, icon+text `Edit`, icon-only `Delete`. |
| 3 | `kpi-scorecards` | KPI Scorecards & Goals | `Analytics` | `true` (Order 3) | Governed `DatMetricDefinition` watchlist with `Decimal128` Actual vs. Target cards, sparklines, `ON_TRACK / AT_RISK / OFF_TRACK` pills, and variance drill-down. |
| 4 | `explore-workbench` | Self-Service Explore | `Modeling & Studio` | `true` (Order 4) | Interactive drag-and-drop dimension/measure query builder (`DatSavedExploration`), chart type switcher, pivot matrix with heat-map shading, and `Pin to Dashboard` action. |
| 5 | `semantic-models` | Semantic Models | `Modeling & Studio` | `true` (Order 5) | Catalog of `DatSemanticModel`s showing source suite/warehouse bindings, execution mode (`LIVE` / `CACHED_SNAPSHOT`), certification badge, and cache refresh trigger. |
| 6 | `metrics-library` | Certified Metrics Library | `Modeling & Studio` | `true` (Order 6) | Searchable `DatMetricDefinition` registry with formula preview, downstream dashboard impact count, `#100` glossary link, and `Request Certification` workflow trigger. |
| 7 | `metric-alerts` | Alerts & Thresholds | `Governance & Delivery` | `true` (Order 7) | `DatMetricAlert` management table with condition badges, breach history, cooldown status, and linked Rapider Home `WorkItem` references. |
| 8 | `report-subscriptions` | Scheduled Subscriptions | `Governance & Delivery` | `true` (Order 8) | `DatReportSubscription` schedule manager (cron cadence, PDF/CSV format, RLS-enforced recipient groups, last run status). |
| 9 | `rls-embed-policies` | Security & Embed Policies | `Governance & Delivery` | `true` (Order 9) | Unified governance view for `DatRowSecurityPolicy` rules (with "Test as Role" simulator) and `DatEmbedConfig` signed widget tokens. |
| 10 | `dashboard-view` | Interactive Dashboard Canvas | *(Hidden)* | `false` | Full responsive multi-tile canvas (`#/{dashboard-view}?id=...`), global date/dimension filter bar, what-if sliders, drill-through drawer, `projectChannelMessage` comments, and `Back to Dashboards` state-preserving link. |
| 11 | `semantic-model-detail` | Semantic Model Designer | *(Hidden)* | `false` | Entity relationship join graph, primary grain validator, calculated measure editor, version history diff, and impact lineage inspector. |
| 12 | `dashboard-tile-editor` | Dashboard & Tile Drawer | *(Hidden)* | `false` | Sliding creation/edit drawer for `DatDashboard` metadata, `DatDashboardTile` binding, and `DatMetricAlert` rules; auto-closes via `closePageDrawer()` on save/cancel with `try/catch/finally` button reset. |

**Mandatory Rapider UI Standards Enforced Across All Pages:**
- **SWR First:** All initial page mounts use `window.rapiderApi.findSWR` and `countSWR` without redundant client `isDeleted` predicates, paired with defensive in-memory filtering (`items = rawData.filter(it => it && it.isDeleted !== true)`).
- **Table Actions & Navigation:** Zero standalone `"View"` buttons in `Actions` columns; primary record names (`title`, `name`, `metricCode`) render as `font-semibold text-primary hover:underline cursor-pointer` navigating to detail views; `Edit` uses `<i class="fas fa-edit mr-1"></i><span>Edit</span>`; `Delete` uses icon-only `<i class="fas fa-trash-alt"></i>` with custom modal confirmation (`#rapiderConfirmOkBtn`) and zero `window.alert`/`confirm`/`prompt`.
- **Contrast & Active Filter Bar:** Primary and destructive buttons strictly enforce `!text-white text-white` with `style="color: #ffffff !important;"`. Every searchable/filterable view renders `#activeFilterTagsBar` with HTML-escaped dismissible chips and `[Clear all]`.

---

## 4. Canonical Data Models (`Dat*` — 8 Models)

All models omit `projectId` (enforced by platform host tenancy), use platform UUID `id`, track optimistic concurrency `version`, and use `00000000-xxxx-xxxx-xxxx-xxxxxxxxxxxx` UUIDs for seed records.

| Model Name | Key Fields & Types | Relationships & External Refs | Invariants & Lifecycle |
| :--- | :--- | :--- | :--- |
| **`DatSemanticModel`** | `id: UUID`, `code: string`, `name: string`, `sourceType: enum(RAPIDER_SUITE, WAREHOUSE_TABLE, BLENDED)`, `entitiesJson: object[]`, `joinsJson: object[]`, `primaryGrain: string`, `executionMode: enum(LIVE, CACHED_SNAPSHOT)`, `refreshCron: string?`, `lastRefreshedAt: datetime?`, `certificationStatus: enum(DRAFT, PROMOTED, CERTIFIED, ARCHIVED)`, `certifiedByUserId: UUID?`, `version: int` | Optional refs to `#099` `DatWarehouseTable.id` and `#100` `DatCatalogAsset.id`; owned by Author `ownerUserId`. | `code` unique per workspace. Every join must declare explicit cardinality (`ONE_TO_ONE`, `MANY_TO_ONE`) to block fan-out double counting. Transition to `CERTIFIED` requires Steward role via guarded command. |
| **`DatMetricDefinition`** | `id: UUID`, `semanticModelId: UUID`, `metricCode: string`, `name: string`, `formulaExpression: string`, `aggregationType: enum(SUM, AVG, COUNT_DISTINCT, RATIO, WINDOW_DELTA)`, `unitType: enum(CURRENCY, PERCENT, INTEGER, DECIMAL, DURATION_MS)`, `currencyCode: string?`, `targetValue: Decimal128?`, `polarity: enum(HIGHER_IS_BETTER, LOWER_IS_BETTER)`, `certificationStatus: enum(DRAFT, PROMOTED, CERTIFIED, DEPRECATED)`, `glossaryTermId: UUID?`, `version: int` | `semanticModelId -> DatSemanticModel.id`; optional `glossaryTermId -> DatBusinessGlossaryTerm.id` (`#100`); optional `legalEntityId -> FinLegalEntity.id`. | Monetary `targetValue` stored as `Decimal128`. Formula edits increment `version` and record an immutable formula audit snapshot so historical scorecard variance remains reproducible. |
| **`DatDashboard`** | `id: UUID`, `slug: string`, `title: string`, `collectionName: string`, `description: string`, `globalFiltersSchemaJson: object[]`, `whatIfParametersJson: object[]`, `certificationStatus: enum(DRAFT, PROMOTED, CERTIFIED, ARCHIVED)`, `starterPackKey: string?`, `viewCount90d: int`, `discussionChannelId: UUID?`, `ownerUserId: UUID`, `version: int` | `discussionChannelId -> projectChannelMessage` thread; contains `1..N` `DatDashboardTile` records. | Deleting a dashboard soft-deletes child tiles and disables active `DatReportSubscription`s atomically. |
| **`DatDashboardTile`** | `id: UUID`, `dashboardId: UUID`, `semanticModelId: UUID`, `metricIds: UUID[]`, `title: string`, `visualType: enum(KPI_CARD, LINE, AREA, BAR_STACKED, BAR_GROUPED, COMBO, WATERFALL, FUNNEL, PIVOT_MATRIX, SCATTER, GEO_MAP)`, `dimensionsJson: string[]`, `tileFiltersJson: object[]`, `drillThroughRoute: string?`, `gridPositionJson: {x, y, w, h}`, `version: int` | `dashboardId -> DatDashboard.id`; `semanticModelId -> DatSemanticModel.id`; `metricIds -> DatMetricDefinition.id[]`. | Every `metricIds` entry must belong to `semanticModelId` or a shared conformed dimension bridge. `gridPositionJson` coordinates must not overlap on desktop (`12-col`) grid. |
| **`DatSavedExploration`** | `id: UUID`, `semanticModelId: UUID`, `name: string`, `querySpecJson: object`, `visualType: string`, `bookmarkFiltersJson: object`, `visibility: enum(PRIVATE, SHARED_WORKSPACE)`, `ownerUserId: UUID`, `discussionChannelId: UUID?`, `version: int` | `semanticModelId -> DatSemanticModel.id`; optional `pinnedDashboardId -> DatDashboard.id`. | Private explorations are readable only by `ownerUserId` until explicitly shared; query execution always enforces `DatRowSecurityPolicy` dynamically at run time. |
| **`DatRowSecurityPolicy`** | `id: UUID`, `semanticModelId: UUID`, `policyName: string`, `subjectRoleKeys: string[]`, `rowFilterPredicateJson: object`, `maskedColumns: string[]`, `isEnabled: boolean`, `version: int` | `semanticModelId -> DatSemanticModel.id`; binds dynamic attributes (`currentUser.id`, `HrDepartment.id`, `CrmTerritory.id`). | Default-deny when RLS is active on a `DatSemanticModel` and caller matches zero enabled policies (unless granted explicit `BI_ADMIN_BYPASS_RLS` audit-logged permission). |
| **`DatMetricAlert`** | `id: UUID`, `metricId: UUID`, `name: string`, `operator: enum(ABOVE, BELOW, PCT_INCREASE_ABOVE, PCT_DECREASE_ABOVE)`, `thresholdValue: Decimal128`, `evaluationCron: string`, `cooldownMinutes: int`, `severity: enum(INFO, WARNING, CRITICAL)`, `lastEvaluatedAt: datetime?`, `lastTriggeredAt: datetime?`, `activeWorkItemId: UUID?`, `isEnabled: boolean`, `version: int` | `metricId -> DatMetricDefinition.id`; `activeWorkItemId -> Platform WorkItem.id`. | `thresholdValue` uses `Decimal128`. Duplicate breaches inside `cooldownMinutes` update existing `WorkItem` context rather than spamming duplicate tasks. |
| **`DatReportSubscription`** | `id: UUID`, `dashboardId: UUID`, `name: string`, `scheduleCron: string`, `exportFormat: enum(PDF, CSV_ZIP, DIGEST_LINK)`, `recipientUserIds: UUID[]`, `presetFiltersJson: object`, `isEnabled: boolean`, `lastDeliveredAt: datetime?`, `lastDeliveryFileId: UUID?`, `version: int` | `dashboardId -> DatDashboard.id`; `lastDeliveryFileId -> Platform Drive File.id`. | Snapshot rendering MUST evaluate `DatRowSecurityPolicy` per individual recipient (`recipientUserIds`) — never broadcast an unrestricted admin snapshot to restricted viewers. |

*(Note: Embedded widget configurations are stored as governed `DatEmbedConfig` documents or scoped tokens managed via `rls-embed-policies` with allowed origin allowlists, dashboard/tile scope, and mandatory TTL expiration <= 24 hours per signed token).*

---

## 5. Core End-to-End User Journeys (6 Journeys)

1. **Journey 1 — SME 5-Minute Executive Starter Pack Activation:**
   - An Operations Director opens `bi-home` (`Executive Analytics Hub`), clicks `"Activate Sales & Cashflow Starter Pack"`, and previews pre-wired models over `CrmOpportunity` and `FinInvoice`.
   - The guarded command `activateStarterPack` provisions certified-ready `DatSemanticModel`, six `DatMetricDefinition`s (`Pipeline Coverage`, `Win Rate`, `Net Invoiced Revenue`, `DSO`, `Overdue AR`, `Gross Cash Inflow`), and a `DatDashboard`.
   - The user lands directly on `#/dashboard-view?id=...`, applies a `"This Quarter"` filter pill (`#activeFilterTagsBar` renders `Date Range: "This Quarter" ✕`), and inspects live KPIs without writing a single line of SQL.
2. **Journey 2 — Semantic Modeling & Steward Certification via Home `WorkItem`:**
   - A BI Analyst creates a `DatSemanticModel` (`Net Revenue Retention Model`) joining `CrmAccount`, `FinSubscription`, and `FinInvoice` on `semantic-model-detail`, authors `NRR %` in `metrics-library`, and clicks `"Submit for Certification"`.
   - The system transitions status to `PROMOTED` and dispatches a Rapider Home `WorkItem` assigned to the Data Steward role.
   - The Data Steward clicks deep into `semantic-model-detail` from the Home `WorkItem`, verifies grain and `#100` `DatBusinessGlossaryTerm` alignment, and approves certification (`CERTIFIED`), illuminating the verified shield badge across every consuming dashboard.
3. **Journey 3 — Interactive Cross-Filtering, Drill-Through to Source Suite & Back-Navigation Restoration:**
   - On `dashboards`, a Regional VP filters by `Collection: "Revenue"` (`#activeFilterTagsBar` shows `Collection: "Revenue" ✕`), then physically clicks the primary link `"Global Enterprise Pipeline"` to navigate to `#/dashboard-view?id=...`.
   - Clicking the `"EMEA"` bar on the Regional Revenue Waterfall cross-filters all sibling tiles and updates `#activeFilterTagsBar` (`Region: "EMEA" ✕`).
   - Clicking drill-through on an at-risk deal row opens the canonical `CRM` `CrmOpportunity` detail route (`window.rapiderApi.navigate`), and clicking `"Back to Dashboards"` restores `dashboards` with `Collection: "Revenue"` intact.
4. **Journey 4 — KPI Threshold Breach to Actionable Home `WorkItem`:**
   - A Support Director configures a `DatMetricAlert` on `SLA Breach Rate %` (`ABOVE 5.00%`, `CRITICAL`, `cooldownMinutes: 240`) from `metric-alerts`.
   - Upon scheduled evaluation, `SLA Breach Rate %` reaches `6.40%`. The alert workflow emits `dat.bi.metric_alert.triggered`, creates a high-priority Rapider Home `WorkItem` linked to the filtered `#/dashboard-view`, and suppresses duplicate alerts during the 4-hour cooldown window.
5. **Journey 5 — Self-Service Exploration & Pinning to Dashboard:**
   - A Financial Analyst opens `explore-workbench`, selects `Cash & AP Aging Model`, groups `FinSupplierBill` by `Department` (`HrDepartment`) and `Aging Bucket`, applies conditional heat-map shading, and clicks `"Pin to Dashboard"`.
   - The sliding `dashboard-tile-editor` drawer opens, binds the new `PIVOT_MATRIX` `DatDashboardTile` to `"Controller Weekly Cockpit"`, invalidates `DatDashboard` SWR cache on save, auto-closes the drawer via `closePageDrawer()`, and immediately renders the newly pinned tile.
6. **Journey 6 — Grounded Conversational BI ("Ask BI") with Citation & RLS Enforcement:**
   - A Territory Manager opens the `"Ask BI"` side thread (`projectChannelMessage`) on `bi-home` and asks: *"Why did Q3 Net Bookings drop compared to Q2?"*
   - The AI assistant resolves `Net Bookings` strictly against the certified `DatMetricDefinition`, executes the variance driver decomposition (`BID-027`) constrained by the caller's `EMEA-North` `DatRowSecurityPolicy`, and posts an interactive chart + cited explanation referencing `DatMetricDefinition` `MTR-BOOKINGS-NET` without exposing `AMER` or `APAC` rows.

---

## 6. Consolidated Feature Priorities & Work Packages (`P0`, `P1`, `P2`)

### `P0` Foundation Work Packages (Must-Have for Initial Release)
- **`WP-P0-1` Semantic & Metrics Core (`BID-001`, `BID-002`, `BID-003`):** Build `semantic-models`, `semantic-model-detail`, and `metrics-library` supporting entity-join validation, `Decimal128` measure formulas, and `LIVE` vs. `CACHED_SNAPSHOT` execution modes (`BI Author` / `Data Engineer`).
- **`WP-P0-2` Interactive Dashboard Canvas & Starter Packs (`BID-007`, `BID-008`, `BID-009`, `BID-010`):** Build `bi-home`, `dashboards`, `dashboard-view`, and `dashboard-tile-editor` with 11 core chart types, `#activeFilterTagsBar`, hierarchical drill-down, cross-suite drill-through, and 5 turnkey SME starter packs (`All Roles`).
- **`WP-P0-3` Governed Explore Workbench & Pivot Matrix (`BID-014`, `BID-015`):** Build `explore-workbench` for no-code slice-and-dice, heat-map pivot matrices, and one-click dashboard pinning (`Explorer` / `BI Author`).
- **`WP-P0-4` Scorecards, Alerts, Subscriptions & RLS (`BID-018`, `BID-019`, `BID-020`, `BID-023`):** Build `kpi-scorecards`, `metric-alerts`, `report-subscriptions`, and `rls-embed-policies` enforcing dynamic user/department/territory RLS across live queries and scheduled exports (`Data Steward` / `Executive`).

### `P1` Scale & Enterprise Governance Work Packages
- **`WP-P1-1` Steward Certification, Fiscal Multi-Currency & Versioning (`BID-004`, `BID-005`, `BID-006`):** Home `WorkItem` certification flows, `FinLegalEntity` currency/fiscal calendar alignment, and downstream impact lineage checks before model edits (`Data Steward`).
- **`WP-P1-2` What-If Scenarios, Bookmarks, IBCS Variance & Tile Threads (`BID-011`, `BID-012`, `BID-013`, `BID-016`, `BID-021`):** Parameter sliders, personal/shared filter bookmarks, Actual-vs-Budget (`FinBudget`) variance waterfalls, Top-N bucketing, and `projectChannelMessage` chart annotations (`Analyst` / `Executive`).
- **`WP-P1-3` Collections Governance & Signed Embedded SDK (`BID-024`, `BID-025`):** Granular collection permissions and short-lived signed embed tokens for embedding KPIs in sibling Rapider apps and external portals (`BI Admin`).

### `P2` Adjacent & Grounded AI Work Packages
- **`WP-P2-1` Grounded "Ask BI" & Key-Driver Variance Decomposition (`BID-026`, `BID-027`):** Certified-metric-grounded NLQ over `projectChannelMessage` and automated statistical variance driver attribution (`All Permitted Viewers`).
- **`WP-P2-2` Associative Excluded-Value Inspector, Alert Workflows & Telemetry (`BID-017`, `BID-022`, `BID-028`):** Unmatched dimension member discovery, OpenWhisk/Kafka workflow triggers on KPI breach, and 90-day stale dashboard hygiene analytics (`BI Author` / `Data Steward`).

---

## 7. Roles, Permissions & Field-Level Governance

| Role | Permitted Actions | Restricted / Field-Level Boundaries |
| :--- | :--- | :--- |
| **`BI Viewer`** | View shared `DatDashboard`s and `kpi-scorecards`, apply interactive filters/bookmarks, participate in `projectChannelMessage` tile comments, subscribe to permitted digests. | Strictly bound by `DatRowSecurityPolicy` row filters and `maskedColumns`. Cannot create/edit dashboards, run raw CSV exports if disabled on collection, or view unshared draft models. |
| **`BI Explorer / Analyst`** | All `BI Viewer` permissions + query certified/promoted `DatSemanticModel`s in `explore-workbench`, save `DatSavedExploration`s, create personal `DatMetricAlert`s, use grounded `"Ask BI"`. | Cannot modify canonical `DatSemanticModel` joins, alter `CERTIFIED` `DatMetricDefinition` formulas, or edit `DatRowSecurityPolicy` rules. |
| **`BI Author`** | Create and edit `DatSemanticModel`s (`DRAFT`/`PROMOTED`), author `DatMetricDefinition`s, build and publish `DatDashboard`s and `DatDashboardTile`s, submit certification requests. | Cannot self-certify models/metrics (`CERTIFIED` requires `Data Steward`) and cannot disable workspace-mandatory `DatRowSecurityPolicy` predicates. |
| **`Data Steward`** | Approve/reject `CERTIFIED` promotions via Home `WorkItem`s, link `#100` `DatBusinessGlossaryTerm`s, author and simulate `DatRowSecurityPolicy` rules, archive stale assets. | Cannot bypass audit logging; RLS "Test as Role" simulation displays aggregated/masked previews under audit trail. |
| **`BI Administrator`** | Manage workspace starter packs, configure `DatEmbedConfig` allowed origins and signing keys, tune snapshot refresh schedules (`refreshCron`), manage role assignments. | Every administrative override or embed key rotation emits an immutable security audit event. |

---

## 8. Technical APIs, Guarded Commands, Events & `requiredWorkflows`

### Guarded Commands (Idempotent & Concurrency-Safe)
Every mutation command requires `idempotencyKey: string`, `correlationId: string`, and `expectedVersion: number`:
1. `dat.bi.activateStarterPack({ packKey, idempotencyKey, correlationId })` — Idempotently provisions prebuilt semantic models, metrics, and dashboards without duplicating existing `code`/`slug` assets.
2. `dat.bi.publishSemanticModel({ semanticModelId, targetStatus, expectedVersion, idempotencyKey, correlationId })` — Validates DAG join acyclicity and downstream tile compatibility; if `targetStatus === 'PROMOTED'` with certification requested, creates a Home `WorkItem`.
3. `dat.bi.certifyAsset({ assetType, assetId, decision, reviewNotes, expectedVersion, idempotencyKey, correlationId })` — Restricted to `Data Steward`; transitions `DatSemanticModel`, `DatMetricDefinition`, or `DatDashboard` to `CERTIFIED` and resolves the linked `WorkItem`.
4. `dat.bi.refreshModelSnapshot({ semanticModelId, idempotencyKey, correlationId })` — Triggers asynchronous materialization of `CACHED_SNAPSHOT` aggregates and updates `lastRefreshedAt`.
5. `dat.bi.evaluateMetricAlert({ alertId, idempotencyKey, correlationId })` — Evaluates `DatMetricAlert` against `Decimal128` `thresholdValue`, respects `cooldownMinutes`, and opens/updates a Home `WorkItem`.
6. `dat.bi.issueEmbedToken({ embedConfigId, viewerContext, ttlSeconds, idempotencyKey, correlationId })` — Issues a cryptographically signed, RLS-scoped token (`ttlSeconds <= 86400`).

### Domain Events Published & Consumed (Kafka)
- **Published:** `dat.bi.semantic_model.certified.v1`, `dat.bi.metric.updated.v1`, `dat.bi.dashboard.published.v1`, `dat.bi.metric_alert.triggered.v1`, `dat.bi.subscription.delivered.v1`, `dat.bi.embed_token.issued.v1`.
- **Consumed:** `dat.warehouse.table.refreshed.v1` (from `#099` to trigger `CACHED_SNAPSHOT` invalidation), `dat.governance.classification.updated.v1` (from `#100` to auto-enforce column masking on newly tagged PII columns), `fin.period.closed.v1` (from `FIN` to refresh executive financial scorecards).

### `requiredWorkflows` (Declared in `app.manifest.json`)
- `biSnapshotRefreshWorkflow` — Scheduled & event-driven semantic cache materialization with exponential backoff.
- `biMetricAlertEvaluatorWorkflow` — Cron/refresh-triggered KPI threshold evaluation creating Home `WorkItem`s and optional downstream webhook triggers.
- `biReportSubscriptionBurstWorkflow` — Per-recipient RLS-scoped dashboard PDF/CSV rendering to Rapider Drive (`Files`) and digest notification dispatch.
- `biCertificationApprovalWorkflow` — Coordinates Steward review `WorkItem` lifecycle and audit stamping.

### Grounded Rapider AI Governance Rules
- `"Ask BI"` (`BID-026`) MUST compile natural-language requests exclusively into structured query specifications over permitted `DatSemanticModel` and `DatMetricDefinition` IDs — arbitrary free-form LLM-generated raw database queries are strictly prohibited.
- Every AI response MUST cite the exact `metricCode`, applied date window, data freshness timestamp, and active RLS policy name, and MUST require explicit human confirmation before creating a `DatMetricAlert` or modifying any `DatDashboardTile`.

---

## 9. Security, Privacy, Compliance & Non-Functional Requirements

- **Zero Cross-Tenant & Row Leakage (SOC 2 Type II / ISO 27001):** Workspace isolation is enforced at the platform host layer (zero `projectId` fields in app models), while `DatRowSecurityPolicy` predicates are injected server-side prior to SQL/aggregation execution, cache key derivation (`cacheKey = hash(modelId + querySpec + rlsPolicyScope)`), scheduled subscription rendering, and AI context assembly.
- **GDPR / CCPA & PII Masking:** Columns tagged as PII (`email`, `phone`, `nationalId`, `compensation`) in `#100` `DatClassificationTag` or `DatRowSecurityPolicy.maskedColumns` are redacted or aggregated (`COUNT >= 5` k-anonymity minimum for sensitive HR cohorts) before returning payloads to non-privileged callers.
- **Immutable Audit Trail & Retention:** Every semantic formula change, RLS policy edit, CSV/PDF data export, and embed token issuance writes an append-only audit entry retained for 7 years; cached snapshot artifacts auto-purge per workspace retention policy (default 90 days for rendered subscription exports).
- **Monetary Accuracy:** Financial KPIs (`ARR`, `Net Revenue`, `Gross Margin`, `AP/AR Aging`, `Budget Variance` in `\$`) strictly preserve `Decimal128` precision end-to-end without floating-point drift.
- **Accessibility & Responsive UX:** Full WCAG 2.2 AA compliance (colorblind-safe chart palettes, keyboard-accessible data tables fallback for every visual tile, ARIA live announcements on filter updates, `#ffffff !important` contrast on primary/danger buttons) across `390px` mobile scorecards, `768px` tablet grids, and `1440px` executive wallboards.

---

## 10. Deterministic Acceptance Criteria (14 Testable Criteria)

1. **AC-01 (Sidebar Minimum-3 Rule):** `app.manifest.json` defines exactly 3 visible sidebar sections (`Analytics`: 3 items, `Modeling & Studio`: 3 items, `Governance & Delivery`: 3 items), each containing `>= 3` `displayOnMenu: true` pages, while detail/drawer routes (`dashboard-view`, `semantic-model-detail`, `dashboard-tile-editor`) set `displayOnMenu: false`.
2. **AC-02 (SWR Clean Query & Defensive Filtering):** Mounting `bi-home`, `dashboards`, `kpi-scorecards`, `semantic-models`, `metrics-library`, `metric-alerts`, `report-subscriptions`, and `rls-embed-policies` invokes `findSWR`/`countSWR` with zero `isDeleted` query clauses in network payloads while filtering soft-deleted items in memory (`item.isDeleted !== true`).
3. **AC-03 (Table Actions & Click-Through Navigation):** On `dashboards`, `semantic-models`, `metrics-library`, and `metric-alerts`, zero `"View"` buttons exist in `Actions`; clicking the primary identifier physically navigates to the target detail view (`#/dashboard-view?id=...` or `#/semantic-model-detail?id=...`) without `<base href>` 404 diversion, `Edit` renders icon + `"Edit"` text, and `Delete` renders an icon-only button.
4. **AC-04 (Active Filter Tags & Clear All):** Entering a search query or clicking a collection/status pill on `dashboards` or `dashboard-view` immediately displays `#activeFilterTagsBar` with HTML-escaped chips (`Search: "..." ✕`, `Status: "..." ✕`) and `[Clear all]`; dismissing a chip or clicking `[Clear all]` synchronously restores the unfiltered tiles/rows.
5. **AC-05 (Optimistic Edit, Delete & Drawer Auto-Close):** Saving a new or edited tile in `dashboard-tile-editor` resets the save button inside a `finally` block, clears `DatDashboard`/`DatDashboardTile` SWR caches, broadcasts `RECORD_UPDATED`, closes the drawer via `closePageDrawer()`, and updates the canvas without a page reload; deleting a dashboard via `#rapiderConfirmOkBtn` removes it optimistically from memory with zero `window.confirm` calls.
6. **AC-06 (Semantic Fan-Out Guard):** Attempting to save a `DatSemanticModel` with an undeclared `MANY_TO_MANY` join or circular entity path fails deterministic validation with a descriptive field error before persistence.
7. **AC-07 (`Decimal128` Financial Metric Fidelity):** Evaluating a `CURRENCY` `DatMetricDefinition` over `FinInvoice` records totaling `\$1,250,430.17` vs. a `FinBudget` target of `\$1,200,000.00` computes an exact variance of `+\$50,430.17` (`+4.20%`) with zero floating-point rounding error.
8. **AC-08 (Row-Level & Column-Level Security Enforcement):** A `BI Viewer` scoped via `DatRowSecurityPolicy` to `Region = "EMEA"` querying `dashboard-view`, `explore-workbench`, CSV export, or `"Ask BI"` receives strictly `EMEA` aggregates and redacted `maskedColumns`, and never shares an unscoped SWR/snapshot cache key with an `AMER` viewer.
9. **AC-09 (Steward Certification via Home `WorkItem`):** Submitting a `DatSemanticModel` or `DatMetricDefinition` for certification creates a Rapider Home `WorkItem`; a `BI Author` attempting to invoke `dat.bi.certifyAsset` receives `403 Forbidden`, whereas a `Data Steward` approving it transitions status to `CERTIFIED` and displays the verified badge.
10. **AC-10 (Threshold Alert Cooldown Deduplication):** Triggering `dat.bi.evaluateMetricAlert` twice within `cooldownMinutes` when a KPI breaches `thresholdValue` creates exactly one Home `WorkItem` and updates its latest reading rather than creating duplicate tasks.
11. **AC-11 (Per-Recipient RLS Report Bursting):** Executing `biReportSubscriptionBurstWorkflow` for a `DatReportSubscription` with two recipients in different territories produces distinct territory-filtered snapshots rather than sending an unfiltered admin PDF.
12. **AC-12 (Back Navigation Filter Preservation):** Navigating from `dashboards` (with `Status: "CERTIFIED"` active) into `dashboard-view` and clicking `#btnGoBack` returns to `dashboards` with `Status: "CERTIFIED"` and `#activeFilterTagsBar` preserved.
13. **AC-13 (Grounded AI Metric Citation):** Submitting a natural-language question in `"Ask BI"` returns a response persisted in `projectChannelMessage` that explicitly cites the resolved `DatMetricDefinition.metricCode` and rejects queries referencing non-existent or unauthorized metrics.
14. **AC-14 (Dark/Light Theme & Button Contrast):** Automated `validate_theme` checks pass across all 12 routes in both light and dark modes with `#ffffff !important` white text on all primary and destructive buttons and WCAG 2.2 AA contrast ratios.
