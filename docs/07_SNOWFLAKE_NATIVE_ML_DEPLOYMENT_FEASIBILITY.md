# Cisco EA What-If Modeling — Snowflake-Native ML Deployment Feasibility

**Date:** 19 November 2026  
**Document type:** Technical feasibility assessment and implementation recommendation  
**Status:** Proposed architecture; dependent on Cisco account capabilities, permissions, data readiness and validation

## Executive summary

It is technically feasible to implement most of the Cisco EA What-If Modeling solution within the Snowflake ecosystem while retaining Python code and existing CatBoost models. The recommended approach is **Snowflake-native with multiple purpose-built execution environments**, rather than attempting to run every component inside a single SQL warehouse or Python stored procedure.

Snowflake would provide governed structured data, feature preparation, model management, LLM capabilities, and potentially the Streamlit UI. Snowflake ML Jobs or suitable container runtimes would run Python-based training and specialized workloads. The simulation and KPI layers would use approved, versioned formulas and predictive model outputs.

**Core recommendation:** Begin with one end-to-end scenario and one CatBoost model trained, registered, and invoked in Cisco's actual Snowflake environment. Expand to two or three validated models and broader scenario coverage once the platform and data foundations have been verified.

## 20 November 2026 deployment decision and compatibility gates

**Logical solution alignment:** baseline-first, question-first, LLM-selected **registered** controls, Finance-approved formula book, separate Simulation and KPI engines, and the latest deck's **four lenses** (Growth, Discounting, Customer Value, Ops Cost & Simplicity). The manager's predictive priorities are renewal, bookings/expansion and causal lever effects; cross-sell later; Monte Carlo is simulation, not another supervised model.

**Recommended Snowflake target** (only where enabled in Cisco account):

| Logical task | Preferred runtime | Key validation |
|---|---|---|
| Historical baseline, filters, deduplicated metrics, feature engineering | Snowflake SQL + Snowpark, certified read-only semantic views | Finance metric sign-off, keys, access |
| Labeled CatBoost renewal/bookings training | Snowflake ML Jobs / suitable Snowpark Python compute | Package/version approvals, memory, job and registry rights |
| Model artifact governance and inference | Snowflake Model Registry / approved service | Signatures, models, promotion, batch/interactive latency |
| EconML/Double ML and complex Python simulations | Approved ML Job or Snowpark Container Services | Custom dependencies, CPU/GPU compute pools, causal validity |
| Formula registry and KPI processing | Snowflake tables plus tested SQL/Python operators | Formula approvals, numerical golden tests |
| LLM planner and PDF/RAG guidance | Approved Cortex AI + Search/document processing | Region, policy, cost and prompt-injection controls |
| UI and lens dashboards | Streamlit in Snowflake or approved Cisco UI/API adapter | Network/security, authorized access, runtime limits |
| NVIDIA Kumo | Optional external/contained experimental provider | GPU, version, license, latency/security; **not a dependency** |

**No guarantee:** Snowflake capability support does not guarantee enablement in Cisco's tenant. Confirm Cortex availability, ML Jobs, CPU/GPU pools, Snowpark Container Services, package allowlist, stage and registry permissions before locking deployment dates. No need to rewrite all Python model logic as SQL, but data I/O, feature loading, packaging, security and model-serving interfaces **will** change.

**Delivery:** technical feasibility spike first (one train-register-score loop + query baseline). Conditional 11-Dec federal scenario checkpoint; 4–6 weeks for integrated 2–3-model phase if labels and access are ready. Broader 14- and 37-scenario estimates are governed by new targets, data sources, formulas and tests, not separate model builds for each scenario. Financial calculations must remain in approved operators, not LLM-generated runtime code. Margin excluded.

## 1. Component feasibility and recommended runtime

