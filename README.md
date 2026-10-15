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

## Architecture

