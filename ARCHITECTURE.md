# System Architecture

## Sustainable Infrastructure Intelligence Platform

> **End-to-End AI/ML Architecture for Sustainable Infrastructure Intelligence**

---

## 1. Architecture Overview

The Sustainable Infrastructure Intelligence Platform is designed as a layered AI engineering system that transforms infrastructure, energy, environmental, and temporal data into validated sustainability intelligence and production-accessible AI services.

The architecture follows this progression:

```text
Data
  ↓
Validation & Feature Engineering
  ↓
Sustainability Intelligence
  ↓
Knowledge Engineering
  ↓
RAG + LLM
  ↓
Forecasting
  ↓
Explainable AI
  ↓
Automated Reporting
  ↓
Interactive Application
  ↓
MLOps & Production Inference
  ↓
API & Deployment
```

The architecture intentionally separates analytical intelligence, generative intelligence, predictive intelligence, explainability, presentation, and production inference.

---

# 2. High-Level System Architecture

```text
┌───────────────────────────────────────────────────────────────────────┐
│                        DATA FOUNDATION                                │
│                                                                       │
│  Building Data │ Energy Data │ Weather Data │ Environmental Data     │
│                         │                                             │
│                         ▼                                             │
│              Validation + Feature Engineering                         │
└─────────────────────────┬─────────────────────────────────────────────┘
                          │
                          ▼
┌───────────────────────────────────────────────────────────────────────┐
│                 SUSTAINABILITY INTELLIGENCE                           │
│                                                                       │
│  Energy Intelligence │ Carbon Intelligence │ Cost Intelligence       │
│  Temporal Features   │ Lag/Rolling Features │ Degree Days           │
│                                                                       │
│  Building Analytics → Carbon Ranking → Cost Ranking                  │
│                         ↓                                             │
│                 Combined Sustainability Ranking                       │
└─────────────────────────┬─────────────────────────────────────────────┘
                          │
              ┌───────────┴───────────┐
              │                       │
              ▼                       ▼
┌───────────────────────────┐   ┌───────────────────────────────┐
│ KNOWLEDGE ENGINEERING     │   │ PREDICTIVE ML                 │
│                           │   │                               │
│ 1,579 Knowledge Records   │   │ XGBoost Forecasting           │
│ Sentence Transformers     │   │                               │
│ 384-D Embeddings          │   │ 13-Feature Contract           │
│ FAISS Vector Store        │   │                               │
└─────────────┬─────────────┘   └───────────────┬───────────────┘
              │                                 │
              ▼                                 ▼
┌───────────────────────────┐   ┌───────────────────────────────┐
│ RAG + LLM                 │   │ EXPLAINABLE AI                │
│                           │   │                               │
│ Semantic Retrieval        │   │ XGBoost Importance             │
│ Context Construction      │   │ SHAP Global Explanations      │
│ Grounded Generation       │   │ SHAP Local Explanations       │
└─────────────┬─────────────┘   └───────────────┬───────────────┘
              │                                 │
              └──────────────┬──────────────────┘
                             ▼
┌───────────────────────────────────────────────────────────────────────┐
│                      APPLICATION INTELLIGENCE                          │
│                                                                       │
│ Automated Reports │ Management Summaries │ Recommendations           │
│ JSON Reports      │ Markdown Reports     │ Building Reports          │
└─────────────────────────────┬─────────────────────────────────────────┘
                              │
                ┌─────────────┴─────────────┐
                │                           │
                ▼                           ▼
┌──────────────────────────┐     ┌────────────────────────────────────┐
│ STREAMLIT APPLICATION     │     │ PRODUCTION INFERENCE              │
│                          │     │                                    │
│ KPI Dashboard            │     │ MLflow Experiment Tracking         │
│ Building Search          │     │ Lightweight Inference Pipeline     │
│ Sustainability Analysis  │     │ FastAPI REST Service               │
│ Forecast Visualization    │     │ Request Validation                 │
│ XAI Visualization        │     │ Docker Deployment                   │
│ RAG Assistant            │     │                                    │
└──────────────────────────┘     └────────────────────────────────────┘
```

---

# 3. Architectural Layers

## Layer 1 — Data Foundation

The data foundation provides the raw and derived information required by all downstream intelligence layers.

### Primary data domains

* Building-level infrastructure information
* Energy consumption
* Weather/environmental variables
* Temporal information
* Historical energy behavior

### Responsibilities