| Cisco EA component | Suggested Snowflake service / runtime | Assessment and caveats |
|---|---|---|
| Structured data and feature engineering | Snowflake SQL, Snowpark Python, governed views/tables | High feasibility; clarify join keys, data quality, access and feature freshness |
| CatBoost training | Snowflake ML Jobs, supported Python training runtimes, CPU/GPU compute where applicable | Feasible; validate package versions, memory, job permissions and costs |
| Renewal, bookings and cross-sell prediction | Snowflake Model Registry and supported inference execution | Feasible; confirm model-signature and deployment support in Cisco's environment |
| Lever elasticity / causal ML | Custom Python environment, potentially Snowpark Container Services | Feasible in principle; more demanding dependencies and statistical identification requirements |
| Monte Carlo simulations | Python jobs/services or efficient SQL-based simulation where appropriate | Feasible; design for workload size, reproducibility and latency |
| Historical baseline | Governed SQL views / aggregations | High feasibility; requires approved metric grain and time windows |
| Future no-change baseline | Predictive models plus business rules, on the same horizon as proposed scenarios | Feasible if future labels and models can be validated |
| Levers, filters, rules, formulas and KPI registries | Snowflake tables and configuration metadata | High feasibility; all entries should be versioned and reviewed |
| LLM scenario interpretation and planning | Snowflake Cortex AI or permitted alternative model endpoint | Feasible subject to enabled features, region, approvals and access |
| Guidance PDFs / RAG | Snowflake stages, document extraction and Cortex Search or permitted alternatives | Feasible subject to document-processing and search availability |
| Streamlit UI | Streamlit in Snowflake or existing external Streamlit app | Feasible; validate dependencies, interactivity and service connectivity |
| NVIDIA Kumo | Custom provider/runtime feasibility spike | Do **not** treat as a committed Snowflake-native component until runtime, licensing and benchmarking are verified |

**Key caveat:** “Supported in the Snowflake ecosystem” is not the same as “enabled in Cisco's Snowflake account.” Permissions, region support, runtime restrictions, approved packages, GPU availability and infrastructure budgets are gating dependencies.

## 2. Proposed logical architecture

```mermaid
flowchart TD
    UI[Streamlit UI: natural-language scenario, dynamic controls, four lenses]
    PLAN[Cortex AI / LLM interpreter and validated AI planner]
    META[Approved registries: filters, levers, metrics, rules, model capabilities]
    GUIDANCE[EA guidance PDFs, RAG / Cortex Search]
    DATA[Snowflake source tables and governed semantic views]
    BASE[Baseline service: actuals or forecast]
    TRAIN[Offline Snowflake ML Jobs / Python training]
    REG[Versioned Snowflake Model Registry]
    CAUSAL[Causal / elasticity analysis service]
    SIM[Simulation engine: lever application, business operators, Monte Carlo]
    KPI[KPI engine: canonical metrics, deltas and uncertainty]
    LENSES[Growth | Discounting | Customer Value | Ops Cost & Simplicity]

    UI --> PLAN
    GUIDANCE --> PLAN
    META --> PLAN
    DATA --> BASE
    DATA --> TRAIN
    TRAIN --> REG
    PLAN --> BASE
    PLAN --> SIM
    META --> SIM
    BASE --> SIM
    REG --> SIM
    CAUSAL --> SIM
    SIM --> KPI
    META --> KPI
    KPI --> LENSES
    LENSES --> UI
```

### Offline workflow

1. Access approved historical Snowflake data.
2. Resolve contract/customer/agreement/suite grain and construct time-aware features and labels.
3. Create a versioned feature/training dataset.
4. Run Python/CatBoost training through an appropriate Snowflake execution runtime.
5. Validate performance, calibration, leakage, robustness and applicability.
6. Register an approved model version, signatures and supported features in the Model Registry.
7. Monitor data drift and retrain only when justified by policy and validation.

### Online scenario workflow

1. User asks an EA business question in the UI.
2. LLM produces structured intent: population filters, relevant levers, requested metrics and time horizon.
3. Backend validates the plan against approved registries and guidance-derived rules.
4. Baseline service retrieves current actuals or calculates a matched no-change forecast.
5. Prediction services score the approved models required for that scenario.
6. Simulation engine applies levers and reusable calculations, with causal/assumption controls where needed.
7. KPI engine compares baseline and scenario using the same metric definitions.
8. Results are shown across four lenses with traceable evidence, uncertainty and limitations.

**Important:** Models are trained offline/periodically. Moving a UI slider should trigger inference and simulation, not retraining.

## 3. Reusing existing Python and CatBoost code

Most of the existing Python implementation can be retained. The key migration changes are **data access, execution and deployment**.

