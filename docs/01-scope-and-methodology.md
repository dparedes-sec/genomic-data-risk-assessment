# Scope and Methodology

## 1.1 Assessment Purpose

This risk assessment identifies and prioritizes information security risks for
GenomaCorp's AWS-based genomic data platform. Results are intended to inform
risk response decisions and support ISO 27001 certification planning.

This is an initial risk assessment (SP 800-30 Task 1-1) conducted at Tier 3
(information system level) with Tier 2 mission/business process considerations.

## 1.2 Scope

In scope: Six key assets defined in the asset inventory (doc 02).

Time frame: Findings are valid for 12 months or until significant
architectural changes occur (SP 800-30 Section 3.1, Task 1-2).

Regulatory frameworks in scope: GDPR, HIPAA Security Rule, ISO 27001:2022.

## 1.3 Methodology

Risk assessment follows NIST SP 800-30 Rev.1 four-step process:

1. Prepare  -> Define scope, assumptions, risk model
2. Conduct  -> Identify threats, vulnerabilities, likelihood, impact, risk
3. Communicate -> Executive summary and risk register
4. Maintain -> Roadmap and monitoring recommendations

Risk model: Risk = f(Likelihood, Impact) per Table I-2 of SP 800-30.
Scales: Five-level qualitative scale — Very High, High, Moderate, Low, Very Low.
Threat modeling: STRIDE per-element (Microsoft SDL) applied to each key asset.
Analysis approach: Vulnerability-oriented (SP 800-30 Section 2.3.3).

## 1.4 Risk Determination Matrix (NIST SP 800-30 Rev.1, Table I-2)

| Likelihood \ Impact | Very Low | Low | Moderate | High | Very High |
| --- | --- | --- | --- | --- | --- |
| Very High | VL | L | M | H | VH |
| High | VL | L | M | H | VH |
| Moderate | VL | L | M | M | H |
| Low | VL | VL | L | L | M |
| Very Low | VL | VL | VL | VL | L |

## 1.5 Assumptions and Constraints

- GenomaCorp has no existing security controls documented (worst-case baseline).
- Assessment is based on architecture review, not active vulnerability scanning.
- Likelihood values assume an adversary with moderate capability (Table D-3).
- Impact values reflect potential for genomic PHI/PII breach at scale.

## 1.6 Primary Sources

- NIST SP 800-30 Rev.1: Guide for Conducting Risk Assessments
- AWS Well-Architected Framework: Security Pillar
- GDPR Articles 9, 25, 28, 32 (gdpr-info.eu)
- HIPAA Security Rule 45 CFR Part 164 (hhs.gov)
- ISO/IEC 27001:2022 Annex A
