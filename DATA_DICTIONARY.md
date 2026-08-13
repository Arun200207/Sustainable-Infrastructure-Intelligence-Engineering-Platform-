# Data Dictionary

## Sustainable Infrastructure Intelligence Platform

> **Data model, feature definitions, analytical domains, and semantic interpretation of the sustainability intelligence system.**

---

# 1. Purpose

This document defines the major data domains and feature categories used throughout the Sustainable Infrastructure Intelligence Platform.

The purpose is to provide a consistent reference for:

* Data interpretation
* Feature engineering
* Sustainability calculations
* Forecasting
* RAG knowledge generation
* Building-level analysis
* Reporting
* Model inference

The data dictionary focuses on the **semantic structure and engineering meaning** of the data rather than publishing the underlying dataset.

---

# 2. Authoritative Dataset Scope

The validated sustainability intelligence layer contains:

| Metric          |            Value |
| --------------- | ---------------: |
| Buildings       |        **1,578** |
| Daily Records   |    **1,153,518** |
| Total Energy    |  **24.587B kWh** |
| Net Carbon      | **8.391M tCO₂e** |
| Net Energy Cost |      **$2.705B** |

These values represent the authoritative project-level scope after validation and correction of historical sustainability calculations.

---

# 3. Data Architecture

The data can be conceptually divided into the following domains:

```text
┌─────────────────────────────────────────────┐
│              DATA FOUNDATION                │
├─────────────────────────────────────────────┤
│                                             │
│  Building Information                       │
│  Energy Measurements                        │
│  Weather / Environmental Variables          │
│  Temporal Information                       │
│                                             │
└───────────────────┬─────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│           DERIVED FEATURES                  │
├─────────────────────────────────────────────┤
│                                             │
│  Lag Features                               │
│  Rolling Features                           │
│  Degree-Day Features                        │
│  Temporal Features                          │
│                                             │
└───────────────────┬─────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│        SUSTAINABILITY INTELLIGENCE          │
├─────────────────────────────────────────────┤
│                                             │
│  Energy Intelligence                        │
│  Carbon Intelligence                        │
│  Cost Intelligence                          │
│  Sustainability Rankings                    │
│                                             │
└───────────────────┬─────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│            AI / ML CONSUMPTION               │
├─────────────────────────────────────────────┤
│                                             │
│  Knowledge Records                          │
│  Embeddings                                 │
│  Forecasting Features                       │
│  XAI Inputs                                 │
│                                             │
└─────────────────────────────────────────────┘
```

---

# 4. Entity Model

The primary analytical entity is the **building**.

A building can be represented conceptually as:

```text
Building
   │
   ├── Building Attributes
   │
   ├── Daily Energy Records
   │
   ├── Environmental Conditions
   │
   ├── Temporal Context
   │
   ├── Sustainability Metrics
   │
   ├── Rankings
   │
   ├── Forecasting Features
   │
   └── Knowledge Representation
```

The building is therefore the primary entity connecting the analytical, predictive, and generative layers.

---

# 5. Building-Level Data

Building-level information identifies and describes the infrastructure entities being analyzed.

Typical semantic categories include:

| Category               | Purpose                                             |
| ---------------------- | --------------------------------------------------- |
| Building Identifier    | Unique building reference                           |
| Building Attributes    | Descriptive infrastructure characteristics          |
| Building Category      | Groups buildings by relevant type                   |
| Location Context       | Provides geographic/environmental context           |
| Historical Association | Connects a building to its time-series observations |

The exact source fields should be interpreted according to the authoritative dataset used by the project.

---

# 6. Energy Data

Energy data represents building-level energy consumption over time.

### Primary concept

**Energy Consumption**

Represents the amount of energy associated with a building for a given observation period.

### Unit

**kWh**

### Analytical uses

Energy data is used for:

* Historical consumption analysis
* Sustainability calculations
* Cost calculations
* Carbon calculations
* Forecasting
* Lag features
* Rolling features
* Building ranking

---

# 7. Daily Records

The project contains:

**1,153,518 daily records**

A daily record conceptually represents:

```text
Building
+
Date
+
Energy
+
Environmental Context
+
Derived Features
```

The daily granularity provides the temporal basis for energy analysis and forecasting.

---

# 8. Temporal Data

Temporal variables provide the time context required for energy analysis.

Examples of semantic categories include:

* Date
* Day-related attributes
* Seasonal context
* Calendar-related variables
* Historical position

Temporal information enables the system to capture recurring energy behavior.

---

# 9. Temporal Features

Temporal features transform date information into model- and analysis-ready representations.

Their purpose is to help models distinguish between different temporal regimes.

