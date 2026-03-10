# OWASP Top 10:2025 Review Guide

Use this guide during the traditional web/API security pass.

## A01:2025 Broken Access Control

### Detection Cues

- Missing authorization checks on object- or action-level endpoints.
- IDOR patterns (`/users/:id`, `/invoice/:id`) without ownership validation.
- Privilege escalation through role tampering or unguarded admin routes.
- Cross-tenant data exposure in multi-tenant systems.

### Typical Exploit Path

1. Authenticate as low-privilege user.
2. Enumerate predictable resource IDs.
3. Access or mutate another user's resource.

### Concrete Mitigations

- Enforce deny-by-default authorization at service layer.
- Apply object-level policy checks on every read/write.
- Isolate tenant data in query predicates and caching keys.
- Use centralized policy guards (RBAC/ABAC), not controller-local checks.

### Verification

- Automated authorization matrix tests for each endpoint + role.
- Negative tests proving forbidden access returns 403/404.
- Multi-tenant tests to ensure zero cross-tenant access.

## A02:2025 Security Misconfiguration

### Detection Cues

- Default credentials, debug routes, or verbose errors in production.
- Overly broad CORS, open admin panels, excessive headers leakage.
- Insecure cloud/storage/network defaults.

### Typical Exploit Path

1. Fingerprint environment from exposed headers or errors.
2. Abuse misconfigured endpoint or cloud policy.
3. Pivot to sensitive services.

### Concrete Mitigations

- Harden environment baselines (prod/staging/dev).
- Enforce config as code with policy checks.
- Disable debug capabilities in production builds.
- Restrict CORS, methods, and headers to explicit allowlists.

### Verification

- Runtime config audit script per environment.
- IaC policy gate in CI.
- Canary checks for forbidden debug endpoints.

## A03:2025 Software Supply Chain Failures

### Detection Cues

- Unpinned dependencies and unsigned artifacts.
- No SBOM, provenance, or vulnerability policy.
- Build pipelines pulling code from untrusted sources.

### Typical Exploit Path

1. Introduce malicious dependency/transitive package.
2. Inject payload into build artifact.
3. Execute in production under trusted runtime identity.

### Concrete Mitigations

- Pin and verify dependency versions.
- Require provenance attestations for builds.
- Generate and archive SBOM per release.
- Implement dependency allowlist and vulnerability SLA policy.

### Verification

- CI blocks release for critical CVEs above policy threshold.
- Provenance verification passes for all deployable artifacts.
- Reproducible build checks on release candidates.

## A04:2025 Cryptographic Failures

### Detection Cues

- Weak algorithms, custom crypto, or missing key rotation.
- Secrets or PII in plaintext storage/logs.
- Tokens without integrity/signature enforcement.

### Typical Exploit Path

1. Acquire encrypted data or tokens.
2. Exploit weak algorithm/key handling.
3. Recover plaintext or forge trusted payloads.

### Concrete Mitigations

- Use vetted libraries and modern algorithms.
- Encrypt sensitive data at rest and in transit.
- Store keys in managed KMS/HSM, rotate by policy.
- Use signed tokens with strict validation and expiry.

### Verification

- Static checks for banned algorithms.
- Key age and rotation compliance dashboard.
- Cryptographic integration tests and failure-mode tests.

## A05:2025 Injection

### Detection Cues

- Dynamic query construction from unsanitized inputs.
- Unsafe shell execution and template injection patterns.
- NoSQL operators accepted from user input.

### Typical Exploit Path

1. Submit crafted payload.
2. Break parsing boundary (SQL/NoSQL/command/template).
3. Execute unauthorized query or command.

### Concrete Mitigations

- Use parameterized queries and typed query builders.
- Validate inputs with strict schemas.
- Avoid shell interpolation; use argument arrays.
- Contextually encode output for HTML/JS/URL sinks.

### Verification

