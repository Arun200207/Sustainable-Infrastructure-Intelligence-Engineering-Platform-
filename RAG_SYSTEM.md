# RAG System

## Sustainable Infrastructure Intelligence Platform

> **Technical documentation of the retrieval-augmented generation architecture, knowledge engineering pipeline, semantic retrieval layer, grounding strategy, validation, and LLM integration.**

---

# 1. Overview

The Sustainable Infrastructure Intelligence Platform includes a **Retrieval-Augmented Generation (RAG)** system designed to provide natural-language access to validated sustainability intelligence.

The RAG system connects:

```text
Validated Sustainability Intelligence
              ↓
       Knowledge Engineering
              ↓
      Sentence Transformers
              ↓
       384-D Embeddings
              ↓
             FAISS
              ↓
        Semantic Retrieval
              ↓
        Retrieved Context
              ↓
          Transformer LLM
              ↓
        Grounded Response
```

The objective is to allow users to ask questions about infrastructure sustainability without requiring direct interaction with the underlying analytical artifacts.

---

# 2. Why RAG Was Used

A conventional LLM cannot be assumed to know the project's dynamically generated sustainability metrics.

The project contains information that is specific to the generated analytical dataset, including:

* Building-level sustainability metrics
* Energy intelligence
* Carbon intelligence
* Cost intelligence
* Rankings
* Project-level KPIs

RAG provides a mechanism for connecting the LLM to this project-specific knowledge.

```text
LLM Alone
   ↓
General Knowledge

RAG + Project Knowledge
   ↓
Project-Specific Intelligence
```

The architecture therefore separates:

**Knowledge retrieval** from **language generation**.

---

# 3. RAG Objectives

The RAG system was designed to:

1. Retrieve relevant sustainability knowledge.
2. Ground generated responses in project-specific information.
3. Support global KPI questions.
4. Support building-level questions.
5. Connect structured analytics with natural-language interaction.
6. Reduce dependence on unsupported model memory.
7. Provide a user-facing intelligence interface through the dashboard.

---

# 4. Knowledge Base

The validated knowledge base contains:

**1,579 knowledge records**

The knowledge base consists conceptually of:

```text
1,579 Knowledge Records
        │
        ├── Global Sustainability Knowledge
        │
        └── Building-Level Sustainability Knowledge
```

---

# 5. Knowledge Engineering

The knowledge layer is derived from the validated sustainability intelligence layer.

The transformation is:

```text
Structured Analytics
       ↓
Knowledge Representation
       ↓
Retrieval-Ready Records
```

This creates an abstraction boundary between numerical computation and natural-language generation.

---

# 6. Knowledge Record Types

## Global Knowledge

Global records represent project-wide sustainability information.

Examples include:

* Total building count
* Total energy
* Total carbon
* Total energy cost
* Aggregate sustainability metrics
* Global rankings
* Overall project statistics

---

## Building Knowledge

Building-level records represent individual infrastructure entities.

They can contain information such as:

* Building identity
* Energy performance
* Carbon metrics
* Cost metrics
* Sustainability ranking
* Building-level analytical context

This allows users to ask questions about specific buildings.

---

# 7. Knowledge Layer Design

The knowledge layer can be represented as:

```text
                 SUSTAINABILITY DATA
                         │
                         ▼
              VALIDATED INTELLIGENCE
                         │
                         ▼
                KNOWLEDGE ENGINEERING
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
          Global Records       Building Records
              │                     │
              └──────────┬──────────┘
                         ▼
                  Knowledge Base
```

---

# 8. Why the Knowledge Layer Matters

The knowledge layer prevents the RAG system from directly depending on raw analytical tables.

Instead:

```text
Raw Data
   ↓
Validated Analytics
   ↓
Knowledge Representation
   ↓
Semantic Retrieval
```

This improves separation of concerns and makes the RAG system easier to reason about.

---

# 9. Embedding Model

The system uses **Sentence Transformers** to convert knowledge records into semantic vectors.

