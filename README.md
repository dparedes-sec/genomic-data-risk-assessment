# Genomic Data Security Risk Assessment

![Secret Scanning](https://github.com/dparedes-sec/genomic-data-risk-assessment/actions/workflows/security.yml/badge.svg)

> Full information security risk assessment for a fictional genomic sequencing startup
> on AWS — NIST SP 800-30 Rev.1 methodology, STRIDE threat modeling,
> GDPR/HIPAA/ISO 27001 compliance gap analysis.

---

## Assessment Scope

| Attribute | Value |
| --- | --- |
| Organization | GenomaCorp (fictional genomic sequencing startup) |
| Infrastructure | AWS (S3, RDS, EC2, API Gateway, Lambda, VPC) |
| Regulatory frameworks | GDPR (EU), HIPAA (US), ISO 27001:2022 |
| Methodology | NIST SP 800-30 Rev.1 + STRIDE threat modeling |
| Assessment date | 2026-03-14 |
| Key assets analyzed | 6 |

---

## Risk Summary

| Risk Level | Count | Key Findings |
| --- | --- | --- |
| Very High | 3 | Public S3 bucket with genomic data, root account without MFA, public RDS instance |
| High | 6 | Unauthenticated upload API, over-privileged Lambda roles, no CloudTrail, missing DPAs |
| Moderate | 12 | No encryption at rest (RDS), no MFA for IAM users, no VPC flow logs, no DR testing, SQL injection exposure, no API rate limiting, root pipeline execution |
| Low | 4 | Weak password policy, missing S3 access logs, unencrypted CloudTrail logs, unencrypted EC2 EBS volumes |
| Total | 25 | |

---

## Compliance Gaps Summary

| Framework | Key Gaps |
| --- | --- |
| GDPR | Art.9 (special category data), Art.25 (by design), Art.28 (no DPAs), Art.32 (security measures) |
| HIPAA | 164.308(a)(1) no risk analysis, 164.312(b) no audit controls, 164.312(e)(1) no transmission security |
| ISO 27001 | A.5.15 (access control), A.8.2 (privileged access), A.8.15 (logging), A.8.20 (network security), A.8.26 (application security), A.8.28 (secure coding) |

---

## Methodology

This assessment applies the four-step NIST SP 800-30 Rev.1 process:

1. Prepare   — Define scope, risk model, and assessment approach (qualitative, vulnerability-oriented)
2. Conduct   — Identify threat sources (Table D-2), threat events (Table E-2), vulnerabilities (Table F-2),
              likelihood (Table G-5), impact (Table H-3), and risk (Table I-2)
3. Communicate — Executive summary + risk register with prioritization
4. Maintain  — 90-day remediation roadmap with measurable verification criteria

Threat modeling uses STRIDE-per-element (Microsoft SDL) applied to each of the 6 key assets,
producing 36 threat scenarios.

---

## Document Index

| Document | Content |
| --- | --- |
| [Executive Summary](docs/00-executive-summary.md) | Top findings, risk distribution, immediate actions |
| [Scope and Methodology](docs/01-scope-and-methodology.md) | NIST SP 800-30 framework, risk matrix, assumptions |
| [Asset Inventory](docs/02-asset-inventory.md) | 6 key assets with data classification |
| [STRIDE Threat Model](docs/03-threat-model-stride.md) | 36 threat scenarios across 6 assets |
| [Risk Register](docs/04-risk-register.md) | 25 risks with likelihood/impact/rationale |
| [Compliance Gap Analysis](docs/05-compliance-mapping.md) | GDPR, HIPAA, ISO 27001 control mapping |
| [Remediation Roadmap](docs/06-remediation-roadmap.md) | 90-day prioritized action plan |
| [Architecture Diagram](diagrams/architecture.md) | Mermaid diagram of the AWS environment |
| [Risk Matrix](diagrams/risk-matrix.md) | 5x5 Likelihood x Impact matrix |