| Local POC | Snowflake-oriented equivalent |
|---|---|
| `pd.read_csv(...)` | Governed Snowflake view, Snowpark DataFrame or suitable training-data connector |
| Local pandas feature transformations | SQL / Snowpark transformations and versioned feature datasets |
| `CatBoostRegressor.fit(X_train, y_train)` | Same core Python training logic executed in a supported Snowflake ML runtime |
| Local CatBoost artifact (`.cbm`) | Approved model version registered and deployed through Snowflake Model Registry |
| Local `model.predict(...)` | Model Registry inference or approved Python inference service |
| Local simulation functions | Reusable, validated SQL/Python operators invoked by a scenario executor |
| Local Streamlit UI | Streamlit in Snowflake, if it satisfies the app and integration requirements |

Illustrative existing training logic:

```python
from catboost import CatBoostRegressor

model = CatBoostRegressor(
    iterations=500,
    depth=6,
    learning_rate=0.05,
    loss_function="RMSE",
    verbose=False,
)
model.fit(X_train, y_train)
```

The model algorithm need not be rewritten in SQL. What changes is how `X_train` and `y_train` are produced and how training, registration and inference are orchestrated.

### Implications for a 1 GB raw CSV

A 1 GB CSV can require substantially more memory after decoding, joins, transformations and model training. Prefer filtering, joining, summarizing and enforcing data quality within Snowflake before loading prepared records into the Python training environment. Validate compute sizing with actual representative data.

## 4. Python stored procedures versus container-based compute

### Snowpark Python / SQL

Use for bounded transformations and relatively lightweight governed calculations such as:

- Baseline SQL aggregations.
- Feature engineering and semantic-view construction.
- KPI calculations and business-rule lookups.
- Bounded inference where runtime/package support and resource limits permit.

### Snowpark Container Services / ML Jobs

Prefer for workloads needing custom environments, larger memory/GPU resources or more complex Python execution:

- CatBoost training and validation at scale.
- EconML/Double ML or specialized statistical dependencies.
- Computationally heavy Monte Carlo simulation.
- Custom inference services or advanced integrations.

Not every component requires a container. An approved formula that can be evaluated efficiently in SQL should remain in SQL; a large Python simulation may warrant dedicated compute. Confirm the specific supported runtime for each workload during the technical spike.

## 5. LLM planner, registries and guardrails

The LLM should **interpret the business question and propose a structured plan**. It should not independently invent financial formulas, discount ranges or arbitrary Python/SQL to execute.

Example candidate plan (illustrative):

```json
{
  "scenario": "term_flexibility",
  "filters": {
    "segment": "Federal",
    "suite": "Switching"
  },
  "levers": {
    "contract_term_years": 1,
    "true_forward_enabled": false
  },
  "models": [
    "renewal_propensity_v1",
    "bookings_prediction_v1"
  ],
  "calculations": [
    "commitment_v1",
    "kpi_delta_v1"
  ]
}
```

Validation requires checking:

- The requested filters and lever IDs exist in a governed registry.
- Lever values are policy-allowed and within validated model support.
- The model IDs exist and support the requested targets/features.
- Formula/operator IDs are approved, versioned and compatible.
- Data access and population restrictions are applied.
- Unsupported predictions or causal claims are flagged rather than fabricated.

For a first-phase POC, a **single planner with approved tools and metadata** is simpler than five separate agents. Four lens-specific output configurations can still be supported without four separate LLM processes.

## 6. Guidance data and RAG inside Snowflake

Recommended flow:

`Guidance PDFs → Snowflake stage → Document extraction/chunking → Cortex Search / RAG → LLM planner and explanation`

Use guidance for definitions, eligibility rules, policy conditions, documented assumptions and explanations. Convert authoritative executable rules into structured, reviewed records; retrieved paragraphs are not themselves executable financial policies.

Guidance may also inform pre-modeling feature/target definitions and post-modeling explanations. A rule extraction process should preserve document versions and require SME review before activation.

## 7. Baseline, simulation and KPI responsibilities

