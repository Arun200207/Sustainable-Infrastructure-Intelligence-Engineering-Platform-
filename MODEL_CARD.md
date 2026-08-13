# Model Card

## Sustainable Infrastructure Intelligence Platform

> **Technical model documentation for the validated XGBoost energy forecasting system.**

---

# 1. Model Overview

The Sustainable Infrastructure Intelligence Platform uses an **XGBoost-based forecasting model** to generate energy-related predictions from a validated set of engineered infrastructure, temporal, historical, and environmental features.

The model is integrated into a larger AI engineering platform containing:

* Sustainability intelligence
* RAG + LLM
* Explainable AI
* Automated reporting
* Streamlit dashboard
* MLflow tracking
* Production inference
* FastAPI
* Docker deployment

The forecasting model therefore operates as one component of a larger intelligence system rather than as an isolated machine-learning experiment.

---

# 2. Model Identity

| Property             | Description                       |
| -------------------- | --------------------------------- |
| Model Type           | Gradient-Boosted Decision Tree    |
| Framework            | XGBoost                           |
| Task                 | Energy Forecasting                |
| Input Features       | **13**                            |
| Explainability       | SHAP + XGBoost Feature Importance |
| Production Inference | Yes                               |
| API Integration      | Yes                               |
| MLflow Integration   | Yes                               |
| Deployment Packaging | Docker                            |
| Validation Status    | **PASS**                          |

---

# 3. Intended Use

The model is intended to support:

* Building-level energy forecasting
* Sustainability analysis
* Energy planning
* Comparative infrastructure analysis
* Dashboard visualization
* Automated sustainability reporting
* Production inference demonstrations

The model is particularly useful as a predictive layer within the broader sustainability intelligence platform.

---

# 4. Out-of-Scope Uses

The model should not be treated as:

* A guaranteed future energy demand predictor
* A financial forecasting model
* A causal model
* A complete building-control system
* A substitute for engineering assessment
* A substitute for operational safety systems
* A standalone basis for major infrastructure investment decisions

Predictions should be interpreted in the context of the available historical data and model assumptions.

---

# 5. Problem Definition

The forecasting problem can be represented conceptually as:

```text id="2l9i5w"
Historical Building Information
          +
Historical Energy Behavior
          +
Temporal Information
          +
Environmental Context
          ↓
     Feature Engineering
          ↓
     13-Feature Contract
          ↓
        XGBoost
          ↓
   Energy Prediction
```

The model attempts to learn relationships between engineered historical/environmental variables and the target energy behavior represented in the project's forecasting workflow.

---

# 6. Model Architecture

XGBoost is a gradient-boosting algorithm based on ensembles of decision trees.

Conceptually:

```text id="g7s9z1"
Input Features
      ↓
Decision Tree 1
      ↓
Residual Correction
      ↓
Decision Tree 2
      ↓
Residual Correction
      ↓
      ...
      ↓
Ensemble Prediction
```

The model was selected for its suitability for structured/tabular data and its compatibility with feature importance and SHAP-based interpretation.

---

# 7. Input Contract

The production model requires:

**13 features**

The exact feature names and ordering are part of the serialized model's contract.

The inference pipeline therefore follows:

```text id="x2p7k4"
Input Data
    ↓
Feature Validation
    ↓
13 Required Features
    ↓
Exact Feature Ordering
    ↓
Feature Matrix
    ↓
XGBoost
```

Feature order is treated as a correctness requirement.

A feature matrix containing the correct values in an incorrect order can produce invalid predictions even if the matrix itself is numerically well-formed.

---

# 8. Feature Engineering

The forecasting workflow uses engineered representations derived from the sustainability data layer.

Major feature categories include:

### Temporal Features

Capture recurring time-dependent behavior.

### Lag Features

Represent historical energy observations.

### Rolling Features

Represent recent historical energy behavior.

### Environmental Features

Represent weather and environmental context.

### Degree-Day Features

Represent heating and cooling demand conditions.

These features are transformed into the model's required 13-feature input contract.

---

# 9. Feature Contract Reconstruction

One of the major engineering tasks in the forecasting phase was inspecting the serialized model and reconstructing its expected feature matrix.

The workflow was:

```text id="1b2jgz"
Serialized Model
      ↓
Model Contract Inspection
      ↓
Required Feature Identification
      ↓
Feature Matrix Reconstruction
      ↓
Exact Ordering
      ↓
Prediction Validation
```

