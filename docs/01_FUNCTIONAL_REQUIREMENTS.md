# Cisco EA What-If Intelligence Engine — Functional Requirements

**Version:** 0.2 | **Updated:** 20 November 2026 | **Status:** Revised proposal / pending Cisco approval

## 1. Purpose and decision record

Build a Cisco-owned, question-first EA what-if capability. A user describes a business change in natural language without selecting a prebuilt scenario or specifying the outcomes to analyze. The system discovers relevant EA data, establishes a verifiable baseline, identifies business drivers, constructs a supported analysis, quantifies effects and presents the four decision lenses with evidence, uncertainty and editable assumptions.

**Crucial scope clarification:** The 44 examples and six clusters in Scope v4 are **reference cases and a domain taxonomy**, not a finite list of coded scenarios. The 16-Nov meeting and 19-Nov proposed deck require non-hardcoded scenario interpretation and explicitly challenges the prior fixed-lever POC. This is a change/clarification to the operating interpretation of v4; secure written sign-off on the revised boundaries before committing delivery dates.

### Source register

| ID | Provided source | Key evidence |
|---|---|---|
| S1 | `Cisco_EA_Architecture_Combined(2).pptx` | Sources → foundation → Ask/Interpret/Retrieve/Simulate/Explain/Decide/Refine → experience; unified model, knowledge index, lever registry, assumptions book. |
| S2 | `Cisco_EA_Lever_Modelling_Scope_v4(2).pptx` | Three lenses, six clusters, 44 examples, read-only Snowflake, auditability, P10/P50/P90, two-phase plan, Type 1/2/3 changes and D1–D8 acceptance. |
| S3 | `EA Modeling Scenarios and Data Readiness(1).xlsx`, `Scenario  Coverage` | A/C initial must-have examples; term flexibility marked Full; early renewal Partial; A True Up/Down Partial; data statuses vary. |
| S4 | `EA_Term_Flexibility_Client_Demo-16_Nov.pptx` | Existing C-08 CatBoost predictions B1/B3/B5; fixed adoption, five-year share and discount levers; calculated proxies are not measured churn/ops cost. |
| S5 | `Cisco .pdf` (meeting transcript) | Pages 1–3: user need not specify impacted metrics; 4–8: live Snowflake early; 11–21: baseline/business explanation; 22–27 and 31–35: no per-scenario coding, evolving data, examples not scope. Transcript ends mid-discussion. |

| S6 | `Cisco_EA_WhatIf_Approach (1).pptx` (19-Nov deck) | Four proposed lenses; planner + four lens specialists; approved lever catalog/formula book; margin excluded; federal scenario demo on 11 Dec, contingent on readiness. |
| S7 | Manager modeling proposal (conversation) | Renewal, bookings/expansion, lever elasticity/causal ML, cross-sell; Monte Carlo as a simulation service; Snowflake-native aspiration. |
| S8 | Updated project plan and 14-/37-scenario planning discussion | Preliminary staffing (~2 developers), phase/dependency gates; scenario count is not model count. |

## 2. Product goals and non-goals

**Goals:** (G1) support novel phrasing and novel combinations of **supported** mechanics; (G2) business-first, traceable outputs; (G3) live Cisco-controlled data; (G4) reusable models/analytical operators; (G5) graceful abstention on insufficient evidence; (G6) self-service parameter and assumption correction.

**Non-goals without new approval:** unrestricted LLM execution of financial code, unvalidated claims of causal uplift, arbitrary external data/web APIs, production CRM/billing writeback, autonomous business decisions, guaranteed numerical answers for unseen mechanics, or extracting production Cisco data to local machines.

## 3. Personas and permissions

| Persona | Capabilities |
|---|---|
| EA business user | Ask what-if, inspect baseline and results, change permitted levers, export approved view. |
| Finance/EA SME | Approve metric definitions and assumption ranges, challenge model behavior, sign off baseline. |
| Cisco data steward | Register semantic views, certify keys, grains, lineage and data-quality rules. |
| Model owner | Validate, promote, monitor and recalibrate response models. |
| Platform admin | Configure RBAC, audit retention, approved tools and execution budgets. |

All permissions are subject to Cisco identity and data policies; exact role mapping is TBD.

## 4. End-to-end functional workflow