* Data ingestion
* Data normalization
* Data validation
* Feature generation
* Temporal alignment
* Historical feature construction

The validated sustainability layer contains:

* **1,578 buildings**
* **1,153,518 daily records**

---

# 4. Layer 2 — Feature Engineering

Feature engineering transforms raw observations into model- and analytics-ready representations.

The platform incorporates:

### Temporal features

Examples include calendar and time-based representations required for modeling recurring energy behavior.

### Lag features

Historical energy observations are transformed into lag-based predictors.

### Rolling features

Rolling windows capture recent energy-consumption behavior and temporal trends.

### Heating/Cooling Degree Days

Degree-day features provide weather-normalized representations of heating and cooling demand.

### Feature Contract

The production forecasting workflow explicitly defines a **13-feature input contract**.

The feature contract is treated as an engineering interface.

Therefore:

```text
Feature Name
      +
Feature Order
      +
Feature Shape
      +
Feature Type
      ↓
Model Input Contract
```

must remain consistent between inference and the serialized model.

---

# 5. Layer 3 — Sustainability Intelligence

The sustainability intelligence layer converts energy and environmental observations into higher-level infrastructure intelligence.

## Energy Intelligence

Provides building-level energy consumption analysis.

## Carbon Intelligence

Converts energy activity into carbon-related metrics using the project's validated calculation methodology.

## Cost Intelligence

Calculates energy-related cost intelligence.

## Building Sustainability Intelligence

Combines relevant metrics into building-level sustainability analysis.

## Ranking

The platform provides:

```text
Carbon Ranking
      +
Cost Ranking
      +
Combined Sustainability Ranking
```

This transforms raw measurements into decision-oriented intelligence.

---

# 6. Layer 4 — Knowledge Engineering

The knowledge layer converts structured sustainability intelligence into retrieval-ready knowledge records.

The validated knowledge layer contains:

**1,579 knowledge records**

These records represent sustainability information at different levels, including:

* Global sustainability KPIs
* Building-level sustainability intelligence

The architecture intentionally separates:

```text
Structured Data
      ↓
Sustainability Intelligence
      ↓
Knowledge Representation
      ↓
Semantic Retrieval
```

This allows the generative AI layer to operate on validated project knowledge rather than directly generating answers from raw data.

---

# 7. Layer 5 — Embedding and Vector Search

The knowledge records are transformed into semantic embeddings using Sentence Transformers.

### Embedding specification

```text
Embedding Dimension: 384
```

The embeddings are indexed using FAISS.

Architecture:

```text
Knowledge Record
      ↓
Sentence Transformer
      ↓
384-Dimensional Vector
      ↓
FAISS Index
      ↓
Nearest-Neighbor Retrieval
```

FAISS provides the semantic retrieval layer required by the RAG pipeline.

---

# 8. Layer 6 — RAG + LLM Intelligence

The RAG system combines semantic retrieval with transformer-based language generation.

### RAG pipeline

```text
User Question
      ↓
Question Representation
      ↓
Vector Retrieval
      ↓
FAISS
      ↓
Relevant Sustainability Knowledge
      ↓
Context Construction
      ↓
Transformer-Based LLM
      ↓
Grounded Response
```

The architecture supports two primary query categories:

### Global Questions

Questions about overall sustainability performance and project-level KPIs.

### Building-Level Questions

Questions targeting individual building sustainability characteristics.

---

# 9. RAG Grounding Strategy

The system follows a retrieval-first approach.

The LLM is not treated as the primary source of sustainability facts.

Instead:

```text
Validated Knowledge
       ↓
Retrieval
       ↓
Context
       ↓
Generation
```

This reduces the risk of unsupported answers and provides a clearer relationship between the underlying sustainability intelligence and generated responses.

---

# 10. Cross-Layer Consistency

A critical engineering requirement is consistency between upstream analytical artifacts and downstream AI artifacts.

The dependency chain is:

```text
Sustainability Calculations
          ↓
Knowledge Base
          ↓
Embeddings
          ↓
FAISS Index
          ↓
RAG Retrieval
          ↓
LLM Response
```

If an upstream numerical calculation changes, downstream knowledge artifacts can become stale.

The project explicitly encountered and addressed this class of issue by rebuilding stale RAG artifacts after corrections to the Phase-09 sustainability calculations.

This demonstrates an important principle:

> **Derived AI artifacts must remain synchronized with their authoritative upstream data sources.**

---

# 11. Layer 7 — Predictive Intelligence

