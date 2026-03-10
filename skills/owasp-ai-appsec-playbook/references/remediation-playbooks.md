# Remediation Playbooks

Use these playbooks to turn findings into executable delivery plans.

## Playbook A: Web/API Systems (No LLM Runtime)

### Primary Risks

- A01 Broken Access Control
- A05 Injection
- A07 Authentication Failures
- A09 Logging and Alerting Failures

### 0-48 Hour Containment

1. Patch critical authorization bypasses and injection vectors.
2. Rotate exposed secrets and invalidate high-risk sessions.
3. Enable temporary defensive controls (WAF rules, strict rate limits).
4. Increase audit logging for authn/authz and admin paths.

### Sprint Hardening (1-2 sprints)

1. Introduce centralized authorization guard at service layer.
2. Enforce schema validation on all external inputs.
3. Replace dynamic query/command execution with safe abstractions.
4. Implement session lifecycle controls (rotation/revocation/timeout).

### Programmatic Prevention

1. Add SAST/DAST and dependency policy gates to CI.
2. Add security acceptance tests to definition-of-done.
3. Track remediation SLA by severity.

## Playbook B: RAG Assistants

### Primary Risks

- LLM01 Prompt Injection
- LLM02 Sensitive Information Disclosure
- LLM05 Improper Output Handling
- LLM08 Vector and Embedding Weaknesses

### 0-48 Hour Containment

1. Disable high-risk tools and write actions by default.
2. Add output filtering for secrets and dangerous instructions.
3. Restrict retrieval scope to trusted corpora only.
4. Add temporary manual approval for privileged tool actions.

### Sprint Hardening (1-2 sprints)

1. Implement prompt isolation (system/developer/user/context separation).
2. Add schema enforcement for all model outputs consumed downstream.
3. Enforce vector store tenant isolation and metadata-based access controls.
4. Add ingestion pipeline checks for source trust and poisoning indicators.

### Programmatic Prevention

1. Run injection regression suite in CI before deployment.
2. Add continuous drift and hallucination monitoring.
3. Implement budget controls for token/tool usage.

## Playbook C: Agentic Systems (Tools + Autonomy)

### Primary Risks

- LLM06 Excessive Agency
- LLM07 System Prompt Leakage
- LLM10 Unbounded Consumption
- A10 Mishandling of Exceptional Conditions

### 0-48 Hour Containment

1. Reduce tool scopes to minimum viable permissions.
2. Enforce hard kill-switch for automated write actions.
3. Cap recursion depth, tool-call count, and runtime budget.
4. Remove secrets from prompt templates and agent memory.

### Sprint Hardening (1-2 sprints)

1. Implement policy decision point (PDP) before any external action.
2. Add human-approval gate for destructive or high-value actions.
3. Apply least-privilege identities per tool and per environment.
4. Build robust exception handling with fail-closed defaults.

### Programmatic Prevention

1. Add simulation tests for prompt-injection-triggered tool misuse.
2. Add control-plane telemetry for every agent action.
3. Enforce change review for tool onboarding and permission changes.

## Cross-Mapping Matrix For Prioritization

| Scenario | Traditional OWASP Link | LLM OWASP Link | Why This Matters |
|---|---|---|---|
| Unauthorized tool action | A01 | LLM06 | Same root cause: over-broad permissions |
| Model output causes exploit | A05 | LLM05 | Same failure: untrusted data reaches execution sink |
| Hidden failure path bypasses controls | A10 | LLM10 | Resource/exception paths become attack paths |
| Leaked internal instructions and secrets | A04/A09 | LLM07/LLM02 | Confidentiality and detection gaps combine |

## Design Pattern Guidance For Durable Fixes

- **Service Layer + Policy Guard (Facade/Strategy):** centralize security decisions.
- **Repository + Query Object:** prevent ad hoc query injection patterns.
- **Command Pattern for Tool Actions:** make agent actions explicit, auditable, and rejectable.
- **Decorator Pattern for Security Middleware:** add validation, authz, and telemetry uniformly.
- **Template Method for Secure Pipelines:** enforce mandatory security stages in ingestion/execution flow.

## Definition Of Done For Remediation

A finding is closed only when all are true:

1. Exploit path is blocked by implemented controls.
2. Automated verification test exists and passes.
3. Runtime telemetry confirms control effectiveness.
4. Residual risk is documented and accepted by owner.
