# OWASP AI AppSec Playbook Skill

A cross-platform security review skill for AI coding agents.

This repository provides a single canonical skill package, `owasp-ai-appsec-playbook`, designed to work across:

- OpenAI/Codex skill ecosystems (including `skills.sh`-style metadata consumption)
- Claude Code skill workflows

The skill performs structured security assessments and produces concrete remediation guidance aligned with:

- OWASP Top 10:2025 (traditional web/API application security)
- OWASP LLM Top 10:2025 (AI/LLM application security)

## Why this exists

Most security audits stop at "what is wrong." This skill is optimized for "how to fix it" and "how to prove the fix works."

Every review flow is designed to produce:

1. Prioritized findings with severity, confidence, and evidence
2. Explicit OWASP control mapping
3. Concrete remediation steps (code/config/process)
4. Verification checks and tests
5. A phased improvement roadmap (quick wins to long-term assurance)

## Repository contents

```text
owasp-ai-appsec-playbook/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── owasp-top-10-2025-review-guide.md
    ├── owasp-llm-top-10-2025-review-guide.md
    ├── remediation-playbooks.md
    ├── security-improvement-roadmaps.md
    └── ts-node-react-secure-implementations.md
```

## What the skill covers

### Traditional OWASP Top 10:2025

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

## Skill design principles

- Single source of truth: one canonical `SKILL.md` and shared references
- Cross-platform compatibility: OpenAI metadata in `agents/openai.yaml` without coupling core behavior to platform-specific files
- Evidence-first reviews: findings should be backed by concrete evidence or clearly marked inference
- Actionability: remediation guidance must be specific and measurable
- Progressive disclosure: concise trigger instructions in `SKILL.md`, deep detail in `references/`

## Output contract (mandatory)

The skill enforces this response structure:

1. Executive Summary
2. System Context and Assumptions
3. Findings (Prioritized)
4. Control Mapping (OWASP 2025 + LLM 2025)
5. Concrete Remediation Plan
6. Verification Checklist
7. Security Improvement Roadmap
8. Open Risks and Residual Risk Statement

## Quick start

### Option A: Use as a local skill folder

1. Clone this repository.
2. Copy `owasp-ai-appsec-playbook/` into your local skills directory for your agent runtime.
3. Invoke with `$owasp-ai-appsec-playbook` in your prompt.

### Option B: Use packaged zip (Claude-style distribution)

Create a zip package from the repository root:

```bash
zip -r owasp-ai-appsec-playbook.zip owasp-ai-appsec-playbook
```

Then import/install the zip using your Claude Code skill flow.

### Option C: URL-based usage (OpenAI/Codex ecosystems)

Publish this repository publicly and reference the skill folder URL in your skill installation flow where supported.

## Example prompts

### Traditional app review

```text
Use $owasp-ai-appsec-playbook to review this Node/Express API for OWASP Top 10:2025 risks and provide concrete fixes plus verification tests.
```

### LLM app review

```text
Use $owasp-ai-appsec-playbook to audit this RAG chatbot against OWASP LLM Top 10:2025 and provide prioritized remediation steps with measurable acceptance criteria.
```

### Hybrid system review

```text
Use $owasp-ai-appsec-playbook to assess our React frontend + NestJS API + agentic assistant stack, map findings to both OWASP 2025 lists, and produce a 30/60/90-day security improvement roadmap.
```

## Reference guide map

- `references/owasp-top-10-2025-review-guide.md`
  - Traditional OWASP category-by-category review cues, exploit paths, mitigations, and verification checks.
- `references/owasp-llm-top-10-2025-review-guide.md`
  - LLM risk category guidance, red-team scenarios, and AI-specific controls.
- `references/remediation-playbooks.md`
  - Prescriptive remediation flows for web/API, RAG, and agentic systems.
- `references/security-improvement-roadmaps.md`
  - Security maturity model, 30/60/90+ sequencing, and KPI framework.
- `references/ts-node-react-secure-implementations.md`
  - Concrete TypeScript/Node/React implementation patterns and anti-patterns.

## Contribution guidelines

Contributions should preserve the core contract of this skill:

- Keep `SKILL.md` concise and workflow-oriented.
- Put deep details in `references/` rather than bloating trigger content.
- Maintain equal weighting across traditional and LLM OWASP categories.
- Prefer concrete remediation and verification over abstract recommendations.
- Do not introduce platform-specific behavior differences between OpenAI/Codex and Claude flows.

Recommended update workflow:

1. Update content in `owasp-ai-appsec-playbook/`.
2. Validate frontmatter and metadata consistency.
3. Test with at least one traditional, one AI-only, and one hybrid prompt.
4. Confirm output contract sections are all present.
5. Submit PR with before/after examples.

## Versioning and maintenance

When OWASP revisions occur:

1. Update category names/IDs in `SKILL.md` and corresponding reference guides.
2. Keep mappings and playbooks synchronized.
3. Record migration notes in the pull request description.
4. Re-test example prompts for taxonomy and output consistency.

## Security notice

This skill improves review quality and remediation clarity, but it is not a replacement for:

- Hands-on penetration testing
- Production security monitoring
- Legal/compliance review
- Formal threat modeling for high-risk systems

Treat generated guidance as expert support, then validate in your actual environment.