- Negative tests with known payload corpus.
- SAST rules for dynamic execution patterns.
- DAST payload replay against staging.

## A06:2025 Insecure Design

### Detection Cues

- No abuse-case modeling for critical flows.
- Missing safety limits (rate limits, quotas, fail-safe defaults).
- High-risk features shipped without threat model.

### Typical Exploit Path

1. Find business logic gap not covered by controls.
2. Abuse intended workflow at scale.
3. Extract value or disrupt service.

### Concrete Mitigations

- Add threat modeling and abuse-case reviews in design phase.
- Define security acceptance criteria before implementation.
- Add fail-safe defaults and business logic guardrails.

### Verification

- Architecture review checklist completion before merge.
- Threat model artifacts linked to releases.
- Abuse-case tests in pre-release validation.

## A07:2025 Authentication Failures

### Detection Cues

- Weak password policies, no MFA for privileged actions.
- Session fixation, long-lived or non-rotating tokens.
- Account takeover paths with weak recovery workflows.

### Typical Exploit Path

1. Brute force/credential-stuff account.
2. Reuse weak session/token controls.
3. Access protected features.

### Concrete Mitigations

- Require MFA for admin and sensitive operations.
- Implement robust session lifecycle (rotation, revocation, timeout).
- Add adaptive login protections and anomaly detection.

### Verification

- Authentication flow tests for brute-force lockout and MFA.
- Session invalidation tests after password change and logout.
- Alerting for impossible travel and suspicious login patterns.

## A08:2025 Software or Data Integrity Failures

### Detection Cues

- Untrusted updates or deserialization of unsigned data.
- CI/CD jobs with mutable artifacts and no integrity checks.
- Insecure webhook/event trust assumptions.

### Typical Exploit Path

1. Tamper with data or artifact in transit.
2. Bypass integrity checks due to missing signatures.
3. Execute compromised artifact or trust poisoned data.

### Concrete Mitigations

- Verify signatures/checksums on updates and artifacts.
- Require immutable build outputs and controlled promotion.
- Validate webhook signatures and replay windows.

### Verification

- Pipeline tests that reject tampered artifacts.
- Replay-attack tests on webhook endpoints.
- Integrity alarms when checksum/signature mismatches occur.

## A09:2025 Security Logging and Alerting Failures

### Detection Cues

- Missing audit events for authz/authn/admin actions.
- Logs without correlation IDs or insufficient retention.
- Alerts not tied to actionable runbooks.

### Typical Exploit Path

1. Perform low-noise malicious actions.
2. Avoid detection due to sparse telemetry.
3. Persist and expand compromise.

### Concrete Mitigations

- Define minimum required security events.
- Add structured logs with trace/user/session IDs.
- Route alerts to owned on-call runbooks with SLOs.

### Verification

- Detection engineering tests for key attack scenarios.
- Tabletop exercises that validate MTTR and alert quality.
- Coverage report: required event set vs emitted events.

## A10:2025 Mishandling of Exceptional Conditions

### Detection Cues

- Fail-open logic on timeout/error paths.
- Unhandled exceptions leaking internals.
- Retry storms or cascading failures under load.

### Typical Exploit Path

1. Trigger exception boundary or dependency timeout.
2. Force degraded mode with weakened controls.
3. Bypass protections or cause denial of service.

### Concrete Mitigations

- Use fail-closed defaults for authorization and sensitive actions.
- Standardize exception handling and sanitize error responses.
- Add circuit breakers, backoff, and bulkhead isolation.

### Verification

- Chaos tests for dependency and timeout failures.
- Unit/integration tests for fail-closed behavior.
- SLO monitoring for error budgets and fallback paths.

## Cross-Cutting Review Heuristics

- Check trust boundaries before endpoint-level details.
- Prefer evidence of exploitability over theoretical claims.
- Tie each remediation to a measurable acceptance criterion.
- Track residual risk when full remediation is deferred.
