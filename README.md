# Sanctum AI 🧠🛡️

![Go](https://img.shields.io/badge/go-1.22+-00ADD8?logo=go)
![Security](https://img.shields.io/badge/Security-PII%20Redaction-red)
![Governance](https://img.shields.io/badge/Cost-FinOps%20Guardrails-green)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

**Sanctum AI** is a high-performance Governance Layer designed to sit between corporate infrastructure and external LLM providers.

In an enterprise environment, direct access to public APIs (e.g., GPT-4) introduces significant risks regarding data leakage and uncontrolled costs. Sanctum AI mitigates these risks via a **Smart Proxy** architecture that enforces security and budget policies in real-time without adding perceptible latency.

## 🏗️ Architecture & Design Decisions

### 1. Zero-Trust PII Masking
The system intercepts prompts and identifies sensitive entities (SSN, Email, IBAN) using a hybrid approach of optimized Regex and lightweight NLP (Named Entity Recognition).
* **Strategy:** Sensitive data is replaced with synthetic tokens (e.g., `<PERSON_1>`) before leaving the secure perimeter.
* **Re-hydration:** Upon receiving the LLM response, tokens are swapped back to the original data, ensuring a seamless user experience while keeping the LLM provider "blind" to PII.

### 2. FinOps & Cost Guardrails
Sanctum implements semantic rate-limiting and departmental budgets using **Redis** (Token Bucket algorithm).
* *Scenario:* "The Marketing Team has a $500/day budget. At 80% usage, traffic is throttled; at 100%, it is blocked."

### 3. Model Agnostic Routing
Requests are dynamically routed based on complexity. Simple queries are offloaded to lower-cost models (e.g., local Llama 3), while complex reasoning tasks are sent to SOTA models, optimizing TCO (Total Cost of Ownership).

## 🛠️ Tech Stack

* **Core:** Go (Golang)
* **State Store:** Redis (for Rate Limiting & Token Mapping)
* **Observability:** OpenTelemetry (Traces) + Prometheus (Metrics)
* **Deployment:** Docker / Kubernetes

## 🚀 Quick Start

```bash
# Clone the repository
git clone [https://github.com/yourusername/sanctum-ai.git](https://github.com/yourusername/sanctum-ai.git)

# Start services (Redis + Sanctum)
docker-compose up -d

# Test the proxy (PII Redaction)
curl -X POST http://localhost:8080/v1/chat/completions \
  -d '{"prompt": "Contact John Doe at john@example.com"}'
# Response prompt received by LLM: "Contact <PERSON_1> at <EMAIL_1>"