Conceptually:

```text
Knowledge Record
      ↓
Sentence Transformer
      ↓
384-Dimensional Embedding
```

Each knowledge record therefore has a numerical semantic representation.

---

# 10. Embedding Dimension

The generated embedding vectors have:

**384 dimensions**

Therefore, the semantic representation of each knowledge record can be expressed conceptually as:

```text
Record
 ↓
[ e₁, e₂, e₃, ..., e₃₈₄ ]
```

These vectors are subsequently indexed for similarity retrieval.

---

# 11. Semantic Retrieval

The retrieval system uses vector similarity rather than relying exclusively on exact keyword matching.

For a user query:

```text
User Question
      ↓
Query Embedding
      ↓
Vector Similarity Search
      ↓
Relevant Knowledge Records
```

This enables semantically related information to be retrieved even when the wording of the question differs from the wording of the knowledge record.

---

# 12. FAISS Vector Store

FAISS provides the vector-search layer.

Its role is:

* Store embeddings
* Index vectors
* Perform similarity search
* Return relevant knowledge records

Conceptually:

```text
                 Knowledge Records
                       │
                       ▼
                  Embeddings
                       │
                       ▼
                    FAISS
                       │
                       ▲
                       │
                  Query Vector
```

FAISS therefore acts as the retrieval engine between the knowledge base and the generation layer.

---

# 13. Query Processing

A RAG query follows this conceptual pipeline:

```text
User Question
      ↓
Query Processing
      ↓
Query Embedding
      ↓
FAISS Search
      ↓
Top Relevant Records
      ↓
Context Assembly
      ↓
LLM
      ↓
Response
```

The LLM is not the first component that processes project knowledge.

Retrieval occurs before generation.

---

# 14. Context Construction

Retrieved records are assembled into a context representation for the LLM.

Conceptually:

```text
User Query
     +
Retrieved Knowledge
     ↓
Generation Context
     ↓
Transformer LLM
```

The retrieved knowledge provides the project-specific information used to formulate the response.

---

# 15. Grounded Generation

The objective of grounding is to make generated responses depend on retrieved project knowledge.

The intended behavior is:

```text
Question
   ↓
Retrieve Evidence
   ↓
Provide Evidence to LLM
   ↓
Generate Answer
```

This differs from simply sending a user question directly to an LLM.

---

# 16. RAG vs Direct LLM

## Direct LLM

```text
Question
   ↓
LLM
   ↓
Answer
```

The model primarily relies on its pretrained knowledge and generation behavior.

---

## RAG

```text
Question
   ↓
Retriever
   ↓
Project Knowledge
   ↓
LLM
   ↓
Grounded Answer
```

The second architecture provides access to the project's generated knowledge layer.

---

# 17. Retrieval Scope

The RAG system supports two major analytical scopes.

### Global Scope

Questions about the complete infrastructure population.

Examples:

```text
What is the total energy consumption?
What is the total carbon impact?
What is the total energy cost?
```

### Building Scope

Questions about individual buildings.

Examples:

```text
What is the sustainability performance of a building?
Which building has stronger carbon performance?
How does a particular building compare with others?
```

---

# 18. Global KPI Retrieval

Global KPI questions are answered using project-level knowledge records.

Conceptually:

```text
Global Question
      ↓
Query Embedding
      ↓
FAISS
      ↓
Global KPI Knowledge
      ↓
LLM
      ↓
Response
```

This allows the RAG interface to expose project-level sustainability intelligence conversationally.

---

# 19. Building-Level Retrieval

Building-level queries follow a similar workflow.

```text
Building Question
      ↓
Query Embedding
      ↓
FAISS
      ↓
Building Knowledge
      ↓
LLM
      ↓
Response
```

The retrieved context can contain building-specific sustainability information.

---

# 20. Retrieval and Generation Separation

The architecture intentionally separates retrieval from generation.

### Retriever

Responsible for:

* Finding relevant information
* Ranking semantic similarity
* Returning knowledge records

### Generator

Responsible for:

* Understanding retrieved context
* Producing natural-language responses
* Formatting the final answer

```text
Retriever ≠ Generator
```

This separation makes the system easier to debug and evaluate.

---

# 21. RAG Validation

The project validated the RAG system against multiple query categories.

Validation included:

* Global KPI questions
* Building-level questions
* Knowledge consistency
* Cross-layer consistency

The objective was to verify that the generated responses were connected to the validated sustainability intelligence layer.

---

# 22. Cross-Layer Validation

RAG validation does not stop at checking whether an answer was generated.

The system considers the relationship:

```text
Sustainability Intelligence
          ↓
Knowledge Base
          ↓
Vector Index
          ↓
Retrieved Context
          ↓
Generated Response
```

A valid response should remain consistent with the authoritative upstream intelligence.

---

# 23. Artifact Staleness

A significant engineering consideration was downstream artifact staleness.

The sustainability layer underwent correction of historical carbon and cost calculations.

That created a dependency problem:

```text
Corrected Sustainability Data
             ↓
Existing Knowledge Records
             ↓
Potentially Stale
```

The correct engineering response was to rebuild downstream RAG artifacts.

```text
Corrected Intelligence
        ↓
Knowledge Rebuild
        ↓
Embedding Rebuild
        ↓
FAISS Rebuild
        ↓
RAG Validation
```

---

# 24. Why Rebuilding Was Necessary

Embeddings are derived representations.

If the source knowledge changes:

```text
Knowledge A
   ↓
Embedding A
```

and the knowledge becomes:

```text
Knowledge B
```

then the previous embedding may no longer represent the current knowledge.

Therefore:

> **Changing authoritative knowledge can invalidate downstream vector artifacts.**

This is an important data-lineage principle in production RAG systems.

---

# 25. RAG Artifact Lineage

The RAG lineage is:

```text
Sustainability Dataset
        ↓
Sustainability Intelligence
        ↓
Knowledge Records
        ↓
Embeddings
        ↓
FAISS Index
        ↓
Retrieved Context
        ↓
LLM Response
```

Each downstream layer depends on the correctness of the previous layer.

---

# 26. RAG Integrity Model

The system follows:

```text
Authoritative
     ↓
Derived
     ↓
Indexed
     ↓
Retrieved
     ↓
Generated
```

The further a result moves from the authoritative layer, the more dependency layers exist between the original data and the final response.

This makes upstream validation essential.

---

# 27. Hallucination Risk

RAG reduces dependence on unsupported generation but does not mathematically eliminate hallucination.

Potential failure modes include:

* Incorrect retrieval
* Missing relevant context
* Ambiguous queries
* Insufficient knowledge coverage
* Incorrect interpretation by the LLM
* Unsupported generation beyond retrieved evidence

Therefore, RAG should be treated as a grounding mechanism rather than a guarantee of factual correctness.

---

# 28. Retrieval Failure Modes

## No Relevant Result

The query may not match available knowledge.

### Risk

The generator may lack sufficient context.

---

## Incorrect Result

The retriever may return semantically similar but analytically inappropriate records.

### Risk

The LLM may generate a plausible but incorrect answer.

---

## Partial Context

Only part of the required information may be retrieved.

### Risk

The response may be incomplete.

---

# 29. Generation Failure Modes

Even with correct retrieval, the LLM can:

* Misinterpret context
* Combine unrelated facts
* Generate unsupported conclusions
* Overstate certainty
* Produce incomplete responses

Therefore, production RAG systems should evaluate both:

```text
Retrieval Quality
        +
Generation Quality
```

---

# 30. RAG Evaluation Framework

A future production-grade evaluation framework should separately measure:

## Retrieval

* Precision@K
* Recall@K
* MRR
* Relevant-context rate

## Generation

* Groundedness
* Faithfulness
* Relevance
* Completeness
* Answer consistency

