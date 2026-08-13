# API Documentation

## Sustainable Infrastructure Intelligence Platform

> Production-oriented FastAPI inference service for exposing the validated sustainability forecasting model through a documented REST API.

---

## 1. API Overview

The Sustainable Infrastructure Intelligence Platform exposes its production inference workflow through a FastAPI service.

The API provides a lightweight interface between the trained machine-learning model and external applications.

### Architecture

```text
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
Production Inference Pipeline
  ↓
XGBoost Model
  ↓
Prediction Validation
  ↓
JSON Response
```

The API establishes a clear boundary between external applications and the machine-learning inference layer.

---

## 2. API Objectives

The API layer was designed to:

* Expose the validated forecasting model.
* Provide a production-style inference interface.
* Validate incoming requests.
* Preserve the model's 13-feature input contract.
* Return machine-readable prediction responses.
* Provide service health monitoring.
* Maintain consistency with Phase-14 inference.
* Separate model inference from the dashboard layer.
* Support containerized deployment.
* Provide a foundation for future production integration.

---

## 3. API Technology Stack

| Component            | Technology                    |
| -------------------- | ----------------------------- |
| API Framework        | FastAPI                       |
| Programming Language | Python                        |
| Validation           | Pydantic / FastAPI validation |
| Model                | XGBoost                       |
| Inference            | Production inference pipeline |
| Serialization        | JSON                          |
| Documentation        | OpenAPI / Swagger / ReDoc     |
| Deployment           | Docker                        |
| Experiment Tracking  | MLflow                        |
| Repository           | GitHub                        |

---

## 4. API Architecture

### High-Level Architecture

```text
┌───────────────────────────────────────────────┐
│              External Client                 │
└──────────────────────┬────────────────────────┘
                       │
                       │ HTTP
                       ▼
┌───────────────────────────────────────────────┐
│                  FastAPI                      │
│                                               │
│  ┌─────────────────────────────────────────┐  │
│  │ Request Validation                      │  │
│  └──────────────────────┬──────────────────┘  │
│                         │                     │
│  ┌──────────────────────▼──────────────────┐  │
│  │ 13-Feature Contract                     │  │
│  └──────────────────────┬──────────────────┘  │
│                         │                     │
│  ┌──────────────────────▼──────────────────┐  │
│  │ Production Inference Pipeline           │  │
│  └──────────────────────┬──────────────────┘  │
└─────────────────────────┼─────────────────────┘
                          │
                          ▼
              ┌──────────────────────┐
              │    XGBoost Model     │
              └──────────┬───────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │ Prediction Validation│
              └──────────┬───────────┘
                         │
                         ▼
                  JSON Response
```

### API Boundary

```text
External System
      ↓
     HTTP
      ↓
   FastAPI
      ↓
 Validation
      ↓
 Inference
      ↓
 Prediction
```

The FastAPI service acts as the model-serving boundary, separating external clients from the underlying inference implementation.

---

## 5. API Endpoints

The current API exposes two primary endpoints:

| Method | Endpoint   | Purpose                     |
| ------ | ---------- | --------------------------- |
| `GET`  | `/health`  | Service health check        |
| `POST` | `/predict` | Generate a model prediction |

---

# 6. Health Endpoint

## `GET /health`

The health endpoint verifies that the API service is operational.

### Request

```http
GET /health
```

### Example

```bash
curl http://localhost:8000/health
```

### Expected Response

```json
{
  "status": "healthy"
}
```

> **Note:** The exact response structure should follow the implementation currently present in the repository.

### Purpose

The endpoint provides a lightweight mechanism for:

* Local development checks
* Container health checks
* Deployment verification
* Service monitoring
* Availability testing
* API smoke testing

The endpoint is intentionally lightweight and should not perform expensive model inference.

---

# 7. Prediction Endpoint

## `POST /predict`

The prediction endpoint accepts the model's required feature vector and returns an inference result.

### Request

```http
POST /predict
Content-Type: application/json
```

The request body contains the validated model inputs.

### Prediction Flow

