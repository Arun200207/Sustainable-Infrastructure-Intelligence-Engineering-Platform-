# System Design

## Sustainable Infrastructure Intelligence Platform

> **Technical system design covering data flow, component boundaries, contracts, validation, intelligence layers, inference, and deployment.**

---

# 1. Purpose

This document defines the technical design of the Sustainable Infrastructure Intelligence Platform.

The system is designed to transform large-scale infrastructure and energy data into:

* Sustainability intelligence
* Building-level rankings
* Natural-language sustainability intelligence
* Energy forecasts
* Explainable predictions
* Automated reports
* Interactive analytics
* Production inference services

The design emphasizes:

* Separation of concerns
* Explicit data and model contracts
* Artifact lineage
* Validation
* Reproducibility
* Explainability
* Grounded generative AI
* Production-oriented interfaces

---

# 2. System Objectives

The system is designed to satisfy five major objectives.

## Objective 1 — Analyze Infrastructure Sustainability

Convert building-level energy and environmental information into validated sustainability metrics.

## Objective 2 — Provide Natural-Language Intelligence

Allow users to ask sustainability questions through a grounded RAG + LLM interface.

## Objective 3 — Forecast Energy Behavior

Use a validated XGBoost model to generate energy predictions.

## Objective 4 — Explain Predictions

Use feature importance and SHAP to provide model-level and prediction-level explanations.

## Objective 5 — Operationalize Intelligence

Expose validated capabilities through:

* Dashboard
* Automated reports
* MLflow
* FastAPI
* Docker

---

# 3. Design Philosophy

The platform follows an **artifact-first architecture**.

Instead of treating the project as a single notebook or monolithic application, each major stage produces a validated artifact consumed by downstream stages.

```text id="6e8k7k"
Input
  ↓
Transformation
  ↓
Artifact
  ↓
Validation
  ↓
Downstream Consumer
```

A downstream component should not assume that an upstream artifact is correct merely because it exists.

---

# 4. Logical Components

The platform consists of the following logical components:

```text id="n6wmq1"
1. Data Foundation
2. Feature Engineering
3. Sustainability Intelligence
4. Knowledge Engineering
5. Vector Retrieval
6. RAG + LLM
7. Forecasting
8. Explainability
9. Automated Reporting
10. Dashboard
11. Inference
12. MLflow
13. FastAPI
14. Deployment
15. Validation
```

---

# 5. Component Responsibility Matrix

| Component                   | Primary Responsibility                            |
| --------------------------- | ------------------------------------------------- |
| Data Foundation             | Provide authoritative input data                  |
| Feature Engineering         | Create analytical/model features                  |
| Sustainability Intelligence | Calculate domain-level sustainability metrics     |
| Knowledge Engineering       | Convert intelligence into retrieval-ready records |
| Embedding Layer             | Generate semantic representations                 |
| FAISS                       | Perform vector similarity retrieval               |
| RAG                         | Retrieve relevant knowledge                       |
| LLM                         | Generate grounded natural-language responses      |
| XGBoost                     | Generate energy forecasts                         |
| SHAP                        | Explain predictive behavior                       |
| Reporting                   | Convert intelligence into structured reports      |
| Streamlit                   | Provide interactive user interface                |
| MLflow                      | Track experiments and inference                   |
| Inference Pipeline          | Execute validated model prediction                |
| FastAPI                     | Expose inference through REST                     |
| Docker                      | Package the production service                    |
| Validation                  | Verify correctness across boundaries              |

---

# 6. Data Flow

The primary data flow is:

```text id="qf6qgl"
Building + Energy + Environmental Data
                  ↓
         Data Validation
                  ↓
        Feature Engineering
                  ↓
     Sustainability Master Dataset
                  ↓
       Sustainability Intelligence
                  ↓
       ┌──────────┴──────────┐
       ↓                     ↓
 Knowledge Layer       Forecasting Layer
       ↓                     ↓
 Embeddings             XGBoost
       ↓                     ↓
    FAISS                  SHAP
       ↓                     ↓
 RAG + LLM                  │
       └──────────┬──────────┘
                  ↓
         Automated Reporting
                  ↓
        ┌─────────┴─────────┐
        ↓                   ↓
    Dashboard          Production API
                            ↓
                         Docker
```

---

# 7. Data Foundation Design