The forecasting layer uses an XGBoost model for energy prediction.

The production workflow includes:

```text
Serialized XGBoost Model
          ↓
Feature Contract Inspection
          ↓
13 Required Features
          ↓
Feature Matrix Reconstruction
          ↓
Exact Feature Ordering
          ↓
Prediction
          ↓
Metric Validation
```

The validated evaluation metrics are:

```text
MAE  = 6087.625294
RMSE = 8492.059022
R²   = 0.768148
```

---

# 12. Model Contract Architecture

The model contract is treated as a strict interface.

```text
                ┌──────────────────────┐
                │ Feature Engineering  │
                └──────────┬───────────┘
                           │
                           ▼
                 ┌──────────────────┐
                 │ 13-Feature       │
                 │ Contract         │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ XGBoost Model    │
                 └────────┬─────────┘
                          │
                          ▼
                    Prediction
```

The system preserves exact feature ordering because a numerically valid feature matrix can still produce incorrect model behavior if the feature order does not match the serialized model contract.

---

# 13. Layer 8 — Explainable AI

The XAI layer provides interpretation of predictive model behavior.

Two complementary approaches are used.

### XGBoost Feature Importance

Provides model-level feature importance.

### SHAP

Provides:

* Global explanations
* Local explanations

Architecture:

```text
Prediction Model
      ↓
Feature Contributions
      ↓
SHAP
      ↓
Global Explanation
+
Local Explanation
```

This allows the system to move from:

> "What did the model predict?"

toward:

> "Which features contributed to the prediction?"

---

# 14. Layer 9 — Automated Reporting

The reporting layer aggregates outputs from multiple intelligence layers.

```text
Sustainability Intelligence
          +
RAG Knowledge
          +
Forecasting
          +
XAI
          ↓
Automated Reporting Layer
          ↓
┌─────────────────────────────┐
│ Global Report               │
│ Management Summary          │
│ Recommendations             │
│ JSON Report                 │
│ Markdown Report             │
│ Building-Level Reports      │
└─────────────────────────────┘
```

The platform generated and validated **1,578 building-level reports**.

---

# 15. Layer 10 — Application Layer

The Streamlit application provides an interactive interface over the intelligence layers.

### Dashboard capabilities

* Global sustainability KPIs
* Building search
* Building rankings
* Sustainability analysis
* Risk analysis
* Forecasting visualization
* XAI visualization
* RAG assistant
* Report access

The dashboard acts as an application layer rather than containing the core analytical logic itself.

This separation allows the underlying intelligence to remain reusable across different interfaces.

---

# 16. Layer 11 — MLOps

The MLOps layer provides experiment and inference tracking.

The architecture includes:

```text
Inference Pipeline
       ↓
Experiment Configuration
       ↓
MLflow
       ↓
Parameters
Metrics
Artifacts
```

The MLflow backend was migrated to SQLite to provide persistent experiment tracking.

---

# 17. Layer 12 — Production Inference

A lightweight production inference pipeline was constructed around the validated forecasting model.

The inference architecture is:

```text
Request
   ↓
Input Validation
   ↓
13-Feature Contract
   ↓
Feature Matrix
   ↓
XGBoost Model
   ↓
Prediction
   ↓
Validation
   ↓
Response
```

The production inference workflow generated and validated **110 predictions**.

---

# 18. Layer 13 — API

FastAPI exposes the inference capability through a REST interface.

```text
Client
  │
  ▼
FastAPI
  │
  ├── /health
  │
  └── /predict
          │
          ▼
   Request Validation
          │
          ▼
    Feature Contract
          │
          ▼
      ML Inference
          │
          ▼
       Response
```

This creates a clean boundary between the machine-learning system and external applications.

---

# 19. Layer 14 — Deployment

The deployment architecture packages the inference service for reproducible execution.

```text
Application
     ↓
Dependencies
     ↓
Docker Image
     ↓
Containerized Service
     ↓
FastAPI
     ↓
Production Inference
```

Deployment configuration is maintained separately from the analytical intelligence itself.

This prevents the production serving layer from becoming tightly coupled to exploratory development workflows.

---

# 20. Validation Architecture

Validation is implemented across major project boundaries.

```text
Data
 ↓
Validation
 ↓
Sustainability Intelligence
 ↓
Validation
 ↓
Knowledge / RAG
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
Deployment Validation
```

The project uses machine-readable validation to reduce reliance on manual inspection.

---