This prevented an incorrect assumption about the model's original feature representation.

---

# 10. Model Validation

The forecasting workflow was independently reconstructed and validated.

Validation included:

* Feature contract inspection
* Required feature identification
* Feature-order verification
* Feature matrix reconstruction
* Prediction generation
* Prediction-finiteness validation
* MAE calculation
* RMSE calculation
* R² calculation
* Feature-importance extraction
* SHAP integration

---

# 11. Validated Performance

The validated model metrics are:

| Metric   |           Value |
| -------- | --------------: |
| **MAE**  | **6087.625294** |
| **RMSE** | **8492.059022** |
| **R²**   |    **0.768148** |

---

# 12. Metric Interpretation

## MAE — Mean Absolute Error

```text id="t1b6c2"
MAE = 6087.625294
```

MAE represents the average absolute difference between predicted and observed target values under the evaluation procedure.

Lower values indicate smaller average absolute prediction errors.

---

## RMSE — Root Mean Squared Error

```text id="4g4p3j"
RMSE = 8492.059022
```

RMSE places greater weight on larger errors than MAE.

The difference between MAE and RMSE indicates that larger prediction errors contribute meaningfully to the overall error distribution.

---

## R² — Coefficient of Determination

```text id="g9h8j2"
R² = 0.768148
```

The R² value indicates that the model explains a substantial portion of the target variation under the evaluated conditions.

It should not be interpreted as a guarantee of equivalent performance on unseen infrastructure populations or future operating conditions.

---

# 13. Model Evaluation Perspective

The model should be evaluated using multiple metrics rather than a single score.

The project therefore reports:

```text id="7u0w9k"
MAE
+
RMSE
+
R²
```

This provides complementary information:

* MAE → average absolute error
* RMSE → sensitivity to larger errors
* R² → explained variance perspective

---

# 14. Prediction Validation

Generated predictions were checked for numerical validity.

The validation process includes a prediction-finiteness check to ensure that model outputs do not contain invalid numerical values such as:

* NaN
* Positive infinity
* Negative infinity

Conceptually:

```text id="q2x8vr"
Model Prediction
      ↓
Numerical Validation
      ↓
Finite?
  ┌───┴───┐
 YES     NO
  ↓       ↓
Valid    Reject
```

---

# 15. Explainability

The model is integrated with two complementary explainability approaches.

## XGBoost Feature Importance

Provides an aggregate view of feature relevance within the trained model.

## SHAP

Provides contribution-based explanations.

SHAP is used for:

* Global explanations
* Local explanations

---

# 16. Global Explainability

Global explanations answer:

> Which features generally influence model behavior across the evaluated observations?

The global explanation layer helps identify important predictive signals.

Conceptually:

```text id="r5s6vw"
Dataset
  ↓
Model
  ↓
SHAP Values
  ↓
Aggregate Feature Contributions
  ↓
Global Explanation
```

---

# 17. Local Explainability

Local explanations answer:

> Why did the model produce this particular prediction?

The workflow is:

```text id="b7z3q1"
Single Observation
       ↓
Model Prediction
       ↓
SHAP Contributions
       ↓
Feature-Level Explanation
```

This is useful when examining individual building-level predictions.

---

# 18. Feature Importance vs SHAP

The project distinguishes between:

### Feature Importance

A model-level indication of feature relevance.

### SHAP

A contribution-based explanation of model output.

They should not be treated as interchangeable.

Feature importance can indicate that a feature is heavily used by the model, while SHAP can provide directional contribution information for individual predictions.

---

# 19. Model Interpretability Limitation

Neither feature importance nor SHAP establishes causality.

For example:

> A feature with high predictive importance does not automatically mean that changing that feature will cause energy consumption to change by a corresponding amount.

The explanations describe **model behavior**, not physical causation.

Domain and engineering analysis are required before using model explanations for intervention decisions.

---

# 20. Production Inference

The model has been integrated into a lightweight production inference workflow.

The inference path is:

```text id="3j0tq7"
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
Output Validation
```

The workflow generated:

**110 validated inference predictions**

---

# 21. Inference Contract

The production inference contract requires:

1. Correct feature set
2. Correct feature ordering
3. Compatible data types
4. Valid numerical inputs
5. Model-compatible input shape