1. **Ask:** User enters any business-language what-if query; supported variations must not require a hardcoded scenario ID.
2. **Understand:** LLM extracts cohort, region, suite, term, time horizon, interventions, requested KPIs and ambiguities into a typed intent. It does **not** invent new authoritative lever values or business rules.
3. **Discover and validate:** Planner queries the approved **Filter, Lever, Metric/KPI, Formula/Business Rule, Model, and Data Registries**, plus RAG for versioned EA guidance. The planner may choose relevant controls and lens-specific analysis paths only from registered capabilities.
4. **Baseline:** Query governed read-only Snowflake semantic views for the question's exact scope; use actuals for historical periods and a clearly labeled no-change forecast for future periods. Ensure matching population, period, currency, grain and measure for scenario comparisons.
5. **Propose controls:** Present approved lever ranges/defaults, filters and editable assumptions; show policy, data, and model-supported ranges separately. Unsupported levers produce a clear gap or hypothetical mode.
6. **Plan:** Build a validated execution DAG containing selected predictive targets, causal/assumption-based response estimates, approved economic operators, uncertainty requirements and impacted KPIs/lenses.
7. **Execute:** Apply selected interventions; invoke validated models; calculate contractual consequences and scenario cash flows through **Simulation Engine**; use **KPI Engine** for common metric definitions, aggregation and baseline/scenario deltas.
8. **Quantify:** Calibrated Monte Carlo P10/P50/P90 when distributional evidence is sufficient; do not imply reliability from number of draws alone. Flag confounding, unsupported intervention ranges and missing data.
9. **Explain:** Show **Growth**, **Discounting**, **Customer Value**, **Ops Cost & Simplicity**, including missing measurements where data is unavailable. Margin/profitability are **excluded** under the 19-Nov deck's proposed scope.
10. **Refine and audit:** Re-run with authorized lever changes; retain versions of models, formulas, policies, data cuts, assumptions and outputs.

## 5. Functional requirements

| ID | Priority | Requirement | Acceptance evidence |
|---|---|---|---|
| FR-01 | Must | Interpret free-text EA what-if questions without per-scenario code or fixed scenario IDs, within **supported capabilities**. | Paraphrase and novel-composition tests. |
| FR-02 | Must | Parse question into typed cohort filters, horizon, lever deltas and requested/impacted metrics. | Validated JSON schema and ambiguity tests. |
| FR-03 | Must | Select applicable filters, levers and default/value ranges from versioned, approved registries; no LLM-invented authoritative values. | Registry-driven UI tests. |
| FR-04 | Must | Discover supported models, finance formulas, dependency mappings and data views from governed registries. | Capability lookup and unknown-lever rejection tests. |
| FR-05 | Must | Reconcile historical baseline to Cisco-certified Snowflake figures; separate observed baseline from future **no-change forecast**. | Grain, time period, reconciliation and Finance review. |
| FR-06 | Must | Execute reusable analysis operators (not one implementation per scenario); support model and operator selection via validated planner. | One pipeline serving more than one supported question family. |
| FR-07 | Must | Use target-specific trained prediction services (initially 1–2; first phase 2–3 if data permits), with version, coverage and temporal validation. | Holdout/calibration/model-card review. |
| FR-08 | Must | Show **four lenses: Growth, Discounting, Customer Value, Ops Cost & Simplicity**; mark unavailable/proxy metrics, do not fabricate values. | Lens-level source/evidence review. |
| FR-09 | Must | Maintain approved **Formula/Business Rule Book**; LLM may select but may not execute self-generated business formulas. | Formula unit tests, approval evidence, versioned execution logs. |
| FR-10 | Must | Keep Simulation Engine (lever/model/contract transformations) separate from KPI Engine (metric formulas/deltas/lens aggregation). | Golden numerical regression tests. |
| FR-11 | Must | Distinguish predictive association, causal lever response, deterministic calculation and assumption-driven sensitivity. | Result-level provenance flags and intervention-support checks. |
| FR-12 | Must | RAG supports policies and explanations before/during/after execution; only reviewed rule extraction becomes executable. | Policy source/version citation tests. |
| FR-13 | Must | Enforce read-only semantic queries, RBAC, access scope, budgets, auditability and prompt-injection controls. | Security and policy-gate tests. |
| FR-14 | Should | Provide Monte Carlo P10/P50/P90 with calibrated distributions and dependent outcomes where feasible. | Backtest and coverage evidence. |
| FR-15 | Should | Compare land/expand/renew bookings, discounts, renewal health, adoption and operational events where real data is available. | Four-lens coverage matrix with gaps. |
| FR-16 | Should | Add metadata-backed capability without changing scenario-specific application logic; new mechanism still requires reviewed operator/model. | Configuration extension test. |
| FR-17 | Should | Provide Streamlit POC UX and integration-ready interface, with independently testable orchestrator API. | End-to-end demo and typed API tests. |
| FR-18 | Should | Export summary, evidence, driver attribution and run versions; do not mislabel SHAP as causal effect. | Traceability and explanation review. |
| FR-19 | Should | Monitor model drift, data freshness, latency, cost and failed/unsupported runs. | Observability dashboard and runbook. |
| FR-20 | Must | Exclude margin/profitability metrics unless Cisco explicitly changes scope. | Lens/KPI allowlist test. |