Conceptually:

```text
Raw Date
   ↓
Temporal Representation
   ↓
Model / Analytics Feature
```

These features support both:

* Sustainability analysis
* Forecasting

---

# 10. Lag Features

Lag features represent historical observations relative to the current observation.

Conceptually:

```text
Energy(t)
Energy(t-1)
Energy(t-2)
...
```

Lag features provide the forecasting model with historical context.

They are particularly useful for capturing temporal dependency in energy consumption.

---

# 11. Rolling Features

Rolling features summarize recent historical behavior.

Conceptually:

```text
Historical Window
       ↓
Aggregation
       ↓
Rolling Feature
```

Depending on the configured window, rolling features can capture:

* Recent average behavior
* Short-term trends
* Local energy patterns
* Temporal smoothing

Rolling features are derived from historical observations and therefore require careful temporal ordering.

---

# 12. Weather and Environmental Variables

Environmental variables provide context for building energy demand.

Relevant categories may include:

* Temperature
* Weather conditions
* Environmental measurements
* Heating-related conditions
* Cooling-related conditions

Environmental variables are used to improve the representation of external factors affecting energy consumption.

---

# 13. Heating Degree Days

Heating Degree Days (HDD) represent temperature-related heating demand.

Conceptually:

```text
Temperature
     ↓
Heating Reference
     ↓
Heating Degree-Day Feature
```

HDD features provide a normalized representation of conditions associated with heating requirements.

---

# 14. Cooling Degree Days

Cooling Degree Days (CDD) represent temperature-related cooling demand.

Conceptually:

```text
Temperature
     ↓
Cooling Reference
     ↓
Cooling Degree-Day Feature
```

CDD features provide a normalized representation of conditions associated with cooling requirements.

---

# 15. Energy Intelligence

Energy intelligence is derived from the underlying consumption data.

It provides a higher-level representation of building energy behavior.

Potential analytical dimensions include:

* Total consumption
* Consumption trends
* Historical behavior
* Comparative building performance
* Temporal behavior
* Forecasted behavior

The energy intelligence layer serves as an upstream source for carbon, cost, ranking, forecasting, and reporting.

---

# 16. Carbon Intelligence

Carbon intelligence represents the environmental impact associated with energy activity.

The project calculates:

**Net Carbon = 8.391M tCO₂e**

The carbon layer is used for:

* Building-level carbon analysis
* Carbon ranking
* Combined sustainability ranking
* Sustainability reporting
* RAG knowledge generation

Carbon calculations were independently reviewed as part of the sustainability validation process.

---

# 17. Carbon Units

Carbon is represented using:

**tCO₂e — tonnes of carbon dioxide equivalent**

The metric provides a common representation for aggregated greenhouse-gas impact.

Interpretation should always consider the carbon-factor assumptions and methodology underlying the calculation.

---

# 18. Energy Cost Intelligence

Energy-cost intelligence represents the financial dimension of energy consumption.

The authoritative project-level value is:

**$2.705B net energy cost**

Cost intelligence supports:

* Building-level cost analysis
* Cost ranking
* Combined carbon/cost analysis
* Management reporting
* Sustainability recommendations

---

# 19. Cost Units

The project-level cost metric is represented in:

**USD ($)**

Cost interpretation depends on the underlying energy-price methodology and assumptions.

The cost layer should therefore be interpreted as calculated project intelligence rather than a universally applicable market price.

---

# 20. Sustainability Intelligence

Sustainability intelligence combines multiple analytical dimensions.

Conceptually:

```text
Energy
  +
Carbon
  +
Cost
  +
Temporal Context
  +
Environmental Context
       ↓
Sustainability Intelligence
```

This layer provides the primary analytical foundation for downstream AI components.

---

# 21. Building Sustainability Ranking

Buildings are ranked using sustainability-related metrics.

The project includes:

### Carbon Ranking

Compares buildings based on carbon-related performance.

### Cost Ranking

Compares buildings based on energy-cost performance.

### Combined Sustainability Ranking

Combines relevant sustainability dimensions into a broader comparative ranking.

Rankings are derived from validated upstream metrics.

---

# 22. Knowledge Records

The RAG knowledge layer contains:

**1,579 knowledge records**

Knowledge records transform structured sustainability information into natural-language-retrievable representations.

The records include two major conceptual categories:

```text
Global Sustainability Knowledge
            +
Building-Level Sustainability Knowledge
```

---

# 23. Global KPI Knowledge

Global knowledge records represent project-level sustainability information.

Examples of semantic information include:

* Total buildings
* Total energy
* Total carbon
* Total energy cost
* Aggregate sustainability metrics
* Project-level rankings or summaries