Conceptually:

```text
Query
 ↓
Retriever Evaluation
 ↓
Context Quality
 ↓
Generator Evaluation
 ↓
Final Answer Quality
```

---

# 31. Current Validation Scope

The project performed functional validation rather than presenting the RAG layer as a fully benchmarked enterprise evaluation system.

Validated areas include:

```text
✓ Knowledge base generation
✓ 1,579 knowledge records
✓ 384-D embeddings
✓ FAISS vector store
✓ Global KPI questions
✓ Building-level questions
✓ Rebuilt stale RAG artifacts
✓ Cross-layer validation
```

---

# 32. RAG + Dashboard Integration

The RAG assistant is integrated into the Streamlit dashboard.

The user-facing workflow is:

```text
Dashboard
    ↓
User Question
    ↓
RAG Pipeline
    ↓
Retrieval
    ↓
LLM
    ↓
Answer
    ↓
Dashboard
```

This turns the underlying analytical knowledge base into an interactive interface.

---

# 33. RAG + Reporting Integration

RAG exists alongside automated reporting rather than replacing it.

```text
Sustainability Intelligence
        │
        ├──────────────► Reporting
        │
        └──────────────► Knowledge
                              ↓
                           RAG + LLM
```

Reports provide deterministic structured outputs.

RAG provides conversational access.

---

# 34. Deterministic vs Generative Intelligence

The platform deliberately contains both.

## Deterministic

Examples:

* Energy totals
* Carbon totals
* Cost totals
* Rankings
* Calculated metrics

## Generative

Examples:

* Natural-language explanations
* Conversational responses
* Contextual summaries

The architecture therefore avoids forcing every analytical requirement through an LLM.

---

# 35. Why This Matters

For quantitative sustainability information:

```text
Calculation → Deterministic System
```

For conversational access:

```text
Question → Retrieval → LLM
```

This separation is preferable to asking an LLM to independently calculate authoritative project metrics.

---

# 36. RAG Knowledge Freshness

The knowledge layer is derived from analytical artifacts.

Therefore, knowledge freshness depends on upstream artifact freshness.

Conceptually:

```text
Source Update
      ↓
Analytical Update
      ↓
Knowledge Update
      ↓
Embedding Update
      ↓
FAISS Update
      ↓
RAG Refresh
```

A production implementation should automate this dependency chain.

---

# 37. Future RAG Improvements

Potential improvements include:

## Hybrid Search

Combine:

```text
Keyword Search
+
Vector Search
```

to improve retrieval for exact building names, identifiers, and semantic questions.

---

## Reranking

Use a reranker after initial vector retrieval.

```text
FAISS Top-K
    ↓
Reranker
    ↓
Best Context
```

---

## Metadata Filtering

Use metadata such as:

* Building ID
* Knowledge type
* Geographic context
* Metric category

to constrain retrieval before generation.

---

## Query Classification

Classify the user's question before retrieval.

```text
Question
   ↓
Query Classifier
   ├── Global KPI
   ├── Building
   ├── Ranking
   ├── Forecast
   └── Sustainability Explanation
```

---

## Conversational Memory

Maintain controlled conversational context while ensuring that historical context does not override authoritative project data.

---

# 38. Future Grounding Controls

A production system could introduce explicit grounding checks:

```text
Retrieved Context
       ↓
LLM Response
       ↓
Grounding Validator
       ↓
Supported?
   ┌───┴───┐
  YES      NO
   ↓        ↓
Return    Regenerate /
Answer    Abstain
```

This would improve reliability for decision-support use cases.

---

# 39. Security Considerations

A production RAG deployment should address:

* Prompt injection
* Malicious retrieved content
* Sensitive-data leakage
* Unauthorized knowledge access
* Tool abuse
* Excessive query volume
* Output filtering
* Audit logging

The current system is a portfolio-grade RAG implementation and does not claim to be a hardened enterprise GenAI security architecture.

---

# 40. RAG Architecture