This contract prevents the serving layer from making assumptions about the model input structure.

---

# 22. FastAPI Integration

The model is exposed through a FastAPI service.

Primary endpoints:

```text id="h2y0nr"
GET /health
POST /predict
```

The prediction endpoint provides the external boundary between applications and the model.

```text id="k4q9wx"
Client
  ↓
FastAPI
  ↓
Validation
  ↓
Inference
  ↓
XGBoost
  ↓
Prediction
```

---

# 23. MLflow Integration

The model/inference workflow is integrated with MLflow.

Tracked information includes:

* Parameters
* Metrics
* Inference artifacts
* Experiment metadata

This establishes an experiment-tracking layer around the predictive system.

---

# 24. Deployment

The inference service is packaged using Docker.

The deployment path is:

```text id="6a5g0r"
Model
  ↓
Inference Pipeline
  ↓
FastAPI
  ↓
Docker
  ↓
Containerized Service
```

The model can therefore be treated as a serving component rather than only a development artifact.

---

# 25. Model Lifecycle

The model's lifecycle can be represented as:

```text id="u8w6q5"
Serialized Model
      ↓
Contract Inspection
      ↓
Feature Reconstruction
      ↓
Validation
      ↓
Inference
      ↓
MLflow Tracking
      ↓
FastAPI
      ↓
Docker
```

This demonstrates the transition from a stored ML artifact to a production-oriented inference component.

---

# 26. Artifact Integrity

A historical forecasting artifact contained a discrepancy relative to the independently reconstructed model behavior.

The engineering decision was **not to silently overwrite the historical artifact**.

Instead:

```text id="x0k3yd"
Historical Artifact
       ↓
Discrepancy Identified
       ↓
Independent Reconstruction
       ↓
Validated Result
       ↓
Discrepancy Documented
```

This preserves historical traceability and prevents evidence from being altered simply to make outputs appear consistent.

---

# 27. Model/Data Dependency

The forecasting model depends on the upstream feature-engineering layer.

```text id="q3g1zc"
Source Data
    ↓
Feature Engineering
    ↓
13-Feature Contract
    ↓
XGBoost
```

A change in the feature-generation logic can affect model compatibility and prediction behavior.

Therefore, feature engineering and model inference must be treated as a coupled contract.

---

# 28. Data Drift Considerations

The current implementation does not represent a complete automated production data-drift monitoring system.

A production implementation should monitor:

* Feature distributions
* Missing-value rates
* Input ranges
* Temporal distribution
* Building population changes
* Prediction distributions
* Error distributions

Potential future architecture:

```text id="h8x2m5"
Production Inputs
      ↓
Drift Detection
      ↓
Threshold Evaluation
      ↓
Alert
      ↓
Investigation / Retraining
```

---

# 29. Model Drift Considerations

Model performance can degrade when infrastructure behavior changes.

Potential causes include:

* Building modifications
* Weather regime changes
* Energy-price changes
* Operational changes
* Occupancy changes
* Equipment upgrades
* Data-source changes

Future production monitoring should compare current performance against validated baseline performance.

---

# 30. Bias and Generalization

Model performance depends on the data used to construct and evaluate the model.

Potential generalization limitations include:

* Building-type imbalance
* Geographic differences
* Historical operating patterns
* Environmental differences
* Changes in infrastructure behavior

Therefore, the reported metrics should not automatically be generalized to all buildings or infrastructure systems.

---

# 31. Environmental and Sustainability Context

The model is embedded in a sustainability intelligence platform.

Its predictions can contribute to:

* Energy planning
* Building comparisons
* Sustainability reporting
* Carbon analysis
* Cost analysis
* Decision-support workflows

However, a prediction alone does not establish that a particular operational intervention will reduce carbon or cost.

Predictions should be combined with domain knowledge and validated intervention strategies.

---

# 32. Reproducibility

The forecasting system was developed with reproducibility-oriented controls including:

* Explicit feature contract
* Serialized model inspection
* Exact feature ordering
* Machine-readable validation
* Metric calculation
* MLflow tracking
* Production inference validation
* Containerization

The objective is to make the model behavior reproducible from a defined contract rather than dependent on undocumented notebook state.

---

# 33. Security Considerations

The current model is designed for portfolio-grade production inference architecture.

