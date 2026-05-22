# UnMask 🕵️‍♂️

> An AI-powered transparency layer and public-interest audit engine designed to detect and expose misleading commercial claims in coaching-institute advertisements.

[![Stack: Next.js](https://img.shields.io/badge/Frontend-Next.js%2014-black?logo=nextdotjs)](https://nextjs.org/)
[![Backend: FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688?logo=fastapi)](https://fastapi.tiangolo.com/)
[![Database: Supabase](https://img.shields.io/badge/Database-Supabase-3ECF8E?logo=supabase)](https://supabase.com/)
[![AI Framework: LangChain](https://img.shields.io/badge/AI-LangChain%20%2F%20OpenAI-61D3B4)](https://python.langchain.com/)

UnMask allows users to anonymously upload images of coaching institute print/digital advertisements. The platform executes an automated vision-to-structured-data pipeline, extracts topper results and fine-print disclaimers, surfaces cross-institute conflicts (e.g., the same topper claimed by three different institutes), and auto-generates legal-ready evidence PDFs compliant with the Central Consumer Protection Authority (CCPA) guidelines.

### ⚡ Performance Metrics
- **Processing Speed:** `< 5 seconds` per ad image from upload to structured audit report.
- **Data Yield:** 500+ advertisements scanned and indexed during initial testing.
- **Insights:** 12+ multi-institute structural conflicts surfaced automatically.

---

## 🏗️ Architecture & Pipeline Flow

The platform relies on a decoupled, async pipeline built to execute intensive multi-stage vision parsing within serverless timeout boundaries.

```mermaid
graph TD
    User[User Anonymous Upload] -->|Multipart Form| API[FastAPI Edge Endpoint]
    API -->|Async Read| OCR[Vision API / OCR Extraction Engine]
    OCR -->|Raw Text Blocks| LC[LangChain + OpenAI Structured Output Engine]
    LC -->|Pydantic Validation| DB[(PostgreSQL + pgvector DB)]
    DB -->|Trigger Conflict Check| Conflict[Cross-Institute Aggregator]
    Conflict -->|Match Found| Alert[Surface Discrepancies & Hydrate PDF Matrix]
    Alert -->|Worker| PDF[Auto-Generated CCPA Evidence PDF]