```text
POST /predict
      ↓
JSON Request
      ↓
Schema Validation
      ↓
Feature Extraction
      ↓
Feature Ordering
      ↓
Inference Matrix
      ↓
XGBoost Model
      ↓
Prediction Validation
      ↓
JSON Response
```

---

# 8. Model Feature Contract

The production model requires:

```text
13 Features
```

The feature contract was inspected directly from the serialized model during Phase 11.

The project explicitly preserves the expected feature ordering.

This is critical because a valid numerical value assigned to the wrong feature position can produce an invalid prediction even when the request itself is syntactically correct.

### Feature Contract Principle

```text
Request Schema
      ↓
Feature Contract
      ↓
Model Input
```

The API therefore does not silently infer missing model semantics.

---

# 9. Prediction Request Structure

The exact field names must match the feature names defined by the production model contract.

A conceptual request is:

```json
{
  "feature_1": 0.0,
  "feature_2": 0.0,
  "feature_3": 0.0,
  "feature_4": 0.0,
  "feature_5": 0.0,
  "feature_6": 0.0,
  "feature_7": 0.0,
  "feature_8": 0.0,
  "feature_9": 0.0,
  "feature_10": 0.0,
  "feature_11": 0.0,
  "feature_12": 0.0,
  "feature_13": 0.0
}
```

> **Important:** The placeholder names above are illustrative. The deployed API contract must use the actual 13 feature names defined by the project's serialized model and inference implementation.

---

# 10. Request Validation

FastAPI provides request-level validation before the model receives the input.

Validation protects the inference layer from malformed requests.

Potential validation failures include:

* Missing required fields
* Invalid field types
* Invalid request structure
* Incorrect feature count
* Invalid numerical values
* Unexpected input structure

### Validation Boundary

```text
HTTP Request
     ↓
Schema Validation
     ↓
Type Validation
     ↓
Feature Validation
     ↓
Model
```

Without validation:

```text
Invalid Request
      ↓
Model
      ↓
Potentially Invalid Prediction
```

With validation:

```text
Invalid Request
      ↓
Validation Layer
      ↓
Rejected
```

---

# 11. Example `curl` Request

A generic request structure is:

```bash
curl -X POST "http://localhost:8000/predict" \
  -H "Content-Type: application/json" \
  -d '{
    "feature_1": 0.0,
    "feature_2": 0.0,
    "feature_3": 0.0,
    "feature_4": 0.0,
    "feature_5": 0.0,
    "feature_6": 0.0,
    "feature_7": 0.0,
    "feature_8": 0.0,
    "feature_9": 0.0,
    "feature_10": 0.0,
    "feature_11": 0.0,
    "feature_12": 0.0,
    "feature_13": 0.0
  }'
```

Replace the placeholder fields with the actual production feature names.

---

# 12. Example Python Client

The API can be consumed from Python using `requests`.

```python
import requests

url = "http://localhost:8000/predict"

payload = {
    "feature_1": 0.0,
    "feature_2": 0.0,
    "feature_3": 0.0,
    "feature_4": 0.0,
    "feature_5": 0.0,
    "feature_6": 0.0,
    "feature_7": 0.0,
    "feature_8": 0.0,
    "feature_9": 0.0,
    "feature_10": 0.0,
    "feature_11": 0.0,
    "feature_12": 0.0,
    "feature_13": 0.0,
}

response = requests.post(url, json=payload)

print(response.status_code)
print(response.json())
```

---

# 13. Prediction Response

A successful response contains the model prediction.

Conceptually:

```json
{
  "prediction": 12345.678
}
```

> **Note:** The exact response schema should follow the implementation in the repository.

### Response Flow

```text
Validated Request
      ↓
13 Features
      ↓
Feature Matrix
      ↓
XGBoost
      ↓
Prediction
      ↓
JSON Serialization
      ↓
HTTP Response
```

---

# 14. HTTP Status Codes

The API follows conventional HTTP semantics.

| Status Code | Meaning                             |
| ----------- | ----------------------------------- |
| `200`       | Successful request                  |
| `400`       | Invalid request                     |
| `422`       | Request validation failure          |
| `500`       | Internal server/model error         |
| `503`       | Service unavailable, if implemented |