## 7.1 Authoritative Scope

The validated sustainability dataset contains:

```text id="4z9oyt"
Buildings:       1,578
Daily Records:   1,153,518
Energy:          24.587B kWh
Net Carbon:      8.391M tCO₂e
Net Energy Cost: $2.705B
```

These values represent the authoritative scale of the project's sustainability intelligence layer.

---

# 8. Feature Engineering Design

Feature engineering serves two purposes:

1. Domain-level sustainability analysis
2. Machine-learning inference

The system derives multiple categories of features.

## Temporal Features

Represent recurring time behavior.

## Lag Features

Represent historical energy behavior.

## Rolling Features

Represent local historical trends.

## Weather-Derived Features

Capture environmental conditions relevant to energy demand.

## Degree-Day Features

Represent heating and cooling requirements.

---

# 9. Feature Contract

The forecasting model requires exactly **13 model features**.

The feature contract defines:

```text id="y8txc1"
Feature Identity
Feature Order
Feature Shape
Feature Type
Model Compatibility
```

The feature contract is treated as an interface between feature engineering and model inference.

Conceptually:

```text id="f7p8b1"
Feature Engineering
       │
       ▼
┌───────────────────────┐
│ Feature Contract      │
│                       │
│ 13 Required Features  │
│ Exact Ordering        │
└──────────┬────────────┘
           │
           ▼
      XGBoost Model
```

A model input with the correct values but incorrect ordering is considered invalid.

---

# 10. Sustainability Intelligence Design

The sustainability layer operates above the raw data layer.

It produces:

### Energy Intelligence

Building-level energy behavior and consumption metrics.

### Carbon Intelligence

Carbon-related sustainability metrics.

### Cost Intelligence

Energy-cost metrics.

### Combined Intelligence

A combined view supporting comparative sustainability analysis.

### Ranking

Buildings can be ranked according to:

* Carbon performance
* Cost performance
* Combined sustainability performance

---

# 11. Numerical Integrity

The sustainability layer is considered authoritative only after validation.

The engineering workflow explicitly included correction of incorrect historical carbon and cost calculations.

After correction:

```text id="m0e1fl"
Corrected Sustainability Layer
             ↓
Independent Validation
             ↓
Authoritative Intelligence
```

Downstream artifacts derived from corrected values must therefore be regenerated or validated for consistency.

---

# 12. Knowledge Engineering

The knowledge layer converts structured sustainability intelligence into records suitable for semantic retrieval.

The validated knowledge base contains:

**1,579 knowledge records**

These include:

* Global sustainability knowledge
* Building-level sustainability knowledge

The knowledge layer acts as an abstraction boundary between numerical analytics and natural-language intelligence.

```text id="7s6n7q"
Structured Sustainability Intelligence
                  ↓
           Knowledge Records
                  ↓
          Semantic Representation
```

---

# 13. Embedding System

The platform uses Sentence Transformers to convert knowledge records into semantic vectors.

```text id="cmq3mb"
Knowledge Record
       ↓
Sentence Transformer
       ↓
384-Dimensional Embedding
       ↓
FAISS Index
```

The embedding layer enables semantic retrieval rather than exact keyword matching.

---

# 14. Vector Retrieval Design

FAISS provides the vector-search layer.

Conceptually:

```text id="3y2q5b"
User Query
    ↓
Query Embedding
    ↓
FAISS Similarity Search
    ↓
Relevant Knowledge Records
```

The retrieval layer is responsible for identifying relevant project knowledge before generation.

---

# 15. RAG Design

The RAG pipeline consists of four conceptual stages.

## Stage 1 — Query

The user submits a sustainability question.

## Stage 2 — Retrieval

The query is transformed into a semantic representation and compared against the knowledge index.

## Stage 3 — Context Construction

Relevant knowledge records are assembled into generation context.

## Stage 4 — Generation

The transformer-based LLM generates a response using retrieved context.

```text id="g7ykpf"
Question
   ↓
Embedding
   ↓
Retrieval
   ↓
Context
   ↓
LLM
   ↓
Answer
```

---

# 16. RAG Consistency Design

RAG artifacts are downstream of the sustainability intelligence layer.

Therefore:

```text id="v1tqgm"
Sustainability Calculation
          ↓
Knowledge Record
          ↓
Embedding
          ↓
FAISS
          ↓
RAG
```

