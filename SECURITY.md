# Security Policy

## About This Repository

This repository contains **documentation only** — a security risk assessment
for **GenomaCorp**, a completely fictional genomic sequencing startup created
for educational and portfolio purposes.

This repository does NOT contain:
- Real patient data, genomic sequences, or any PHI/PII
- Real AWS credentials, infrastructure, or account identifiers
- Production systems, live services, or connections to real environments
- Any real organizational data

All content — company name, architecture, patient counts, data volumes — is
entirely invented. Any resemblance to real organizations is coincidental.

---

## Responsible Use

The risk assessment methodology, STRIDE models, compliance gap analysis,
and remediation roadmap documented here are based on publicly available
standards (NIST SP 800-30, GDPR, HIPAA, ISO 27001:2022).

This documentation is intended to demonstrate security assessment methodology
in a healthcare/genomics context. It should not be applied to real infrastructure
without adaptation, professional review, and proper organizational context.

---

## Reporting a Security Issue in This Repository

If you identify a problem in this repository's content, including:

- Documentation that could lead to insecure implementations if misapplied
- Incorrect risk scoring or methodology that contradicts cited standards
- Sensitive data or credentials accidentally committed to the repository
- Content that could cause harm in a real-world security context

**Please report it privately — do not open a public issue.**

1. Go to the **Security** tab of this repository
2. Click **"Report a vulnerability"**
3. Describe what you found, the location in the repository, and the potential impact

---

## Scope and Out-of-Scope Items

**In scope for this security policy:**
- The integrity and accuracy of the documentation
- Accidental exposure of real credentials or sensitive data in commits
- Misleading or harmful guidance in the assessment documents

**Out of scope:**
- Vulnerabilities in AWS services described in this assessment
  → Report to AWS: [aws.amazon.com/security/vulnerability-reporting](https://aws.amazon.com/security/vulnerability-reporting)
- Intentional vulnerabilities described in the risk register (R-001 through R-025)
  → These are the subject of the assessment, not bugs to fix in this repo
- Disagreements with risk scoring methodology
  → Open a public Discussion or Issue for technical debate

---

## Standards Referenced in This Repository

- NIST SP 800-30 Rev.1 (nist.gov)
- ISO/IEC 27001:2022 (iso.org)
- GDPR (gdpr-info.eu)
- HIPAA Security Rule, 45 CFR Part 164 (hhs.gov)
- AWS Well-Architected Framework — Security Pillar (docs.aws.amazon.com)
- Microsoft STRIDE Threat Modeling (microsoft.com)

---

*GenomaCorp is a fictional organization created for security education purposes.*  
*This repository is part of a cybersecurity portfolio focused on bioinformatics security.*