These records support questions concerning overall system performance.

---

# 24. Building-Level Knowledge

Building-level knowledge records represent individual infrastructure entities.

They can encode information such as:

* Building identity
* Energy behavior
* Sustainability metrics
* Carbon performance
* Cost performance
* Ranking information
* Relevant analytical context

These records support building-specific RAG queries.

---

# 25. Embedding Data

Knowledge records are transformed into semantic vectors.

### Embedding dimension

**384**

The conceptual transformation is:

```text
Knowledge Text
      ↓
Sentence Transformer
      ↓
384-Dimensional Vector
```

The resulting vectors are indexed by FAISS.

---

# 26. Vector Index

FAISS provides the vector retrieval layer.

The index connects:

```text
Knowledge Record
      ↕
Semantic Vector
      ↕
FAISS Index
```

The vector index enables semantic similarity search for RAG queries.

---

# 27. Forecasting Feature Domain

The production forecasting model uses a validated **13-feature input contract**.

The contract defines the required model inputs and their ordering.

Conceptually:

```text
13 Features
    ↓
Ordered Feature Matrix
    ↓
XGBoost
```

The exact feature names and ordering should be treated as part of the model contract and must remain synchronized with the serialized model.

---

# 28. Forecast Target

The forecasting system produces energy-related predictions using the validated XGBoost model.

The model's validated performance is:

| Metric |           Value |
| ------ | --------------: |
| MAE    | **6087.625294** |
| RMSE   | **8492.059022** |
| R²     |    **0.768148** |

These metrics represent the independently validated model evaluation associated with the reconstructed feature contract.

---

# 29. Explainability Data

The XAI layer consumes model behavior and feature information.

Two major explanation types are produced.

### Global Explanation

Provides an aggregate view of feature influence.

### Local Explanation

Provides explanation for an individual prediction.

SHAP is used to generate these contribution-based explanations.

---

# 30. Reporting Data

The reporting layer consumes information from:

```text
Sustainability Intelligence
        +
Forecasting
        +
XAI
        +
RAG / Knowledge Layer
```

It generates information for:

* Global reports
* Management summaries
* Recommendations
* Machine-readable JSON
* Human-readable Markdown
* Building-level reports

The system generated and validated:

**1,578 building-level reports**

---

# 31. Inference Data

The production inference layer receives a validated feature representation.

Conceptually:

```text
Request Input
     ↓
Validation
     ↓
13-Feature Contract
     ↓
Model
     ↓
Prediction
```

The production inference workflow generated:

**110 validated predictions**

---

# 32. Data Quality Principles

The data architecture follows several quality principles.

## Validation Before Authority

Data should be validated before being treated as authoritative.

## Temporal Integrity

Historical features must not introduce inappropriate future information.

## Contract Consistency

Model features must match the serialized model contract.

## Derived Artifact Integrity

Downstream artifacts must remain synchronized with upstream corrections.

## Traceability

Important analytical outputs should be traceable to their originating data layer.

---

# 33. Data Dependency Graph

The primary dependency graph is:

```text
Building Data
      │
      ├──────────────┐
      │              │
      ▼              ▼
Energy Data      Weather Data
      │              │
      └──────┬───────┘
             ▼
     Feature Engineering
             │
     ┌───────┴────────┐
     ▼                ▼
Sustainability    Forecasting
Intelligence          │
     │                ▼
     │             XGBoost
     │                │
     ▼                ▼
Knowledge            SHAP
     │
     ▼
 Embeddings
     │
     ▼
   FAISS
     │
     ▼
   RAG + LLM
     │
     └──────────┐
                ▼
             Reporting
```

---

# 34. Data Lineage

The system follows this conceptual lineage:

```text
Source Data
    ↓
Validated Data
    ↓
Derived Features
    ↓
Sustainability Metrics
    ↓
Knowledge Representation
    ↓
Semantic Representation
    ↓
AI / ML Outputs
    ↓
Reports / Dashboard / API
```

Each stage is downstream of the previous stage and should not silently diverge from the authoritative source.

---

# 35. Stale Artifact Handling

A key data-engineering issue identified during development was downstream artifact staleness.

The sequence was:

```text
Historical Sustainability Calculation
             ↓
Correction
             ↓
Authoritative Sustainability Layer
             ↓
Existing RAG Artifacts Become Potentially Stale
             ↓
RAG Artifacts Rebuilt
             ↓
Cross-Layer Validation
```

This demonstrates why derived AI artifacts require dependency-aware regeneration.

---

# 36. Data Scale Summary

The project operates at a meaningful analytical scale:

```text
1,578 Buildings
        ×
1,153,518 Daily Records
        ↓
Large-Scale Sustainability Intelligence
        ↓
1,579 Knowledge Records
        ↓
384-D Embedding Space
        ↓
RAG Retrieval
        +
ML Forecasting
```

The project therefore involves more than a small toy dataset or isolated model-training table.

---

# 37. Semantic Data Layers

The platform can be viewed as four progressively abstract data layers.

### Layer 1 — Observational Data

What happened?

```text
Energy
Weather
Building
Time
```

### Layer 2 — Derived Data

What patterns can be calculated?

```text
Lags
Rolling Features
Degree Days
Temporal Features
```

### Layer 3 — Intelligence

What does the data mean?

```text
Carbon
Cost
Sustainability
Rankings
Forecasts
```

### Layer 4 — Decision Intelligence

What can users interact with?

```text
RAG Answers
Recommendations
Reports
Dashboard
API Predictions
```

---

# 38. Data Governance Considerations

A production implementation should additionally address:

* Data ownership
* Data provenance
* Data retention
* Access control
* Personally identifiable information handling
* Data versioning
* Schema evolution
* Quality monitoring
* Drift detection
* Auditability

The current project establishes the analytical and engineering foundation for these future governance requirements.

---

# 39. Known Data Limitations

The interpretation of the data is subject to several limitations.

### Historical Dependence

The system relies on historical observations for analytics and forecasting.

### External Validity

Results may not generalize to infrastructure populations with substantially different characteristics.

### Carbon Assumptions

Carbon results depend on the methodology and emission factors used.

### Cost Assumptions

Energy-cost intelligence depends on the project's pricing assumptions.

### Environmental Coverage

Weather and environmental variables may not capture every factor influencing building energy demand.

---

# 40. Recommended Interpretation

The data should be interpreted hierarchically:

```text
Observation
    ↓
Derived Feature
    ↓
Metric
    ↓
Ranking / Prediction
    ↓
Decision Intelligence
```

A downstream metric should not automatically be interpreted as causal evidence.

For example:

> A feature strongly associated with energy consumption does not necessarily imply that changing that feature will causally reduce energy consumption.

This distinction is particularly important when using model explanations for sustainability decisions.

---

# 41. Data-to-AI Mapping

| Data Domain           | AI / ML Consumer                  |
| --------------------- | --------------------------------- |
| Building Data         | Sustainability + Forecasting      |
| Energy Data           | Sustainability + Forecasting      |
| Weather Data          | Feature Engineering + Forecasting |
| Temporal Data         | Feature Engineering + Forecasting |
| Lag Features          | Forecasting                       |
| Rolling Features      | Forecasting                       |
| Degree-Day Features   | Forecasting + Sustainability      |
| Carbon Metrics        | RAG + Reporting                   |
| Cost Metrics          | RAG + Reporting                   |
| Rankings              | RAG + Dashboard + Reporting       |
| Knowledge Records     | RAG                               |
| Embeddings            | FAISS                             |
| Forecast Features     | XGBoost                           |
| Model Contributions   | SHAP                              |
| Combined Intelligence | Reporting + Dashboard             |

---

# 42. Data Contract Principles

The project uses explicit contracts for important downstream consumers.

### Sustainability Contract

Validated calculations define authoritative sustainability values.

### Knowledge Contract

Knowledge records represent the validated sustainability layer.

### Model Contract

The forecasting model requires exactly the expected feature structure.

### Inference Contract

Production requests must satisfy the model's input requirements.

### API Contract

The FastAPI interface defines the external prediction boundary.

---

# 43. Data Dictionary Summary

The platform's data architecture can be summarized as:

```text
                    DATA
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
   Building        Energy       Weather
       │             │             │
       └─────────────┼─────────────┘
                     ▼
             Temporal Features
                     │
       ┌─────────────┴─────────────┐
       ▼                           ▼
Derived Features            Sustainability
                             Intelligence
       │                           │
       ▼                           ▼
 Forecasting                 Knowledge Layer
       │                           │
       ▼                           ▼
    XGBoost                    Embeddings
       │                           │
       ▼                           ▼
      SHAP                       FAISS
                                   │
                                   ▼
                                RAG + LLM
```

The resulting architecture creates a continuous path from **observational infrastructure data to analytical, predictive, explanatory, and generative intelligence**.

---

# 44. Final Data Engineering Principle

The central data-engineering principle of the project is:

> **Authoritative data should be validated before downstream intelligence is generated, and every derived AI artifact should remain traceable to the data from which it was created.**

This principle supports the project's goals of reproducibility, consistency, explainability, and production-oriented AI engineering.