| Component | Responsibility |
|---|---|
| Baseline service | Establish current actuals or a future no-change forecast for the same population and horizon |
| ML services | Predict supported outcomes such as renewal probability and future bookings |
| Causal / elasticity service | Estimate applicable intervention effects where identification is defensible |
| Simulation engine | Apply lever modifications, execute approved operators, invoke models and optionally run Monte Carlo |
| KPI engine | Calculate canonical metrics and baseline-versus-scenario absolute/percentage changes |
| Lens layer | Present metrics under Growth, Discounting, Customer Value, and Ops Cost & Simplicity |

**Historical baseline:** Compute from actual Snowflake facts where the historical period is complete and metrics are approved.

**Forward-looking baseline:** Use the same future horizon, cohort and modeling pipeline as the hypothetical scenario but keep current business policies unchanged. Compare forecast to forecast, rather than comparing a 2027 projection directly against unrelated 2026 actuals.

Any use of a predictive model to change a lever should clearly distinguish **associational predictions**, **validated intervention-response estimates**, and **assumption-based sensitivity analyses**.

## 8. NVIDIA Kumo assessment

Kumo remains an optional candidate predictive provider. It may reduce task-specific model engineering for some relational prediction problems, but does not automatically solve causal lever effects or replace business-rule governance and simulation.

Before adoption verify:

1. Compatibility with Cisco's permitted compute/GPU environment.
2. Data residency, access and security requirements.
3. Applicable licenses and commercial deployment rights.
4. Runtime/package dependencies and inference request limits.
5. Predictive performance and calibration against existing CatBoost baselines.
6. End-to-end costs and operational complexity.

Do not put Kumo on the critical path for the initial delivery milestone.

## 9. Main risks, dependencies and blockers

| Dependency | Why it matters | Priority |
|---|---|---|
| Snowpark Container Services / ML Jobs access | Blocks some custom training and simulation workflows | Critical |
| CPU/GPU compute pools, memory and quotas | Affects training feasibility and concurrency | Critical |
| Approved Python packages (CatBoost, EconML, etc.) | Determines supported training/causal methods | High |
| Historical renewal, expansion and cross-sell labels | Determines which models can be trained and evaluated | Critical |
| Data quality, lineage and historical snapshots | Prevents leakage and inconsistent metrics | Critical |
| Cortex model/search/document permissions and region availability | Affects dynamic planning and guidance integration | High |
| Snowflake Model Registry privileges | Supports deployment and model governance | High |
| Finance-approved formulas, lever boundaries and KPI definitions | Prevents invented or inconsistent calculations | Critical |
| Compute budget, latency and concurrency targets | Determines service design and cost | High |
| Streamlit runtime and integration constraints | Affects final user experience | High |

### Questions for Cisco's platform and data teams

1. Is Snowpark Container Services enabled? Can our team create or use CPU/GPU compute pools?
2. Can we submit Snowflake ML Jobs and create Feature Views/feature datasets and Model Registry entries?
3. Which Python versions and external packages are allowed? Are CatBoost and EconML available in the intended runtime?
4. Are Cortex AI, Search and document-processing services enabled and approved in the account's region?
5. What are the compute budgets, quotas, concurrency expectations and maximum response latency?
6. Can we deploy Streamlit in Snowflake and connect it to inference/simulation services?
7. Are bookings, renewals, install base, discounts, adoption, consumption and operational/Deal Desk datasets accessible?
8. Who owns final approval of KPI definitions, financial formulas and policy constraints?
9. What is expected by 11 December 2026: technical feasibility, a limited scenario demo, a validated POC, or production readiness?

## 10. Recommended phased implementation

### Phase A — Snowflake technical feasibility spike

**Indicative effort:** 3–5 working days *after* access, permissions and compute are available. This is a technical feasibility estimate, not an end-to-end project estimate.

- Verify Snowflake connection, access to a representative feature table and permissions.
- Train or execute one existing CatBoost model in the approved Snowflake runtime.
- Register a validated model version and invoke inference.
- Verify basic cost, memory, latency and package compatibility.
- Document blockers and decide whether to use warehouse-based or container-based execution.

**Exit criterion:** A reproducible Python training/inference path in Cisco's Snowflake environment.

### Phase B — One end-to-end scenario

- Create a governed term-flexibility data slice and baseline.
- Implement initial lever, filter, model, business-rule and KPI registries.
- Build approved term/discount/commitment operators.
- Connect the trained model to scenario inference.
- Use LLM interpretation to select only approved controls and calculations.
- Render results in a working UI and present applicable four-lens metrics with source/assumption flags.

