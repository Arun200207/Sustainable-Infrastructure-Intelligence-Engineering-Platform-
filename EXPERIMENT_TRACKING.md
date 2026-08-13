# Experiment Tracking & MLOps

## Sustainable Infrastructure Intelligence Platform

> **Phase 14 — MLOps, Production Inference & Deployment Readiness**

The Sustainable Infrastructure Intelligence Platform implements an MLOps layer that connects **experimentation, validation, inference, artifact management, and deployment**.

The objective is to move beyond a standalone:

```text
Train Model → Save Model
```

toward a traceable machine-learning lifecycle:

```text
Experiment
    ↓
Track
    ↓
Validate
    ↓
Package
    ↓
Serve
    ↓
Monitor
    ↓
Improve
```

---

## 1. MLOps Overview

The MLOps layer establishes traceability across:

```text
Dataset
   ↓
Feature Engineering
   ↓
Model
   ↓
Experiment
   ↓
Parameters
   ↓
Metrics
   ↓
Artifacts
   ↓
Validation
   ↓
Inference
   ↓
API
   ↓
Deployment
```

### Core objectives

* Experiment tracking
* Parameter and metric logging
* Inference traceability
* Artifact management
* Reproducible inference
* Machine-readable validation
* Explicit feature contracts
* Separation of training and serving
* Deployment-oriented model management

---

## 2. Technology Stack

| Component           | Technology                            |
| ------------------- | ------------------------------------- |
| Experiment Tracking | MLflow                                |
| Tracking Backend    | SQLite                                |
| Forecasting Model   | XGBoost                               |
| Inference           | Python                                |
| API                 | FastAPI                               |
| Dashboard           | Streamlit                             |
| Containerization    | Docker                                |
| Validation          | Machine-readable validation artifacts |
| Source Control      | GitHub                                |

---

## 3. Architecture

```text
                         ML DEVELOPMENT
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
         Parameters        Metrics          Artifacts
             │                │                │
             └────────────────┼────────────────┘
                              ▼
                           MLflow
                              │
                     ┌────────┴────────┐
                     ▼                 ▼
                  SQLite          Run Metadata
                     │
                     ▼
             Experiment History
                     │
                     ▼
             Production Inference
                     │
                     ▼
                  FastAPI
                     │
                     ▼
                   Docker
                     │
                     ▼
                Deployment
```

---

# 4. Experiment Tracking

Every meaningful ML run should answer:

```text
What model?
What data?
What features?
What parameters?
What metrics?
What artifacts?
What validation result?
```

MLflow provides the experiment-level record required to answer these questions.

A conceptual MLflow run contains:

```text
Run
├── Parameters
├── Metrics
├── Tags
├── Artifacts
└── Metadata
```

This creates a persistent identity for a particular experiment or inference execution.

---

## 5. MLflow Configuration

MLflow was introduced during **Phase 14 — MLflow + Production Inference**.

The implementation establishes:

* Experiment configuration
* Run tracking
* Parameter logging
* Metric logging
* Artifact tracking
* SQLite persistence
* Machine-readable validation

The repository implementation remains the authoritative source for the exact configuration and logged fields.

---

# 6. SQLite Tracking Backend

The project uses SQLite as the local MLflow tracking backend:

```text
MLflow
   │
   ▼
SQLite
   │
   ├── Experiments
   ├── Runs
   ├── Parameters
   ├── Metrics
   └── Metadata
```

### Why SQLite?

SQLite provides:

* Local persistence
* Minimal infrastructure
* Simple setup
* Low operational overhead
* Repository-local experimentation
* Reproducible development

It is appropriate for the current portfolio-scale implementation.

### Production evolution

SQLite should not be interpreted as the target architecture for a distributed production platform.

A production deployment could evolve toward:

```text
MLflow Tracking Server
        │
        ├── Production Database
        │
        └── Object / Artifact Storage
```

---

# 7. Parameter Tracking

Parameters describe the configuration associated with an experiment or inference workflow.

Examples include:

```text
model_type
feature_count
batch_size
prediction_count
model_version
```

Only parameters actually logged by the implementation should be treated as authoritative metadata.

Parameter tracking transforms an otherwise opaque result:

```text
Model Result
     ↓
Unknown Configuration
```

into:

```text
Model Result
     ↓
Tracked Configuration
     ↓
Reproducible Context
```

---

# 8. Model Performance Baseline

The validated forecasting workflow established the following baseline:

| Metric |           Value |
| ------ | --------------: |
| MAE    | **6087.625294** |
| RMSE   | **8492.059022** |
| R²     |    **0.768148** |

### Metric interpretation

**MAE — Mean Absolute Error**

```text
MAE = mean(|y - ŷ|)
```

Measures average absolute prediction error.

**RMSE — Root Mean Squared Error**