A change at the first stage can invalidate all later stages.

The project addressed this dependency by rebuilding stale RAG artifacts after the underlying sustainability calculations were corrected.

This is treated as an **artifact lineage problem**, not merely a model problem.

---

# 17. Forecasting System

The forecasting system consumes the validated 13-feature input contract.

```text id="r9ef2x"
Validated Features
       ↓
Feature Contract
       ↓
XGBoost
       ↓
Prediction
```

The model workflow includes:

* Serialized model inspection
* Feature contract reconstruction
* Input matrix construction
* Exact ordering preservation
* Prediction generation
* Prediction finiteness validation
* Metric calculation

---

# 18. Forecasting Validation

Validated model performance:

| Metric |       Value |
| ------ | ----------: |
| MAE    | 6087.625294 |
| RMSE   | 8492.059022 |
| R²     |    0.768148 |

The model outputs were independently tested rather than relying exclusively on a previously generated forecast artifact.

---

# 19. Explainability System

The explainability layer receives model predictions and provides interpretation.

```text id="h3z4tv"
XGBoost Prediction
       ↓
 ┌─────┴──────┐
 ↓            ↓
Feature       SHAP
Importance
 ↓            ↓
Global      Global + Local
Importance  Explanations
```

The distinction between model importance and SHAP contribution is preserved.

Feature importance indicates model-level relevance.

SHAP provides contribution-oriented explanations for individual predictions and aggregated behavior.

---

# 20. Automated Reporting System

The reporting layer consumes outputs from multiple system components.

```text id="u8twk4"
Sustainability
      +
RAG
      +
Forecasting
      +
XAI
      ↓
Reporting Engine
      ↓
┌──────────────────────┐
│ Global Report        │
│ Management Summary   │
│ Recommendations      │
│ JSON Report          │
│ Markdown Report      │
│ Building Reports     │
└──────────────────────┘
```

The system generated and validated **1,578 building-level reports**.

---

# 21. Dashboard Design

The dashboard is an application layer over validated intelligence.

Its responsibilities include:

* Visualization
* Filtering
* Building search
* Ranking exploration
* Forecast visualization
* XAI presentation
* RAG interaction
* Report access

The dashboard should not become the authoritative source of data.

Instead:

```text id="m2qmbk"
Authoritative Artifacts
        ↓
Application Layer
        ↓
Visualization
```

This preserves separation between computation and presentation.

---

# 22. Production Inference Design

The inference pipeline is intentionally lightweight.

It does not reproduce the entire development workflow during every prediction.

Instead:

```text id="kaxj9f"
Validated Model
      +
Validated Feature Contract
      ↓
Lightweight Inference Pipeline
      ↓
Prediction
```

This reduces unnecessary runtime dependencies and makes the serving boundary easier to reason about.

---

# 23. Inference Validation

The inference pipeline generated:

**110 predictions**

The predictions were validated against the expected model input contract and inference behavior.

The production inference workflow therefore provides a reproducible path from:

```text id="t6c4l9"
Input
 ↓
Validation
 ↓
Feature Contract
 ↓
Model
 ↓
Prediction
```

---

# 24. MLflow Design

MLflow provides experiment tracking around the inference/model workflow.

Tracked information includes:

* Parameters
* Metrics
* Inference artifacts
* Experiment metadata

The backend was migrated to SQLite for persistent local experiment tracking.

Conceptually:

```text id="8t9d4u"
Model / Inference Workflow
          ↓
       MLflow
          ↓
┌────────────────────────┐
│ Parameters             │
│ Metrics                │
│ Artifacts              │
│ Experiment Information │
└────────────────────────┘
```

---

# 25. API Design

FastAPI provides the external service boundary.

## Health Endpoint

```text id="2kh1n7"
GET /health
```

Purpose:

* Service health verification
* Deployment validation
* Basic availability testing

## Prediction Endpoint

```text id="e3c0o1"
POST /predict
```

Purpose:

* Accept validated model input
* Execute production inference
* Return prediction results

---

# 26. API Request Flow

```text id="5rxm1h"
Client
  ↓
HTTP Request
  ↓
FastAPI
  ↓
Request Validation
  ↓
13-Feature Contract
  ↓
Inference Pipeline
  ↓
XGBoost
  ↓
Prediction
  ↓
HTTP Response
```