# 21. Artifact Dependency Model

The system follows an explicit artifact dependency chain.

```text
Authoritative Data
       ↓
Sustainability Master Dataset
       ↓
Sustainability Intelligence
       ↓
Knowledge Records
       ↓
Embeddings
       ↓
FAISS Index
       ↓
RAG
```

A separate predictive branch is:

```text
Validated Feature Matrix
       ↓
XGBoost Model
       ↓
Inference
       ↓
MLflow
       ↓
FastAPI
```

The reporting branch consumes outputs from multiple layers:

```text
Sustainability
      +
RAG
      +
Forecasting
      +
XAI
      ↓
Reporting
```

---

# 22. Engineering Integrity

The project follows an explicit rule:

> **Validated artifacts should not be silently modified to hide discrepancies.**

When a discrepancy was identified between a historical forecasting artifact and independently reconstructed model behavior, the discrepancy was documented rather than silently overwriting the historical artifact.

This approach preserves:

* Traceability
* Reproducibility
* Auditability
* Engineering transparency

---

# 23. Separation of Concerns

The architecture separates major responsibilities:

| Layer               | Responsibility                         |
| ------------------- | -------------------------------------- |
| Data                | Source information                     |
| Feature Engineering | Model-ready representations            |
| Sustainability      | Domain intelligence                    |
| Knowledge           | Retrieval-ready information            |
| RAG                 | Grounded natural-language intelligence |
| Forecasting         | Predictive modeling                    |
| XAI                 | Model interpretation                   |
| Reporting           | Decision-oriented outputs              |
| Dashboard           | Interactive presentation               |
| MLflow              | Experiment tracking                    |
| Inference           | Production prediction                  |
| FastAPI             | Service interface                      |
| Docker              | Deployment packaging                   |

This separation allows individual components to evolve without requiring the entire platform to be redesigned.

---

# 24. End-to-End Data Flow

A complete request can be conceptualized as:

```text
Infrastructure Data
        ↓
Feature Engineering
        ↓
Sustainability Metrics
        ↓
Knowledge Engineering
        ↓
Semantic Retrieval
        ↓
Grounded AI Response

                   AND

Infrastructure Data
        ↓
Model Features
        ↓
XGBoost
        ↓
Prediction
        ↓
SHAP Explanation

                   ↓
          Automated Reporting

                   ↓

      Dashboard / API / Deployment
```

The platform therefore combines **descriptive, predictive, explanatory, and generative intelligence** within a single engineering architecture.

---

# 25. Architectural Characteristics

The resulting platform is designed around the following characteristics:

### Modular

Major intelligence capabilities are separated into logical layers.

### Validated

Critical transformations and outputs are independently checked.

### Reproducible

Feature contracts, model contracts, and environment configuration are explicitly defined.

### Explainable

Forecasting outputs are accompanied by model interpretation capabilities.

### Grounded

Generative responses are based on retrieved sustainability knowledge.

### Deployable

Production inference is exposed through FastAPI and packaged using Docker.

### Observable

MLflow provides experiment and inference tracking.

### Extensible

The architecture provides clear extension points for real-time data, advanced forecasting, monitoring, optimization, and production deployment.

---

# 26. Architecture Summary

The platform can be summarized as:

```text
┌────────────────────────────────────────────────────────────┐
│                 SUSTAINABLE INFRASTRUCTURE                 │
│                     INTELLIGENCE PLATFORM                  │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  DATA ENGINEERING                                          │
│       ↓                                                    │
│  SUSTAINABILITY INTELLIGENCE                              │
│       ↓                                                    │
│  ┌──────────────────┐       ┌─────────────────────────┐   │
│  │ RAG + LLM        │       │ XGBoost + SHAP         │   │
│  └────────┬─────────┘       └──────────┬──────────────┘   │
│           │                            │                  │
│           └────────────┬───────────────┘                  │
│                        ↓                                   │
│                AUTOMATED REPORTING                         │
│                        ↓                                   │
│             ┌──────────┴───────────┐                       │
│             ↓                      ↓                        │
│        STREAMLIT               PRODUCTION                  │
│        DASHBOARD               INFERENCE                   │
│                                   ↓                        │
│                                MLFLOW                      │
│                                   ↓                        │
│                                FASTAPI                     │
│                                   ↓                        │
│                                DOCKER                      │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

**Core architectural principle:**

> **Build validated intelligence first, expose it through reusable interfaces, and preserve traceability from data to production inference.**
