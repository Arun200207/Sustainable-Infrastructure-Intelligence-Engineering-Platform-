# API Documentation

## Sustainable Infrastructure Intelligence Platform

> Production-oriented FastAPI inference service for exposing the validated sustainability forecasting model through a documented REST API.

---

## 1. API Overview

The Sustainable Infrastructure Intelligence Platform exposes its production inference workflow through a FastAPI service.

The API provides a lightweight interface between the trained machine-learning model and external applications.

The architecture is:

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
Prediction
  ↓
JSON Response
