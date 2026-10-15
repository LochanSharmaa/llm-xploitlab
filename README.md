<!--
  LLM-XploitLab — GitHub README
  Original concept and architecture proposed by Lochan Sharma.
-->

<div align="center">

# LLM-XploitLab

### A Unified Oracle-Verified Vulnerability Discovery Engine for the LLM Stack

**Solving the Verification Crisis in AI Security**

[![Status](https://img.shields.io/badge/status-active%20development-orange.svg)](#roadmap)
[![License](https://img.shields.io/badge/license-Apache%202.0-green.svg)](#license)
[![Python](https://img.shields.io/badge/python-3.11%2B-blue.svg)](https://www.python.org/)
[![OWASP](https://img.shields.io/badge/OWASP-LLM%20Top%2010%202026%20%7C%20Agentic%20Top%2010%202026-red.svg)](#owasp-mapping)

**Proposed and architected by [Lochan Sharma](https://github.com/lochan-sharma)**

</div>

---

## Overview

**Research system, locally validated — not deployed, not independently validated. Runs open-source cyber LLMs against real LLM stacks at a calibrated G3b verification threshold targeting >95% specificity.**

LLM-XploitLab is the first automated vulnerability discovery engine that unifies five critical capabilities under a single verification pipeline: (1) G3b causal patched-counterfactual oracle verification, (2) adaptive cross-server MCP parasitic chain exploitation, (3) corpus-level RAG poisoning testing, (4) concurrency-aware serving infrastructure fuzzing, and (5) multi-agent trust boundary graph analysis.

Every finding is verified through a deterministic oracle that eliminates the false-positive crisis plaguing current LLM vulnerability research, and mapped dually to the **OWASP LLM Top 10 2026** and **OWASP Agentic Applications Top 10 2026**.

> **Core Thesis:** Current LLM vulnerability research produces 67% false positives on patched builds and achieves only 45% oracle specificity. LLM-XploitLab fixes this with a G3b-verified pipeline targeting >95% specificity and <5% false-positive rate.

---

## The Problem

### The Verification Crisis in AI Security

A 2026 pre-registered reproducibility audit ([arXiv:2608.09567](https://arxiv.org/abs/2608.09567)) audited 104 papers (2023–2026) on LLM-driven vulnerability discovery. The findings are damning:

| Metric | Observed Value | Implication |
|---|---|---|
| Papers with publicly reachable artifacts | 59 / 104 (56.7%) | Nearly half of research cannot be independently verified |
| Artifacts completing declared workflow (R0) | 10 / 18 (55.6%) | Half of artifacts do not run |
| **Patched-counterfactual audits still triggering on patched build** | **20 / 30 (67%)** | **Two-thirds of "verified" vulnerabilities are false positives** |
| **Matched negative controls still triggering on benign input** | **7 / 19 (37%)** | Over one-third cannot distinguish benign from malicious |
| **Oracle sensitivity** | **60%** | 40% false-negative rate |
| **Oracle specificity** | **45%** | **More than half of positive results are false** |
| CVE identifier consistency | 58 / 102 (56.9%) | Over half of CVE mappings are misaligned |

> *"A trigger on the vulnerable build is not evidence of CVE-specific reproduction without a clean patched counterfactual."*
> — Reproducibility Audit, 2026

### Why Existing Tools Fail

| Tool | Coverage | Oracle Verification | Cross-Stack | Real CVEs |
|---|---|---|---|---|
| **GRIEF** | Serving only | Controlled replay | ❌ | 2 |
| **RIPE-II** | RAG only | LLM judge | ❌ | 0 |
| **agent-boundary-scan** | Multi-agent static | None | ❌ | 0 |
| **mcp-scan** | MCP static | None | ❌ | 0 |
| **RealVuln** | Code only | None | ❌ | 0 |
| **LLM-XploitLab** | **Agent + MCP + Serving + RAG + Multi-Agent** | **G3b patched-counterfactual** | **✅ 5/5** | **Target: ≥5** |

**No existing work combines three or more pillars, and none performs patched-counterfactual verification.**

---

## Key Contributions

1. **G3b Causal Patched-Counterfactual Oracle** — A deterministic verification protocol implementing the G1→G2→G3a→G3b evidence ladder, with a confusion-matrix-calibrated oracle targeting **>95% specificity**.
2. **Adaptive Cross-Server MCP Exploitation** — The first engine to test **multi-server MCP deployments** for tool shadowing, registry rebinding, OAuth proxy token reuse, cross-tenant IDOR, and temporal persistence attacks.
3. **Corpus-Level RAG Poisoning Detection** — Cross-component propagation modeling that corrects for the **>5× cosine underreporting** identified by RIPE-II.
4. **Concurrency-Aware Serving Fuzzer** — Detects the **>35% of serving-layer bugs** that manifest as silent, non-crash anomalies (KV-cache isolation failures, cross-request contamination).
5. **Multi-Agent Trust Boundary Graph Analysis** — First dynamic validation of confused-deputy (**CWE-441**) escalation paths in multi-agent LLM systems.
6. **Dual OWASP Mapping** — Every finding mapped to **OWASP LLM Top 10 2026** (LLM01–LLM10) and **OWASP Agentic Applications Top 10 2026** (ASI01–ASI10).

---

---

## The Five Pillars

### Pillar 1 — G3b Causal Patched-Counterfactual Oracle

A deterministic verification protocol implementing the **G1–G3b evidence ladder**. A vulnerability is confirmed **only** when all four levels pass:

| Level | Test | Pass Condition |
|---|---|---|
| **G1** | Vulnerable build trigger | Signal fires |
| **G2** | Patched build test | Signal must **NOT** fire on signature-preserving patched build |
| **G3a** | Matched negative control | Signal must **NOT** fire on benign input |
| **G3b** | Cross-version consistency | Signal fires on **≥3 vulnerable versions**, absent on all patched counterparts |

**Target metrics:** Specificity >95% · Sensitivity >90% · FP Rate <5% · CVE Identifier Integrity >98%

### Pillar 2 — Adaptive Cross-Server MCP Parasitic Chain Exploitation

Tests **multi-server MCP deployments** where a single compromised server can weaponize adjacent trusted servers.

**Attack classes tested:**

- **Tool Shadowing** — Malicious server registers tool with same name as a legitimate server's tool
- **Registry Rebinding** — Tool definition changes after approval, redirecting to malicious endpoint
- **OAuth Proxy Token Reuse** — Token issued for the wrong MCP server resource
- **Cross-Tenant IDOR** — User A calls User B's tool on a shared MCP gateway
- **STDIO Command Injection** — Unsanitized OS command execution via MCP transport
- **Temporal Persistence** — Attack adapts across sessions and server restarts

### Pillar 3 — Corpus-Level RAG Poisoning Detection

Builds on RIPE-II ([IEEE DSN 2026](https://www.ieee-security.org/)), which demonstrated poisoned documents reach the model context in **up to 97% of queries** and rank first in **94% of cases**.

- Two-stage evaluation: **retrieval exposure** and **generation compliance** measured separately
- Calibrated LLM judge correcting for **>5× cosine underreporting**
- Cross-component propagation modeling across ingestion → chunking → embedding → retrieval → reranking → generation

### Pillar 4 — Concurrency-Aware Serving Infrastructure Fuzzing

Treats **timed multi-request traces** as first-class inputs. Detects the **>35% of serving bugs** that manifest as silent, non-crash anomalies:

- **KV-Cache Isolation Failures** — Cross-request state contamination
- **Noisy-Neighbor Performance Pathology** — Cost externalization without crash
- **Silent Output Corruption** — Log-probability divergence across runs

**Targets:** vLLM, SGLang, TGI

### Pillar 5 — Multi-Agent Trust Boundary Graph Analysis

Models agent-to-agent communication as a **directed trust graph** where the trust boundary is a graph edge, not a function call. Detects the structural recurrence of the confused-deputy problem (**CWE-441**) in multi-agent LLM systems.

- Taint-path enumeration from attacker-influenced sources to privileged actions
- Semantic manipulation validation without protocol violations
- Cross-framework testing: LangGraph, CrewAI, AutoGen, Google ADK, A2A-MCP

---

## OWASP Mapping

Every finding is mapped dually:

| OWASP LLM Top 10 2026 | OWASP Agentic Applications Top 10 2026 |
|---|---|
| LLM01: Prompt Injection | ASI01: Agent Goal Hijack |
| LLM02: Sensitive Information Disclosure | ASI02: Tool Misuse |
| LLM03: Excessive Agency | ASI03: Privilege Compromise |
| LLM04: Supply Chain | ASI04: Supply Chain |
| LLM05: Data / Model Poisoning | ASI05: Memory Poisoning |
| LLM06: Unbounded Consumption | ASI06: Resource Abuse |
| LLM07: Misinformation | ASI07: Misinformation |
| LLM08: Hidden Context Exposure | ASI08: Context Leakage |
| LLM09: Vector and Embedding Weaknesses | ASI09: Vector Manipulation |
| LLM10: Improper Output Handling | ASI10: Output Manipulation |

---

## Quick Start

### Prerequisites

- **Python** 3.11 or higher
- **Docker** 24.0 or higher (for sandboxed target deployment)
- **GPU** with ≥48GB VRAM (recommended for local model serving)
- **CUDA** 12.1 or higher

### Clone the Repository

```bash
git clone https://github.com/lochan-sharma/llm-xploitlab.git
cd llm-xploitlab
## Architecture

