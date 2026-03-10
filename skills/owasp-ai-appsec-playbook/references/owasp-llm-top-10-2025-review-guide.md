# OWASP LLM Top 10:2025 Review Guide

Use this guide during the AI/LLM security pass.

## LLM01:2025 Prompt Injection

### Detection Cues

- User content merged directly into privileged system/tool prompts.
- No separation between instructions, data, and tool parameters.
- RAG ingestion without prompt-safety filtering.

### Typical Exploit Path

1. Insert malicious directives into user prompt or retrieved document.
2. Override intended model behavior.
3. Trigger unauthorized action or data exfiltration.

### Concrete Mitigations

- Enforce strict prompt layering and role separation.
- Treat model outputs and retrieved content as untrusted.
- Add policy guardrails before tool execution.
- Use allowlisted tool schemas and argument validation.

### Verification

- Red-team prompt suite (direct and indirect injection).
- Regression tests for refusal and safe fallback behavior.
- Tool-call policy tests for blocked unsafe actions.

## LLM02:2025 Sensitive Information Disclosure

### Detection Cues

- Model/system prompts contain secrets or sensitive internals.
- Chat transcripts/logs store PII without retention policy.
- Responses may reveal credentials, tokens, or proprietary data.

### Typical Exploit Path

1. Probe model with extraction prompts.
2. Retrieve leaked context from prompts, memory, or logs.
3. Use leaked data for lateral movement.

### Concrete Mitigations

- Remove secrets from prompts and model-visible context.
- Apply data minimization and redaction before model input/output.
- Add response DLP checks and policy-based masking.

### Verification

- Prompt-based extraction tests.
- DLP scanner checks on prompts, logs, and outputs.
- Secret scanning on code/config and runtime traces.

## LLM03:2025 Supply Chain

### Detection Cues

- Unvetted model providers, plugins, embeddings, or datasets.
- Missing provenance for model artifacts and prompts.
- No risk register for third-party AI dependencies.

### Typical Exploit Path

1. Introduce compromised model/plugin/dataset.
2. Propagate malicious behavior into production pipeline.
3. Exfiltrate data or degrade trustworthiness.

### Concrete Mitigations

- Maintain approved model/provider inventory.
- Require provenance and integrity validation for models and datasets.
- Pin versions and use staged rollout with rollback plans.

### Verification

- Vendor and artifact provenance checks in CI/CD.
- SBOM/AIBOM generation and policy gating.
- Controlled canary for model and dependency updates.

## LLM04:2025 Data and Model Poisoning

### Detection Cues

- Untrusted training/fine-tuning/retrieval sources.
- No data lineage or quality controls.
- Sudden behavior drift after data refresh.

### Typical Exploit Path

1. Poison source data or embeddings.
2. Influence model responses and decision paths.
3. Cause insecure recommendations or harmful output.

### Concrete Mitigations

- Add ingestion validation, provenance, and moderation checks.
- Segment trusted and untrusted corpora with policy labels.
- Monitor model behavior drift and rollback on anomalies.

### Verification

- Data quality/security gates before indexing/training.
- Drift tests on benchmark prompts.
- Poisoned-corpus simulation tests.

## LLM05:2025 Improper Output Handling

### Detection Cues

- LLM output passed directly into interpreters, SQL, HTML, or tools.
- Missing sanitization and context-aware encoding.
- Autonomous execution based on model text.

### Typical Exploit Path

1. Generate malicious model output.
2. Downstream system executes/accepts output without validation.
3. Trigger XSS, SSRF, command execution, or data corruption.

### Concrete Mitigations

- Validate model output against strict schemas.
- Encode and sanitize by sink context.
- Require human approval or policy gate for high-risk actions.

### Verification

- Fuzz tests for malicious output patterns.
- Contract tests that reject non-conforming outputs.
- Audit logs proving approval gate for critical actions.

## LLM06:2025 Excessive Agency

### Detection Cues

- Agents can perform irreversible operations without confirmations.
- Tool permissions are broad and persistent.
- No scoped identity per tool/action.

### Typical Exploit Path

1. Manipulate model intent via prompt/context.
2. Use over-privileged agent to execute impactful actions.
3. Cause destructive or unauthorized state changes.

### Concrete Mitigations

- Apply least-privilege scopes to tools and credentials.
- Add confirmation/approval checkpoints for sensitive actions.
- Use action budgets and execution boundaries.

### Verification

- Permission boundary tests per tool.
- Simulation of unsafe action requests with expected denial.
- Audit trail checks for all write actions.

## LLM07:2025 System Prompt Leakage

### Detection Cues

- System prompts include secrets, credentials, policy internals.
- Prompt retrieval endpoints or logs expose hidden instructions.
- Authorization logic encoded in prompts rather than backend controls.

### Typical Exploit Path

1. Elicit system prompt via crafted extraction queries.
2. Learn hidden rules/secrets.
3. Use leaked information to bypass controls.

### Concrete Mitigations

- Treat system prompts as non-secret.
- Move authorization and secret handling to trusted backend services.
- Store only behavior instructions in prompt templates.

### Verification

- Prompt-leak extraction red-team tests.
- Static checks for secrets in prompt repositories.
- Authz penetration tests independent of prompt text.

## LLM08:2025 Vector and Embedding Weaknesses

### Detection Cues

- Multi-tenant vector stores without strict tenant isolation.
- Embedding inversion or semantic leakage exposure.
- Weak metadata filters on retrieval queries.

### Typical Exploit Path

1. Query vector store with crafted embeddings/filters.
2. Retrieve cross-tenant or sensitive embeddings/documents.
3. Extract information via reconstruction or context leakage.

### Concrete Mitigations

- Enforce tenant-aware namespaces and metadata filters.
- Encrypt sensitive embeddings and restrict retrieval scopes.
- Apply query anomaly monitoring and throttling.

### Verification

- Cross-tenant retrieval denial tests.
- Embedding leakage and inversion simulation.
- Query pattern anomaly alerts in production telemetry.

## LLM09:2025 Misinformation

### Detection Cues

- High-risk outputs generated without grounding/citations.
- No confidence signaling or uncertainty handling.
- Automated decisions based on unchecked model claims.

### Typical Exploit Path

1. Seed misleading context or exploit model uncertainty.
2. Model emits plausible but false recommendations.
3. Operators/users act on inaccurate outputs.

### Concrete Mitigations

- Require grounding to trusted sources for critical domains.
- Add confidence thresholds and fallback to human review.
- Add contradiction checks and independent validation steps.

### Verification

- Benchmark suite with known-truth prompts.
- Hallucination rate thresholds tracked by domain.
- Human-in-the-loop coverage report for high-impact decisions.

## LLM10:2025 Unbounded Consumption

### Detection Cues

- No rate/token/tool-call limits.
- Unlimited context windows and chain depth.
- Missing cost and abuse guardrails.

### Typical Exploit Path

1. Trigger expensive prompt loops/tool loops.
2. Exhaust token budgets and service quotas.
3. Cause denial of wallet/service degradation.

### Concrete Mitigations

- Enforce per-user and per-workload quotas.
- Set max tokens, tool-call depth, and execution timeouts.
- Add anomaly detection for usage spikes and loop patterns.

### Verification

- Load tests for token and tool abuse scenarios.
- Budget alarm and auto-throttle drills.
- Unit tests for recursion/loop guardrails.

## Cross-Cutting LLM Review Heuristics

- Treat model, retrieved data, and tool inputs/outputs as untrusted.
- Place policy enforcement outside the model in deterministic services.
- Require explicit ownership for model updates, prompts, and data pipelines.