The complete RAG architecture is:

```text
                 USER
                   │
                   ▼
                QUERY
                   │
                   ▼
          Query Embedding
                   │
                   ▼
             FAISS Search
                   │
                   ▼
          Retrieved Knowledge
                   │
                   ▼
          Context Construction
                   │
                   ▼
             Transformer LLM
                   │
                   ▼
            Grounded Answer
                   │
                   ▼
               DASHBOARD
```

---

# 41. End-to-End Knowledge Pipeline

```text
Building / Energy / Environmental Data
                  ↓
       Sustainability Intelligence
                  ↓
          Knowledge Engineering
                  ↓
          1,579 Knowledge Records
                  ↓
       Sentence Transformer Model
                  ↓
          384-D Embeddings
                  ↓
             FAISS Index
                  ↓
          Semantic Retrieval
                  ↓
          Retrieved Context
                  ↓
          Transformer LLM
                  ↓
        Natural-Language Answer
```

---

# 42. Engineering Decisions

## Sentence Transformers

Chosen to create compact semantic representations of project-specific knowledge.

## 384-D Embeddings

Provide a fixed-dimensional semantic representation suitable for vector indexing.

## FAISS

Provides an efficient local similarity-search mechanism appropriate for the current project scale.

## RAG

Allows project-specific knowledge to be introduced at query time rather than requiring the LLM itself to contain the project data.

## Separate Knowledge Layer

Provides a clean boundary between analytical computation and generative AI.

---

# 43. RAG System Strengths

The implementation demonstrates:

* Project-specific knowledge retrieval
* Semantic vector search
* Structured knowledge engineering
* Building-level retrieval
* Global KPI retrieval
* LLM integration
* Cross-layer validation
* Artifact lineage awareness
* Stale-artifact recovery
* Dashboard integration

These characteristics move the implementation beyond a basic:

> **"LLM chatbot"**

architecture.

---

# 44. RAG System Limitations

The current system does not claim to provide:

* Enterprise-scale distributed retrieval
* Automated retrieval benchmarking
* Continuous knowledge refresh
* Fully automated grounding evaluation
* Advanced reranking
* Comprehensive prompt-injection defense
* Production-grade multi-tenant authorization

These are appropriate future engineering directions.

---

# 45. Hiring-Focused Engineering Summary

The strongest engineering story of the RAG implementation is not simply:

> "I used an LLM."

It is:

> **I converted validated domain analytics into a structured knowledge layer, generated semantic embeddings, indexed them with FAISS, implemented grounded retrieval, connected retrieved context to an LLM, validated global and building-level queries, and rebuilt downstream RAG artifacts when upstream sustainability calculations changed.**

This demonstrates understanding of:

* Data lineage
* Knowledge engineering
* Embeddings
* Vector search
* Retrieval
* Generation
* Grounding
* Artifact dependency
* Validation
* Production-oriented GenAI architecture

---

# 46. Final RAG Summary

The RAG subsystem can be summarized as:

```text
       VALIDATED SUSTAINABILITY DATA
                    │
                    ▼
           KNOWLEDGE ENGINEERING
                    │
                    ▼
             1,579 RECORDS
                    │
                    ▼
        SENTENCE TRANSFORMERS
                    │
                    ▼
          384-D EMBEDDINGS
                    │
                    ▼
                 FAISS
                    │
             ┌──────┴──────┐
             │             │
             ▼             ▼
        GLOBAL QUERY   BUILDING QUERY
             │             │
             └──────┬──────┘
                    ▼
              RETRIEVED CONTEXT
                    │
                    ▼
             TRANSFORMER LLM
                    │
                    ▼
            GROUNDED RESPONSE
                    │
                    ▼
               DASHBOARD
```

The RAG architecture therefore provides a controlled bridge between **validated quantitative sustainability intelligence** and **natural-language AI interaction**.

Its central engineering principle is:

> **Retrieve project-specific evidence first; generate language second.**