A hardened production deployment should additionally include:

* Authentication
* Authorization
* TLS
* Secret management
* Rate limiting
* Input abuse protection
* Dependency scanning
* Container security scanning
* Audit logging

These are deployment concerns rather than properties of the predictive model itself.

---

# 34. Known Limitations

The model has several important limitations.

### Historical Data Dependence

The model learns from historical patterns and may perform differently when operating conditions change.

### Forecasting Uncertainty

A point prediction does not fully represent uncertainty around future energy behavior.

### Feature Dependence

The model depends on the correctness and availability of its 13 required features.

### Distribution Shift

Performance may degrade when future observations differ substantially from the development distribution.

### Causality

Model explanations do not establish causal relationships.

### External Validity

The validated performance should not automatically be generalized to unrelated infrastructure populations.

---

# 35. Future Model Improvements

Potential future improvements include:

## Probabilistic Forecasting

Generate prediction intervals rather than only point estimates.

## Temporal Deep Learning

Evaluate architectures such as temporal neural networks or transformers.

## Ensemble Forecasting

Combine multiple forecasting approaches.

## Automated Hyperparameter Optimization

Systematically optimize model parameters using tracked experiments.

## Model Monitoring

Track production error, drift, and prediction distributions.

## Automated Retraining

Trigger retraining based on monitored performance or drift thresholds.

## Uncertainty Quantification

Provide confidence or prediction intervals for operational decision support.

---

# 36. Model Governance

A production-grade model governance process should track:

```text id="z9q3hj"
Model Version
      ↓
Feature Contract Version
      ↓
Training Data Version
      ↓
Evaluation Metrics
      ↓
Approval Status
      ↓
Deployment Version
      ↓
Monitoring Results
```

This creates traceability between what was trained, what was evaluated, and what is deployed.

---

# 37. Model Risk Perspective

The main model risks are:

| Risk                              | Impact | Mitigation                      |
| --------------------------------- | ------ | ------------------------------- |
| Feature mismatch                  | High   | Explicit 13-feature contract    |
| Feature-order error               | High   | Exact ordering validation       |
| Invalid prediction                | Medium | Finiteness validation           |
| Data drift                        | High   | Future monitoring               |
| Model drift                       | High   | Future performance monitoring   |
| Poor generalization               | High   | Broader evaluation              |
| Misinterpretation of XAI          | Medium | Causality limitation documented |
| Historical artifact inconsistency | Medium | Independent reconstruction      |
| Deployment misconfiguration       | Medium | API/deployment validation       |

---

# 38. Technical Validation Summary

The forecasting component has passed the following engineering checks:

```text
✓ Serialized model inspected
✓ Model feature contract identified
✓ 13 required features reconstructed
✓ Feature ordering preserved
✓ Test predictions generated
✓ Prediction finiteness validated
✓ MAE calculated
✓ RMSE calculated
✓ R² calculated
✓ Feature importance extracted
✓ SHAP integrated
✓ Global explanations generated
✓ Local explanations generated
✓ Production inference validated
✓ 110 inference predictions generated
✓ MLflow tracking configured
✓ FastAPI inference integrated
✓ Docker deployment packaged
```

---

# 39. Final Model Summary

The forecasting component can be summarized as:

```text id="k7j2v8"
             VALIDATED DATA
                    ↓
           FEATURE ENGINEERING
                    ↓
             13-FEATURE
              CONTRACT
                    ↓
               XGBOOST
                    ↓
              PREDICTION
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
      METRICS                SHAP
          ↓                   ↓
     MAE/RMSE/R²       GLOBAL + LOCAL XAI
          │                   │
          └─────────┬─────────┘
                    ↓
             MLflow Tracking
                    ↓
              FastAPI API
                    ↓
                Docker
```

---

# 40. Final Assessment

The forecasting model should be evaluated not only by its **R² = 0.768148**, but by the engineering system surrounding it.

The project demonstrates:

* Explicit model contracts
* Independent validation
* Reproducible feature construction
* Prediction validation
* Quantitative evaluation
* Explainability
* Experiment tracking
* Production inference
* API integration
* Deployment packaging
* Artifact integrity

The model therefore serves as a concrete example of moving from:

> **Machine Learning Model → Validated ML Component → Production-Oriented AI Service**

within a larger sustainable infrastructure intelligence platform.