## 6. Business definitions requiring sign-off

- **Four-lens approval:** final lens names, KPI owners and whether Discounting is a separate lens (19-Nov deck) versus a Growth submetric (older three-lens scope).
- **Baseline:** observed historical versus projected no-change; comparison horizon, period cut, fiscal/calendar year, exact cohorts, exchange rates and deduplication of annualized bookings.
- **Revenue/booking semantics:** bookings vs annualized price vs total commitments vs cash flows; discounts already reflected in net bookings; no double-application of price effects.
- **Renewal:** consistent label such as renewal within 90 days of term end **only after** handling amendments, partial renewals, migration and censoring; Finance approval needed.
- **Model-ready grain:** customer × agreement × suite × term (initial hypothesis); verify unique keys and time-ordered observations, model-specific targets/grains.
- **Lever definitions and allowed values:** installed-base coverage, tier, term, programmatic/non-standard discount, True Forward, growth allowance, payment frequency, value shift, cross-sell; separate policy/observed/model-supported ranges.
- **Customer Value:** adoption, consumption, installed-base/product-mix sources are not yet certified in the EA semantic view per the 19-Nov deck.
- **Ops Cost & Simplicity:** deal-desk approvals, amendments, renewal events, quoting, billing cycle and unit-cost data sources are not yet established per the 19-Nov deck.
- **Causal validity:** term, discount and free quantity may be selected based on risk; regressors or monotonic constraints alone do not demonstrate intervention effects.
- **Exclusions:** margin/profitability excluded per current deck; obtain explicit approval for any change.

## 7. Scenario example coverage and data readiness

| Reference example | Cluster | Workbook status | Functional test purpose |
|---|---|---|---|
| Flexible 1/5 vs 3 years | C | Full | Baseline, term semantics, question-to-plan and counterfactual validation. |
| Early renewal incentive | C | Partial | Missing-label/evidence behavior. |
| True Forward to True Up/Down | A | Partial | Cross-cluster planning and partial data. |
| TF cap adjustment | A | Full | Different mechanics through same engine. |
| Adoption reward | E | Gap | Abstention and scenario design with explicit assumptions. |

Workbook availability statuses are **not** proof that all four-lens outcome labels exist.

## 8. User experience and response contract

**Question:** original prompt; parsed filters, lever changes, cohort, horizon and missing inputs; planner's supported/unsupported status.

**Controls:** dynamic lever/filter widgets from signed registry metadata; display source, permitted range, observed range, model-supported range, default strategy and override permissions.

**Baseline:** observed or projected-no-change label; metrics, exact cohort, Snowflake source, timestamp, grain, formula version, Finance reconciliation status.

**Scenario:** approved calculations, selected model artifacts, outcome estimates, financial deltas and four lens panels (Growth; Discounting; Customer Value; Ops Cost & Simplicity). Show `not_available` rather than a manufactured KPI if data is missing; margin omitted.

**Uncertainty/attribution:** P10/P50/P90 only if calibrated, dependence-aware distributions are available; SHAP explains model predictions but is **not** proof of lever causality; causal response estimates have separate assumptions.

**Audit:** immutable run ID, actor, question, inputs, data cut, query hashes, policy version, approved model/formula ID, code version, controls and outputs. Mark each value `observed`, `predicted`, `causal_estimate`, `assumption_based`, `derived` or `illustrative`.

## 9. Nonfunctional requirements

- **Performance:** v4 target user runs under five minutes; set separate budgets for question parsing, SQL, scoring and Monte Carlo. Validate on real Snowflake scale early, not at handover.
- **Scale:** query pushdown, partition pruning, aggregate at correct grain, bounded scoring batches, avoid UI-loading 280M raw rows.
- **Security:** no production CSV downloads, no secrets in prompts/logs, approved LLM deployment and Cisco residency requirements, read-only query role.
- **Reliability:** idempotent run IDs, timeouts, retries for safe operations, failure states and deterministic seeds.
- **Quality:** temporal validation, leakage checks, uncertainty calibration, financial reconciliation, SME approval and regression suite.
- **Observability:** prompt/plan/tool/model traces with sensitive data redacted; drift and cost monitoring.

## 10. Acceptance and milestone proposal

**Planned kickoff assumption:** 23 November 2026, subject to access and Cisco SME availability. The 19-Nov discussion deck proposes working demos each Friday and an **11 December federal-term scenario** demonstration. This date is a **conditional intermediate milestone** rather than a completed four-lens platform.