This establishes a clean separation between the client and model implementation.

---

# 27. Deployment Design

The production service is containerized using Docker.

The deployment boundary is:

```text id="kz9gjc"
Application Configuration
        ↓
Dependencies
        ↓
Docker Image
        ↓
Container
        ↓
FastAPI
        ↓
Inference
```

The Docker layer is intended to improve environment consistency and simplify deployment.

---

# 28. Configuration Design

Environment-specific configuration should be externalized rather than hard-coded.

The repository therefore supports an environment configuration pattern using:

```text id="4h4ck7"
.env.example
```

Secrets and environment-specific values should not be committed to the repository.

---

# 29. Validation Architecture

Validation exists at multiple system boundaries.

```text id="cn2m2j"
Data
 ↓
Validation
 ↓
Feature Engineering
 ↓
Validation
 ↓
Sustainability Intelligence
 ↓
Validation
 ↓
Knowledge
 ↓
Validation
 ↓
RAG
 ↓
Validation
 ↓
Forecasting
 ↓
Validation
 ↓
Inference
 ↓
Validation
 ↓
API
 ↓
Deployment
```

This reduces the probability that an upstream error silently propagates through the complete system.

---

# 30. Artifact Lineage

The system maintains logical lineage between artifacts.

## Sustainability Branch

```text id="v4k6m9"
Source Data
   ↓
Master Dataset
   ↓
Sustainability Metrics
   ↓
Rankings
```

## RAG Branch

```text id="o8m6p1"
Sustainability Metrics
   ↓
Knowledge Records
   ↓
Embeddings
   ↓
FAISS
   ↓
RAG
```

## Forecasting Branch

```text id="c7n2qa"
Features
   ↓
13-Feature Contract
   ↓
XGBoost
   ↓
Inference
```

## Explanation Branch

```text id="u3r8zy"
XGBoost
   ↓
Feature Importance
   +
SHAP
```

## Reporting Branch

```text id="3p7f6h"
Sustainability
   +
RAG
   +
Forecasting
   +
XAI
   ↓
Reports
```

---

# 31. Failure and Integrity Considerations

The architecture recognizes several classes of failure.

## Data Failure

Incorrect or incomplete source data can contaminate all downstream calculations.

### Mitigation

Validation before creating authoritative intelligence.

---

## Feature Failure

Missing, reordered, or incorrectly typed features can invalidate model inference.

### Mitigation

Explicit 13-feature model contract.

---

## Artifact Staleness

Upstream corrections can make downstream artifacts obsolete.

### Mitigation

Artifact lineage and downstream rebuild/validation.

---

## RAG Failure

Retrieval can return incomplete or irrelevant context.

### Mitigation

Validated knowledge base and cross-layer retrieval testing.

---

## LLM Failure

The LLM may generate unsupported statements.

### Mitigation

Retrieval-grounded generation and knowledge-layer validation.

---

## Model Failure

Predictions may be inaccurate even when the model executes correctly.

### Mitigation

MAE, RMSE, R², feature analysis, and SHAP-based evaluation.

---

## Deployment Failure

The API may be unavailable or incorrectly configured.

### Mitigation

Health endpoint, request validation, deployment validation, and containerized configuration.

---

# 32. Reproducibility Strategy

Reproducibility is supported through:

* Defined project structure
* Explicit dependencies
* Feature contracts
* Model contracts
* Machine-readable validation
* MLflow tracking
* Environment configuration
* Docker packaging
* Artifact lineage

The goal is to make the engineering workflow understandable and repeatable rather than dependent on undocumented notebook state.

---

# 33. Separation Between Development and Production

The system distinguishes between:

### Development

* Data analysis
* Feature engineering
* Model inspection
* Validation
* Experimentation

### Production Inference

* Input validation
* Feature contract enforcement
* Model loading
* Prediction
* API response

The production inference path is intentionally smaller than the complete development environment.

```text id="9n2u8q"
Development System
       │
       │ validated model + contract
       ▼
Production Inference
       ↓
FastAPI
       ↓
Docker
```

---

# 34. Security Considerations

The current project is a portfolio-grade deployment architecture rather than a hardened enterprise production system.

A production deployment would require additional controls including:

