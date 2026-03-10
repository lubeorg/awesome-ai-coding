---
name: owasp-ai-appsec-playbook
description: Comprehensive security review and remediation playbook aligned to OWASP Top 10:2025 and OWASP LLM Top 10:2025. This skill should be used when assessing web apps, APIs, RAG systems, or agentic AI workflows for vulnerabilities, prioritizing risks, and producing concrete code/config/process fixes with verification tests and a phased improvement roadmap.
license: CC BY-SA 4.0 (OWASP-aligned reference usage)
---

# OWASP AI AppSec Playbook

## Overview

Run a full-spectrum security review that covers both traditional web/API risks and AI-native risks. Produce concrete guidance to improve security posture, not just a vulnerability list.

Use this skill as a review engine with an output contract that always includes:

1. Prioritized findings with severity, confidence, and evidence.
2. Mapped OWASP controls violated.
3. Concrete remediation steps across code, configuration, and process.
4. Verification checks/tests that prove closure.
5. A phased improvement roadmap (quick wins, medium-term, long-term).

## When To Use

Use this skill when requests include any of the following:

- Security review, audit, penetration test triage, vulnerability assessment.
- OWASP Top 10 review (traditional web/API).
- LLM or GenAI security review (prompt injection, RAG risks, agent/tooling risks).
- Remediation planning after findings from scanners, bug bounty, or incidents.
- Secure architecture hardening plans for TypeScript/Node/React systems.

Do not use this skill for purely non-security product planning.

## Required Inputs

Collect the minimum context before scoring findings:

- System type: web app, API, RAG app, agentic app, or hybrid.
- Data sensitivity: PII, financial, health, credentials, trade secrets.
- Trust boundaries: browser, backend, model provider, vector DB, tools, third-party services.
- Authentication and authorization model.
- Deployment model and environment separation.
- Existing controls: WAF, rate limits, SAST/DAST, dependency scanning, logging/monitoring.

If critical context is missing, ask targeted questions first, then continue with explicit assumptions.

## Risk Catalogs To Apply (Equal Weighting)

### OWASP Top 10:2025

- A01:2025 Broken Access Control
- A02:2025 Security Misconfiguration
- A03:2025 Software Supply Chain Failures
- A04:2025 Cryptographic Failures
- A05:2025 Injection
- A06:2025 Insecure Design
- A07:2025 Authentication Failures
- A08:2025 Software or Data Integrity Failures
- A09:2025 Security Logging and Alerting Failures
- A10:2025 Mishandling of Exceptional Conditions

### OWASP LLM Top 10:2025

- LLM01:2025 Prompt Injection
- LLM02:2025 Sensitive Information Disclosure
- LLM03:2025 Supply Chain
- LLM04:2025 Data and Model Poisoning
- LLM05:2025 Improper Output Handling
- LLM06:2025 Excessive Agency
- LLM07:2025 System Prompt Leakage
- LLM08:2025 Vector and Embedding Weaknesses
- LLM09:2025 Misinformation
- LLM10:2025 Unbounded Consumption

## Workflow

### Phase 1: Triage Scope

1. Identify architecture and trust boundaries.
2. Classify attack surfaces per component.
3. Decide review depth by risk tier:
- Tier 1: internet-facing + sensitive data + broad privileges.
- Tier 2: internet-facing + limited sensitivity or partial privileges.
- Tier 3: internal tools with constrained data and access.

### Phase 2: Assess Against Both Catalogs

1. Run OWASP Top 10:2025 pass for web/API/system controls.
2. Run LLM Top 10:2025 pass for prompt/model/RAG/agent controls.
3. Cross-map overlaps (for example, A01 with LLM06; A05 with LLM01/LLM05).
4. Capture evidence for each potential finding.

### Phase 3: Report Findings

For every finding, include:

- `id`: stable identifier (for example `F-003`).
- `risk`: severity (`Critical|High|Medium|Low`) and confidence (`High|Medium|Low`).
- `owasp_mapping`: one or more OWASP category IDs.
- `evidence`: concrete proof (code path, config, logs, reproduction steps).
- `impact`: business and technical blast radius.
- `exploitability`: preconditions and attacker effort.

### Phase 4: Produce Concrete Remediation

For every finding, provide:

- Immediate containment steps (24-48 hours).
- Durable code/config fixes (next sprint).
- Preventive guardrails in CI/CD and runtime.
- Ownership and execution order.

Use precise instructions, not generic statements like "follow best practices."

### Phase 5: Verify And Roadmap

1. Define objective pass/fail verification tests per finding.
2. Define telemetry and alerting required for sustained detection.
3. Build phased roadmap:
- Quick wins (0-30 days)
- Medium-term hardening (31-90 days)
- Long-term assurance (90+ days)

## Mandatory Output Contract

Use this exact section order in responses:

1. `Executive Summary`
2. `System Context and Assumptions`
3. `Findings (Prioritized)`
4. `Control Mapping (OWASP 2025 + LLM 2025)`
5. `Concrete Remediation Plan`
6. `Verification Checklist`
7. `Security Improvement Roadmap`
8. `Open Risks and Residual Risk Statement`

## Quality Bar

- Base every finding on evidence or clearly labeled inference.
- Prefer root-cause fixes over symptom patches.
- Include at least one preventive control for recurrence.
- Call out tradeoffs where security impacts performance or UX.
- Provide measurable acceptance criteria for each remediation item.

## Reference Loading Guide

Load only relevant files to control context size:

- Traditional OWASP controls: `references/owasp-top-10-2025-review-guide.md`
- AI/LLM controls: `references/owasp-llm-top-10-2025-review-guide.md`
- End-to-end system fixes: `references/remediation-playbooks.md`
- Security program sequencing: `references/security-improvement-roadmaps.md`
- TS/Node/React implementation details: `references/ts-node-react-secure-implementations.md`
