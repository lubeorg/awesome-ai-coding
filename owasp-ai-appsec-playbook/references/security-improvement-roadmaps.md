# Security Improvement Roadmaps

Use this guide to sequence improvements after a review.

## Maturity Model

### Level 1: Reactive Baseline

- Fixes happen after incidents or reports.
- Limited security testing and weak ownership.
- Goal: eliminate known critical/high exploit paths quickly.

### Level 2: Managed Controls

- Standard controls implemented for authn/authz, validation, and logging.
- Security checks in CI for dependencies and static analysis.
- Goal: reduce repeat classes of vulnerabilities.

### Level 3: Proactive Assurance

- Threat modeling and abuse-case testing embedded in design.
- AI-specific controls (prompt safety, output validation, tool governance) are standardized.
- Goal: detect and block vulnerabilities before release.

### Level 4: Continuous Resilience

- Security posture measured continuously with SLOs and risk trends.
- Automated policy enforcement across SDLC and runtime.
- Goal: sustain low residual risk with rapid recovery.

## 30/60/90+ Day Roadmap Template

## 0-30 Days (Quick Wins)

1. Close all critical exploit paths with containment and durable patches.
2. Add mandatory input validation and output handling guardrails.
3. Enable minimum security telemetry and actionable alerts.
4. Enforce token/tool/request limits for AI-enabled systems.

## 31-90 Days (Systemic Hardening)

1. Refactor authorization into centralized policy enforcement.
2. Introduce CI policy gates (SAST/DAST/dependency/secrets/provenance).
3. Implement AI data governance for ingestion, embeddings, and prompts.
4. Add red-team regression suites for prompt injection and agent misuse.

## 90+ Days (Assurance At Scale)

1. Formalize security architecture review as release gate.
2. Implement control effectiveness dashboards and error budget tracking.
3. Expand chaos/security game days for exceptional-condition resilience.
4. Establish executive risk reporting tied to measurable KPIs.

## Prioritization Model

Use a simple scoring model to prioritize remediation:

`Priority Score = Impact x Exploitability x Exposure x Detectability Gap`

### Scoring Guidance

- **Impact:** data sensitivity, business interruption, legal/regulatory consequences.
- **Exploitability:** required skill, required access, exploit complexity.
- **Exposure:** internet-facing surface, tenant blast radius, privilege scope.
- **Detectability Gap:** ability to detect and respond before major harm.

## KPI Set For Improvement Tracking

Track at least these metrics:

- Mean time to remediate (MTTR) by severity.
- Open critical/high findings by OWASP category.
- Injection/prompt-injection regression pass rate.
- Unauthorized access attempt detection rate.
- AI safety incidents per 1,000 interactions.
- Cost abuse events (token/tool overconsumption) per week.

## Acceptance Criteria Library

### Access Control

- 100% of protected endpoints covered by authorization tests.
- Cross-tenant negative tests pass in CI.

### Input/Output Safety

- Schema validation present on all untrusted inputs and model outputs.
- Blocking tests for known malicious payload classes pass.

### AI Tooling Governance

- Every tool action mapped to explicit permission scope.
- High-risk actions require approval gate and are fully audited.

### Observability

- Required security events emitted with correlation IDs.
- Alert-to-runbook linkage exists for all critical detections.

## Residual Risk Statement Template

Use this structure when risk cannot be fully removed:

- `Risk`: concise summary.
- `Reason deferred`: technical or business reason.
- `Compensating controls`: currently active safeguards.
- `Expiry`: date for reassessment.
- `Owner`: accountable team/person.