**Exit criterion:** One reproducible scenario answered from Snowflake-backed data with transparent calculations and a defensible baseline.

### Phase C — Two to three models and expanded coverage

- Add renewal propensity and bookings/expansion models where labels are adequate.
- Evaluate elasticity/causal modeling only where evidence and assumptions support it.
- Introduce uncertainty estimation and Monte Carlo as justified by available predictive distributions.
- Validate more questions through the same registries, planner and executor.
- Add model monitoring, calibrated outputs, documentation and SME sign-off.

**Exit criterion:** A validated first-phase integrated POC, not a claim that every possible scenario is automatically supported.

### December 11 planning implication

Prioritize proving the **one-model, one-scenario, end-to-end Snowflake integration**. A fully generalized, production-grade framework with all models, four complete lenses, causal responses, Monte Carlo and broad scenario coverage is a separate delivery objective and should not be assumed feasible by that date without confirmed scope and dependencies.

## 11. Final decision and rationale

Use a **Snowflake-native hybrid execution architecture**:

- **SQL / Snowpark** for governed data preparation, baselines and KPIs.
- **Python ML Jobs / appropriate container compute** for CatBoost and advanced model workloads.
- **Model Registry** for approved, versioned inference capabilities.
- **Cortex AI and RAG** for question interpretation, guidance retrieval and explanation, if approved and enabled.
- **Registries and validated operators** for lever values, filter definitions, business rules and financial formulas.
- **Streamlit** for the interface and four-lens presentation, subject to runtime suitability.

This preserves the current Python/CatBoost investment while separating high-volume data, heavy model training, interactive inference, deterministic calculations and LLM reasoning into suitable execution environments. The decisive next step is to verify the design **inside Cisco's actual Snowflake account**.

## 12. Documentation and verification references

Official Snowflake documentation is the authoritative place to verify feature support, runtimes and account prerequisites at implementation time:

- Snowflake documentation: https://docs.snowflake.com/
- Snowflake ML overview: https://docs.snowflake.com/en/developer-guide/snowflake-ml/overview
- Snowpark Container Services overview: https://docs.snowflake.com/en/developer-guide/snowpark-container-services/overview
- Snowflake Model Registry: https://docs.snowflake.com/en/developer-guide/snowflake-ml/model-registry/overview
- Cortex AI overview: https://docs.snowflake.com/en/user-guide/snowflake-cortex/overview
- Streamlit in Snowflake: https://docs.snowflake.com/en/developer-guide/streamlit/about-streamlit

*These links are reference entry points, not confirmation that a specific Cisco account has the corresponding feature enabled. Validate service availability, permissions, package support and licensing with Cisco administrators before implementation.*

## Document alignment and decision status (20 November 2026)

**Latest deck baseline:** `Cisco_EA_WhatIf_Approach (1).pptx`, dated 19 November 2026, is the controlling **proposed client discussion narrative** for this revision. It specifies **four lenses**: **Growth, Discounting, Customer Value, and Ops Cost & Simplicity**. Earlier scope and architecture decks described **three lenses**; this is a revision that requires Cisco confirmation and consistent metric ownership.

**Evidence vs proposal:** The deck states the desired workflow and a draft calendar; it is not proof that all four lenses have usable data, causal lever-effect estimates, approved formulas, trained models, or production access. Items marked **Proposed** below are implementation recommendations. Items marked **Pending Cisco sign-off** are not established requirements. Its federal term-flexibility walkthrough contains illustrative directional impacts, not validated quantitative predictions.

**Other inputs:** `Cisco_EA_Architecture_Combined(2).pptx`; `Cisco_EA_Lever_Modelling_Scope_v4(2).pptx`; `EA Modeling Scenarios and Data Readiness(1).xlsx`; the 16-Nov term flexibility POC deck; manager's four-model + Monte Carlo suggestion; and the follow-up 14-/37-scenario planning discussion. The 14 and 37 counts are planning comparisons, not model counts or a promise of coverage. The 11-Dec-2026 federal demo is a *conditional proposed milestone*, not a promise of four fully measured lenses.