| Gate | Indicative date/window | Deliverable | Dependency / qualification |
|---|---|---|---|
| D0: foundation | 23 Nov–4 Dec | Named owners, certified bookings/discount baseline slice, data gaps, lever/filter/metric/formula catalogs v0, LLM parsing skeleton | Snowflake read access and Finance definitions |
| D1: federal demo | **11 Dec 2026 (conditional)** | One supported federal term-flexibility question end-to-end: parsed intent → catalog-selected controls → baseline → approved operators and available model inference → KPI comparisons and four lens panels with missing-data flags | Data and labeled-model readiness; illustrative results distinctly marked if not validated |
| D2: integrated first-phase POC | **4–6 working weeks from ready start** | 2–3 selected models *where labels and evidence permit*, registry/plan validator, Snowflake execution, baseline and simulation, tested calculations and initial four-lens outputs | Team assumption: ~2 full-time developers working in parallel + Cisco owners; causal model may extend window |
| D3: broader scenario coverage | **14 scenarios: ~10–14 weeks; 37 scenarios: ~16–24 weeks (planning ranges)** | Scenario-to-capability coverage, added views/operators/models, validation by supported question family | Not 14/37 separate trained models; resource allocation and scenario complexity unconfirmed |
| D4: production hardening | After validated POC; separately planned | RBAC, monitoring, capacity, CI/CD, business approvals, runbooks, release gates | Not included by simply calling the POC deployed |

Dates are **estimates, not commitments**. The source deck's weekly Gantt is a proposal and the revised workbook's precise activity dates reflect its estimates; neither guarantees 11-Dec availability of renewal labels, Customer Value or Ops Cost metrics, or causal estimates. Validation/approval gates can shift all dates.

## 11. Decisions and open questions

1. Confirm **four lenses** and exact KPIs, owners and source data, especially Discounting and Ops Cost.
2. Confirm “no hardcoding” means no *per-scenario* dispatch; **approved reusable finance formulas and governed registry metadata** remain required.
3. Confirm any-business-question behavior means coverage of *registered, validated analytical mechanics* with graceful gaps outside coverage.
4. Confirm exact 11-Dec demo scope, required evidence versus illustrative behavior, and named Cisco approvers.
5. Confirm labels/history for renewal, bookings, expansion, cross-sell, treatment selection and causal overlap.
6. Confirm Cisco-accessible Snowflake SQL/views, Feature Store/Registry, ML Jobs, CPU/GPU Container Services, Cortex/RAG and Streamlit privileges and budgets.
7. Confirm customer/account/contract grain, renewal definition, discount semantics, time horizons and portfolio reconciliation.
8. Identify Customer Value (adoption/consumption) and Ops Cost (deal desk/approvals) data owners; define gap behavior until available.
9. Determine 14/37 scenario-to-lever, target, source, operator and test mapping before committing coverage dates.
10. Confirm model independence versus cross-model correlation and uncertainty-calibration requirements for Monte Carlo.
11. Review use of monotone constraints against observed evidence; do not hard-force all green/red hypotheses.
12. Confirm whether the diagram's four lens specialists are four separate agents or lighter **lens modules sharing one validated planner** for POC.

## 12. Traceability summary

- S1 → FR-02–04, FR-06, FR-09, FR-12, FR-15.
- S2 → FR-05, FR-08–09, FR-11–18; security, lenses, D1–D8 and quantitative criteria.
- S3 → scenario reference tests and missing-data behavior.
- S4 → reuse existing term-model inference and identify proxy/causality gaps.
- S5 → FR-01–03, FR-05–07, FR-10–12; early Snowflake integration and business-first demonstration.

**Document status:** source-grounded requirements plus explicitly proposed implementation interpretation; pending client confirmation.

## Document alignment and decision status (20 November 2026)

**Latest deck baseline:** `Cisco_EA_WhatIf_Approach (1).pptx`, dated 19 November 2026, is the controlling **proposed client discussion narrative** for this revision. It specifies **four lenses**: **Growth, Discounting, Customer Value, and Ops Cost & Simplicity**. Earlier scope and architecture decks described **three lenses**; this is a revision that requires Cisco confirmation and consistent metric ownership.

**Evidence vs proposal:** The deck states the desired workflow and a draft calendar; it is not proof that all four lenses have usable data, causal lever-effect estimates, approved formulas, trained models, or production access. Items marked **Proposed** below are implementation recommendations. Items marked **Pending Cisco sign-off** are not established requirements. Its federal term-flexibility walkthrough contains illustrative directional impacts, not validated quantitative predictions.

**Other inputs:** `Cisco_EA_Architecture_Combined(2).pptx`; `Cisco_EA_Lever_Modelling_Scope_v4(2).pptx`; `EA Modeling Scenarios and Data Readiness(1).xlsx`; the 16-Nov term flexibility POC deck; manager's four-model + Monte Carlo suggestion; and the follow-up 14-/37-scenario planning discussion. The 14 and 37 counts are planning comparisons, not model counts or a promise of coverage. The 11-Dec-2026 federal demo is a *conditional proposed milestone*, not a promise of four fully measured lenses.