> **Note:** Exact status-code behavior depends on the deployed API implementation.

---

# 15. Swagger / OpenAPI Documentation

FastAPI automatically exposes interactive API documentation.

### Swagger UI

```text
http://localhost:8000/docs
```

The Swagger interface allows developers to:

* Inspect endpoints
* View request schemas
* View response schemas
* Submit test requests
* Inspect responses
* Validate API behavior interactively

### OpenAPI Schema

```text
http://localhost:8000/openapi.json
```

The OpenAPI schema allows API metadata to be consumed by tools that understand the OpenAPI specification.

### ReDoc

```text
http://localhost:8000/redoc
```

ReDoc provides an alternative presentation of the API contract.

---

# 16. Local API Startup

The API can be started using the FastAPI application entry point.

A typical development command is:

```bash
uvicorn app:app --host 0.0.0.0 --port 8000
```

If the application is located in another module:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

> **Note:** The exact application module should match the repository implementation.

### Development Server Flow

```text
Developer
   ↓
Start Uvicorn
   ↓
FastAPI Application
   ↓
Load Model
   ↓
Initialize Inference Pipeline
   ↓
Expose Endpoints
```

---

# 17. Model Loading

The API loads the serialized production model required for inference.

The model artifact should be treated as a deployment dependency.

```text
API Startup
    ↓
Model Artifact
    ↓
Model Load
    ↓
Inference Ready
```

If the model cannot be loaded, the API should not be considered fully operational.

### Model Artifact Dependencies

```text
API Application
      +
Model Artifact
      +
Feature Contract
      +
Python Dependencies
      +
Runtime Configuration
```

Together, these components contribute to reproducible inference.

---

# 18. Dependency Management

The repository includes dependency configuration for the API runtime.

Typical dependencies include:

```text
fastapi
uvicorn
pydantic
xgboost
numpy
pandas
```

The exact package versions should be defined in the project's requirements configuration.

---

# 19. API and Production Inference

The API is connected to the lightweight production inference workflow developed during Phase 14.

```text
Phase 14
Production Inference
      ↓
Phase 15
FastAPI
      ↓
HTTP Interface
```

This ensures that the API is not an independent model implementation.

---

# 20. API and Phase-14 Validation

The project validates API predictions against Phase-14 inference results.

The validation objective is:

```text
Same Input
    ↓
Phase-14 Inference
    ↓
Prediction A

Same Input
    ↓
FastAPI
    ↓
Prediction B

Compare A vs B
```

The API therefore acts as a service interface around the validated inference workflow.

### Inference Consistency Principle

```text
Same Model
+
Same Feature Contract
+
Same Input
=
Consistent Prediction
```

This prevents the API from accidentally introducing different preprocessing or feature-ordering behavior.

---

# 21. Prediction Validation

The inference workflow validates prediction outputs for numerical validity.

The Phase-11 workflow included prediction-finiteness validation.

```text
Prediction
   ↓
Finite?
   ├── Yes → Return Response
   └── No  → Handle Failure
```

This prevents invalid numerical outputs from being treated as successful inference.

---

# 22. API Isolation

The API provides a clean separation between the user interface and machine-learning inference.

```text
Streamlit Dashboard
        │
        ▼
     Analysis


External Client
        │
        ▼
     FastAPI
        │
        ▼
 Production Inference
        │
        ▼
      Model
```

This allows the model to be consumed independently of the Streamlit application.

---

# 23. API and Dashboard

The Streamlit dashboard provides an interactive user interface.

FastAPI provides a programmatic interface.

```text
                AI Platform
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
      Streamlit            FastAPI
      Dashboard               │
          │                   │
          ▼                   ▼
      Human User         External Client
```

This separation improves architectural flexibility.

---

# 24. API and Sustainability Intelligence

The prediction service sits within the larger sustainability platform.

```text
Infrastructure Data
       ↓
Sustainability Intelligence
       ↓
Feature Engineering
       ↓
Forecasting
       ↓
FastAPI
       ↓
External Applications
```