```text
RMSE = sqrt(mean((y - ŷ)²))
```

Penalizes larger errors more strongly than MAE.

**R² — Coefficient of Determination**

```text
R² = 1 - SS_res / SS_tot
```

Measures the proportion of variance explained relative to the selected baseline.

---

# 9. Artifact Tracking

The MLOps layer associates generated evidence with experiment runs.

Potential artifacts include:

```text
Prediction Outputs
Validation Results
Metric Files
Inference Metadata
Model Artifacts
Explainability Outputs
```

This follows the project's broader **artifact-first engineering model**:

```text
Phase
  ↓
Artifact
  ↓
Validation
  ↓
Reference
```

Validated artifacts should be preserved rather than silently overwritten.

---

# 10. Model vs. Experiment

The project explicitly separates the **model artifact** from the **experiment record**.

### Model

The executable predictive artifact used for inference.

### Experiment

The contextual record describing how a model or inference workflow was evaluated.

Conceptually:

```text
Model Artifact
      +
Experiment Record
      ↓
Traceable ML Workflow
```

This distinction is important because a model file alone does not explain:

* Which configuration produced it
* Which metrics were observed
* Which artifacts were generated
* Which validation checks passed
* Which environment was used

---

# 11. Production Inference

Phase 14 generated and validated:

```text
110 Inference Predictions
```

The production inference workflow follows:

```text
Production Model
      ↓
Feature Contract
      ↓
Inference Dataset
      ↓
110 Predictions
      ↓
Validation
      ↓
MLflow Tracking
```

This establishes traceability between the production model and its execution.

---

# 12. Feature Contract

The production forecasting model requires:

```text
13 Features
```

The feature contract is therefore part of the model's deployment contract.

```text
Model
  +
13-Feature Contract
  +
Inference Configuration
  ↓
Production Prediction
```

Feature ordering is particularly important:

```text
Correct Values
+
Incorrect Feature Ordering
=
Incorrect Model Input
```

A syntactically valid API request can therefore still produce semantically incorrect predictions.

---

# 13. Reproducible Inference

The inference workflow is designed around deterministic inputs:

```text
Same Model
+
Same Features
+
Same Ordering
+
Same Input
=
Equivalent Prediction
```

Reproducibility is supported through:

* Explicit feature contracts
* Serialized model artifacts
* Dependency configuration
* Deterministic inference workflows
* Machine-readable validation
* MLflow experiment tracking
* Docker packaging

---

# 14. Validation Architecture

MLflow records what happened.

Validation determines whether the result is acceptable.

```text
Track ≠ Validate
```

The architecture therefore separates:

```text
Experiment Tracking
        +
Independent Validation
        ↓
Reliable Evidence
```

### Validation pipeline

```text
Inference
   ↓
Metrics
   ↓
Validation Rules
   ↓
Machine-Readable Result
   ↓
PASS / FAIL
```

---

## 15. Validation Coverage

The MLOps workflow can validate:

* Prediction existence
* Prediction finiteness
* Prediction count
* Metric availability
* Feature count
* Feature ordering
* Artifact availability
* API consistency
* Deployment consistency

The machine-readable validation layer reduces dependence on manual inspection.

---

# 16. Forecasting + Explainability

The production forecasting model uses **XGBoost**.

The analytical workflow combines:

```text
XGBoost
   ↓
Prediction
   ↓
Performance Metrics
   ↓
Feature Importance
   ↓
SHAP Explainability
```

MLflow provides the tracking layer around these outputs.

A mature experiment record can therefore associate:

```text
Model
+
Metrics
+
Feature Importance
+
SHAP Outputs
```

---

# 17. Experiment Comparison

MLflow enables historical experiment comparison using:

```text
Parameters
Metrics
Artifacts
Tags
```

For example:

```text
Experiment A
R² = 0.72

Experiment B
R² = 0.76
```

The objective is not simply to maximize a single metric, but to establish evidence for model-selection decisions.

Potential production criteria include:

```text
Accuracy
+
Stability
+
Inference Cost
+
Explainability
+
Operational Complexity
```

---

# 18. Baseline-Driven Model Improvement

The current validated model establishes the baseline:

```text
MAE  = 6087.625294
RMSE = 8492.059022
R²   = 0.768148
```

Future model development can follow:

```text
Current Baseline
      ↓
Feature Engineering
      ↓
Candidate Model
      ↓
Experiment Run
      ↓
Metric Evaluation
      ↓
Compare Against Baseline
      ↓
Accept / Reject
```

This creates an evidence-driven model improvement process.

---

# 19. MLOps + RAG

The platform also contains a RAG + LLM layer:

```text
Sustainability Knowledge
        ↓
Embeddings
        ↓
FAISS
        ↓
Retrieval
        ↓
LLM
```

Future experiment tracking can extend to:

* Embedding model version
* Knowledge-base version
* Retrieval configuration
* Retrieval metrics
* Prompt configuration
* LLM configuration
* Response evaluation

This would provide equivalent traceability for the platform's generative-AI components.

---

# 20. MLOps + API

The API and experiment-tracking layers have separate responsibilities:

```text
MLflow
  │
  │ Tracking / Metadata
  ▼
Production Inference
  │
  ▼
FastAPI
  │
  ▼
External Client
```

**MLflow tracks the workflow.**

**FastAPI serves the workflow.**

This separation keeps experiment management independent from API transport.

---

# 21. MLOps + Dashboard

The Streamlit dashboard consumes validated project artifacts:

```text
Tracked / Validated Artifacts
            ↓
        Streamlit
            ↓
      Human Interaction
```

The dashboard is therefore a consumer of validated outputs rather than the source of truth.

---

# 22. End-to-End Model Lifecycle

The current architecture connects the major stages of the ML lifecycle:

```text
Data
 ↓
Feature Engineering
 ↓
Model Training
 ↓
Validation
 ↓
Experiment Tracking
 ↓
Model Selection
 ↓
Production Inference
 ↓
API Serving
 ↓
Docker
 ↓
Deployment
```

This moves the project beyond model development into **machine-learning system engineering**.

---

# 23. Reproducibility Architecture

```text
                 REPRODUCIBILITY
                       │
       ┌───────────────┼────────────────┐
       ▼               ▼                ▼
 Model Artifact   Feature Contract   Dependencies
       │               │                │
       └───────────────┼────────────────┘
                       ▼
                   Inference
                       │
                       ▼
                    MLflow
                       │
                       ▼
                   Validation
```

The goal is to make model behavior explainable in terms of both the executable artifact and the context surrounding its execution.

---

# 24. Traceability

The target traceability chain is:

```text
Dataset
   ↓
Features
   ↓
Model
   ↓
Experiment
   ↓
Metrics
   ↓
Inference
   ↓
API
   ↓
Deployment
```

Useful future metadata includes:

```text
Experiment Name
Run ID
Model Type
Model Version
Feature Set
Feature Count
Dataset Version
Metric Values
Artifact References
Environment
Validation Status
```

---

# 25. No Silent Mutation

A core engineering principle is:

> **Validated artifacts should not be silently modified simply because a later phase reveals a discrepancy.**

Instead:

```text
Validated Artifact
       ↓
Preserve
       ↓
Document Discrepancy
       ↓
Reference Original Evidence
```

This principle protects experiment integrity and maintains a reliable historical record.

---

# 26. Model Lifecycle — Future State

A mature production lifecycle can evolve toward:

```text
Training
   ↓
Validation
   ↓
Model Registry
   ↓
Staging
   ↓
Production
   ↓
Monitoring
   ↓
Retraining
```

Potential model states:

```text
Candidate
   ↓
Validated
   ↓
Staging
   ↓
Production
   ↓
Archived
```

---

# 27. Model Promotion

Future promotion gates could require:

```text
Metric Threshold
        +
Validation PASS
        +
Feature Contract PASS
        +
Inference Regression PASS
        +
Security PASS
        +
Deployment PASS
```

Only models satisfying the defined acceptance criteria should be promoted.

---

# 28. Rollback Strategy

With model versioning and experiment history, rollback can follow:

```text
Production Model
      ↓
Monitoring
      ↓
Issue Detected
      ↓
Rollback
      ↓
Previous Validated Model
```

This makes deployment changes reversible rather than irreversible.

---

# 29. Future Monitoring

The current implementation establishes the foundations for production monitoring.

Future monitoring should cover:

```text
Data Quality
Feature Drift
Prediction Drift
Model Performance
Latency
Error Rate
```

A mature monitoring architecture could feed these signals back into the experiment and retraining lifecycle.

---

# 30. Automated Retraining — Future

A future automated workflow could be:

```text
Production Data
      ↓
Data Validation
      ↓
Drift Detection
      ↓
Retraining Trigger
      ↓
Model Training
      ↓
Evaluation
      ↓
MLflow Run
      ↓
Validation
      ↓
Model Promotion
```

Automated retraining is **not part of the current validated implementation**.

---

# 31. CI/CD — Future

MLOps can eventually integrate with CI/CD:

```text
Git Push
   ↓
Automated Tests
   ↓
Model Validation
   ↓
MLflow Tracking
   ↓
Docker Build
   ↓
Security Scan
   ↓
Deployment
```

This would create an automated model-delivery lifecycle.

---

# 32. Data Versioning — Future

A complete MLOps architecture should associate models with explicit dataset versions:

```text
Dataset v1
Dataset v2
Dataset v3
```

Each training and evaluation run can then reference the exact dataset used.

---

# 33. Feature Store — Future

At larger scale, a feature store could manage:

```text
Feature Definitions
Feature Versions
Offline Features
Online Features
Feature Lineage
```

The current project instead uses an explicit **13-feature contract**, keeping the architecture lightweight and transparent.

---

# 34. MLOps Readiness

| Capability                    | Status         |
| ----------------------------- | -------------- |
| MLflow Integration            | 🟢 Implemented |
| Experiment Configuration      | 🟢 Implemented |
| Parameter Tracking            | 🟢 Implemented |
| Metric Tracking               | 🟢 Implemented |
| Artifact Tracking             | 🟢 Implemented |
| SQLite Backend                | 🟢 Implemented |
| Production Inference          | 🟢 Implemented |
| 13-Feature Contract           | 🟢 Implemented |
| 110 Validated Predictions     | 🟢 Implemented |
| Machine-Readable Validation   | 🟢 Implemented |
| API Integration               | 🟢 Implemented |
| Docker Deployment Preparation | 🟢 Implemented |
| Model Registry                | 🟡 Future      |
| Central MLflow Server         | 🟡 Future      |
| Artifact Store                | 🟡 Future      |
| Automated Monitoring          | 🟡 Future      |
| Drift Detection               | 🟡 Future      |
| Automated Retraining          | 🟡 Future      |
| CI/CD                         | 🟡 Future      |
| Feature Store                 | 🟡 Future      |

---

# 35. Sustainable Infrastructure Scale

The MLOps layer sits on top of a substantial sustainability analytics foundation:

```text
Buildings:       1,578
Daily Records:   1,153,518
Energy:          24.587B kWh
Net Carbon:      8.391M tCO₂e
Net Cost:        $2.705B
```

This allows future experiments to evaluate forecasting and intelligence improvements against a defined analytical baseline.

---

# 36. Engineering Value

The MLOps implementation demonstrates practical experience across:

```text
Model Development
        +
Experiment Tracking
        +
Validation
        +
Inference
        +
API Serving
        +
Containerization
        +
Deployment Engineering
```

The engineering narrative is therefore stronger than simply reporting a model score.

### Portfolio positioning

> **Built and validated an XGBoost forecasting workflow, established a 13-feature production contract, tracked experiments and inference artifacts through MLflow, generated 110 validated predictions, exposed the inference workflow through FastAPI, and prepared the system for containerized deployment.**

---

# 37. Current Achievement

```text
PHASE 14 — MLOps + PRODUCTION INFERENCE
================================================

Production Model Artifact       🟢 PASS
13-Feature Contract              🟢 PASS
Inference Pipeline               🟢 PASS
110 Predictions                  🟢 PASS
Inference Metrics                🟢 PASS
Machine-Readable Validation      🟢 PASS

MLflow Experiment Tracking       🟢 PASS
SQLite Tracking Backend          🟢 PASS
Parameter Logging                🟢 PASS
Metric Logging                   🟢 PASS
Artifact Tracking                🟢 PASS

FastAPI Integration              🟢 PASS
Docker Preparation               🟢 PASS

STATUS: TECHNICALLY COMPLETE
================================================
```

---

# 38. Roadmap

```text
CURRENT
   │
   ├── MLflow + SQLite
   ├── Production Inference
   ├── Validation
   ├── FastAPI
   └── Docker Preparation
   │
   ▼
MODEL REGISTRY
   │
   ▼
CENTRAL MLflow SERVER
   │
   ▼
ARTIFACT STORE
   │
   ▼
CI/CD
   │
   ▼
MODEL MONITORING
   │
   ▼
DRIFT DETECTION
   │
   ▼
AUTOMATED RETRAINING
   │
   ▼
MODEL PROMOTION
```

---

# 39. Final Architecture

```text
┌──────────────────────────────────────────────────────────┐
│                    ML DEVELOPMENT                       │
│                                                          │
│  Data → Features → Model → Validation                   │
└──────────────────────────┬───────────────────────────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │      MLflow       │
                 │                   │
                 │ Parameters        │
                 │ Metrics           │
                 │ Artifacts         │
                 │ Runs              │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ Production        │
                 │ Inference         │
                 │                   │
                 │ 13-Feature        │
                 │ Contract          │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ Machine-Readable  │
                 │ Validation        │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ FastAPI           │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ Docker            │
                 └─────────┬─────────┘
                           │
                           ▼
                      Deployment
```

---

## 40. Conclusion

The MLOps architecture establishes the operational layer connecting:

```text
Experimentation
      ↓
Validation
      ↓
Tracking
      ↓
Inference
      ↓
Serving
      ↓
Deployment
```

The project therefore demonstrates an **end-to-end machine-learning lifecycle**, rather than a standalone predictive model.

The current implementation provides a practical foundation for future **model registry integration, centralized tracking, monitoring, drift detection, CI/CD, and automated retraining**.