* Authentication
* Authorization
* Secret management
* HTTPS/TLS
* Rate limiting
* Input abuse protection
* Dependency scanning
* Container security scanning
* Network controls
* Audit logging

These are identified as future production hardening requirements.

---

# 35. Scalability Considerations

The current architecture is designed for extensibility.

Potential scaling directions include:

```text id="w7pq6t"
Current
Local / Containerized Inference
          ↓
Future
Cloud Inference Service
          ↓
Load Balancing
          ↓
Horizontal Scaling
          ↓
Monitoring
          ↓
Automated Retraining
```

For larger deployments, the platform could separate:

* Data processing
* Feature computation
* Model serving
* Vector retrieval
* LLM generation
* Reporting
* Monitoring

into independently scalable services.

---

# 36. Future System Evolution

The architecture can be extended toward a real-time sustainability platform.

Potential future architecture:

```text id="2a0r5e"
IoT / Building Systems
        ↓
Streaming Ingestion
        ↓
Real-Time Feature Store
        ↓
Sustainability Intelligence
        ↓
┌─────────────┬─────────────┬──────────────┐
│ Forecasting │ RAG + LLM   │ Optimization │
└─────────────┴─────────────┴──────────────┘
        ↓
Decision Intelligence
        ↓
API / Dashboard / Alerts
```

Additional future capabilities could include:

* Real-time energy monitoring
* Automated anomaly detection
* Probabilistic forecasting
* Sustainability optimization
* Prescriptive recommendations
* Model monitoring
* Data drift detection
* Automated retraining
* RAG evaluation
* LLM response evaluation

---

# 37. Engineering Trade-Offs

## FAISS

FAISS provides efficient local vector retrieval and is appropriate for the project's current scale.

A larger production deployment could use a managed or distributed vector database depending on operational requirements.

## XGBoost

XGBoost provides strong tabular predictive performance with relatively straightforward inference.

More complex temporal architectures could be evaluated in future work.

## Streamlit

Streamlit enables rapid delivery of an interactive analytical interface.

A larger enterprise application could separate frontend and backend services.

## SQLite MLflow Backend

SQLite provides a lightweight persistent tracking backend suitable for the current project.

A production multi-user deployment would typically require a more robust database configuration.

## Docker

Docker provides environment consistency and reproducible packaging.

A production platform could later move toward Kubernetes or managed container infrastructure.

---

# 38. Design Principles Summary

The system is built around the following principles:

1. **Authoritative data before downstream intelligence**
2. **Explicit contracts between components**
3. **Validation at system boundaries**
4. **No silent modification of validated artifacts**
5. **Traceable artifact lineage**
6. **Grounded generative AI**
7. **Explainable predictive modeling**
8. **Separation of development and inference**
9. **Reproducible deployment**
10. **Clear extension paths toward production scale**

---

# 39. Final System View

The complete system can be summarized as:

```text id="m5i1gq"
                         DATA
                          │
                          ▼
                 FEATURE ENGINEERING
                          │
                          ▼
             SUSTAINABILITY INTELLIGENCE
                          │
             ┌────────────┴────────────┐
             │                         │
             ▼                         ▼
       KNOWLEDGE LAYER             ML MODEL
             │                         │
             ▼                         ▼
        EMBEDDINGS                 INFERENCE
             │                         │
             ▼                         ▼
           FAISS                    SHAP
             │                         │
             ▼                         │
         RAG + LLM                     │
             │                         │
             └────────────┬────────────┘
                          ▼
                   REPORTING LAYER
                          │
             ┌────────────┴────────────┐
             ▼                         ▼
        STREAMLIT                  PRODUCTION API
                                       │
                                       ▼
                                    MLFLOW
                                       │
                                       ▼
                                     DOCKER
```

---

# 40. Conclusion

The Sustainable Infrastructure Intelligence Platform is architected as an integrated AI engineering system rather than a standalone machine-learning experiment.

Its primary engineering contribution is the integration of:

**Data Engineering + Sustainability Analytics + RAG + LLM + Predictive ML + XAI + Reporting + Dashboard + MLOps + Production Inference + API + Deployment**

The architecture maintains explicit boundaries between these capabilities while preserving lineage from authoritative data through downstream intelligence and production interfaces.

The resulting system provides a foundation for evolving from a validated M.Tech-level AI engineering project toward a more comprehensive real-time sustainability intelligence platform.