The API therefore serves as an operational interface to the project's predictive intelligence layer.

---

# 25. API and MLflow

MLflow is used within the project's experiment and inference tracking architecture.

The responsibilities are separated:

```text
Model / Inference
      ↓
MLflow
      ↓
Parameters
Metrics
Artifacts
```

while:

```text
External Client
      ↓
FastAPI
      ↓
Inference
```

FastAPI provides the serving interface, while MLflow supports experiment and model-related tracking.

---

# 26. Docker Deployment

The API is designed to support containerized execution.

### Deployment Flow

```text
Dockerfile
    ↓
Build Image
    ↓
Install Dependencies
    ↓
Copy Application
    ↓
Load Runtime
    ↓
Start FastAPI
    ↓
Expose Port 8000
```

### Build Docker Image

```bash
docker build -t sustainable-infrastructure-api .
```

### Run Container

```bash
docker run -p 8000:8000 sustainable-infrastructure-api
```

> **Note:** The exact Docker configuration should follow the repository's `Dockerfile`.

---

# 27. Deployment Architecture

```text
                   GitHub Repository
                          │
                          ▼
                     Dockerfile
                          │
                          ▼
                    Docker Image
                          │
                          ▼
                  FastAPI Container
                          │
              ┌───────────┴───────────┐
              ▼                       ▼
          /health                  /predict
                                      │
                                      ▼
                              Production Model
```

---

# 28. API Reproducibility

A reproducible API deployment requires:

* Source code
* Model artifact
* Dependency specification
* Feature contract
* Runtime configuration
* Container configuration

Therefore:

```text
Source
+
Dependencies
+
Model
+
Configuration
=
Reproducible Runtime
```

---

# 29. API Testing Strategy

The API should be tested at multiple levels.

### Level 1 — Health Test

```http
GET /health
```

### Level 2 — Request Validation

```text
Invalid Request
      ↓
Expected Validation Error
```

### Level 3 — Prediction Test

```text
Valid Request
      ↓
Prediction
```

### Level 4 — Consistency Test

```text
API Prediction
      vs
Phase-14 Prediction
```

### Level 5 — Deployment Test

```text
Docker Container
      ↓
Health
      ↓
Prediction
```

---

# 30. API Health Monitoring

The `/health` endpoint provides the foundation for service-level health checks.

A production environment could use it with:

```text
Load Balancer
      ↓
Health Check
      ↓
GET /health
      ↓
Healthy / Unhealthy
```

This allows unhealthy containers or service instances to be detected.

---

# 31. API Error Handling

The API should distinguish between client errors and server/model errors.

### Client Error

```text
Invalid Input
    ↓
4xx Response
```

### Server / Model Error

```text
Model Runtime Failure
    ↓
5xx Response
```

This distinction improves API observability and client-side debugging.

---

# 32. API Observability

A production deployment should monitor:

* Request count
* Response latency
* Error rate
* Model inference latency
* Request validation failures
* Service availability
* Resource consumption

Conceptually:

```text
API
 ↓
Logs
 ↓
Metrics
 ↓
Monitoring
 ↓
Operational Visibility
```

---

# 33. API Security Considerations

The current project is a portfolio-oriented inference service and should not be interpreted as a fully hardened enterprise API.

Production deployment would require additional controls such as:

* Authentication
* Authorization
* API keys or OAuth
* TLS
* Rate limiting
* Request logging
* Abuse prevention
* Secret management
* Network controls
* Input sanitization
* Dependency vulnerability scanning

---

# 34. API and XAI

The current API primarily exposes forecasting inference.

The broader platform also contains XAI capabilities.

A potential future extension could be:

```text
POST /predict
      ↓
Prediction

POST /explain
      ↓
Prediction
+
Feature Contributions
```

An explanation endpoint is a potential future extension and is not a claim about the current API.

---

# 35. Phase-15 Validation

The project completed a final Phase-15 completion gate.

Validated areas included:

```text
✓ FastAPI Service
✓ Health Endpoint
✓ Prediction Endpoint
✓ Request Validation
✓ API Prediction Consistency
✓ Requirements Configuration
✓ Dockerfile
✓ .gitignore
✓ GitHub-Oriented Structure
✓ Deployment Configuration
✓ Machine-Readable Validation
```

---

# 36. API Engineering Principles

The API implementation follows these principles:

1. Keep inference lightweight.
2. Validate requests before inference.
3. Preserve the model feature contract.
4. Avoid silent feature transformations.
5. Keep model serving separate from UI logic.
6. Return machine-readable responses.
7. Provide health monitoring.
8. Preserve reproducibility.
9. Support containerized deployment.
10. Validate API predictions against the established inference pipeline.

---

# 37. Production Readiness Assessment

The API is **production-oriented**, but should not be represented as a fully enterprise-hardened service.

### Implemented

```text
✓ FastAPI
✓ Health endpoint
✓ Prediction endpoint
✓ Request validation
✓ Model inference
✓ Feature contract
✓ Prediction validation
✓ Docker packaging
✓ Deployment configuration
```

### Future Production Requirements

```text
○ Authentication
○ Authorization
○ TLS
○ Rate limiting
○ Observability
○ Distributed logging
○ Automated CI/CD
○ Model registry integration
○ Autoscaling
○ Security scanning
○ Secrets management
○ SLA monitoring
```

---

# 38. Complete End-to-End Workflow

```text
Client
  │
  │ POST /predict
  ▼
FastAPI
  │
  ▼
Request Validation
  │
  ▼
13-Feature Contract
  │
  ▼
Inference Pipeline
  │
  ▼
XGBoost Model
  │
  ▼
Prediction Validation
  │
  ▼
JSON Response
  │
  ▼
Client
```

---

# 39. Complete Platform Architecture

```text
Data
  ↓
Feature Engineering
  ↓
Sustainability Intelligence
  ↓
XGBoost Model
  ↓
Model Validation
  ↓
Production Inference
  ↓
FastAPI
  ↓
REST Interface
  ↓
Docker
  ↓
Deployment
```

The broader application architecture is:

```text
                    AI Platform
                        │
           ┌────────────┴────────────┐
           │                         │
           ▼                         ▼
     Streamlit Dashboard          FastAPI
           │                         │
           ▼                         ▼
      Human Users             External Systems
                                     │
                                     ▼
                            Production Inference
                                     │
                                     ▼
                               XGBoost Model
```

---

# 40. Final API Summary

The Sustainable Infrastructure Intelligence Platform provides a FastAPI-based inference service designed to expose the validated forecasting model through a clean REST interface.

The service includes:

* `GET /health`
* `POST /predict`
* Request validation
* 13-feature model contract
* Production inference
* Prediction validation
* API documentation
* Docker deployment support
* Deployment validation

The API establishes a clear boundary between external applications and the machine-learning inference layer.

---

# 41. M.Tech-Level Engineering Relevance

The API component demonstrates practical understanding of:

* REST API architecture
* Model serving
* Schema validation
* Feature contracts
* Production inference
* Service boundaries
* Containerization
* API testing
* Deployment architecture
* Reproducibility
* ML system integration

The implementation demonstrates the transition from:

```text
ML Experiment
```

to:

```text
ML Service
```

---

# 42. Hiring-Focused Engineering Value

From a hiring perspective, the API demonstrates that the project goes beyond:

```text
Train Model
     ↓
Save Model
```

and instead implements:

```text
Train / Validate Model
        ↓
Production Inference
        ↓
Feature Contract
        ↓
Request Validation
        ↓
REST API
        ↓
Docker
        ↓
Deployment Architecture
```

This is directly relevant to AI Engineering, ML Engineering, MLOps, and backend-oriented ML roles.

---

# 43. Final Engineering Statement

The API layer represents the transition of the project from an ML experimentation environment into a service-oriented AI engineering system.

The resulting system demonstrates not only the ability to build a forecasting model, but also the ability to **package, validate, expose, and operationalize machine-learning inference as a deployable software service**.
