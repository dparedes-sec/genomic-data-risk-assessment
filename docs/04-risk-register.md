# Risk Register — GenomaCorp AWS Security Assessment

Assessment Date: 2026-03-14
Methodology: NIST SP 800-30 Rev.1 — Tables G-5, H-3, I-2
Risk Levels: Very High (VH) | High (H) | Moderate (M) | Low (L) | Very Low (VL)

## Summary Dashboard

| Level | Count | IDs |
| --- | --- | --- |
| Very High | 3 | R-001, R-002, R-003 |
| High | 6 | R-004 through R-009 |
| Moderate | 12 | R-010, R-011, R-012, R-013, R-014, R-015, R-016, R-017, R-021, R-022, R-023, R-025 |
| Low | 4 | R-018, R-019, R-020, R-024 |
| Total | 25 | |

## R-001 — S3 Bucket Publicly Accessible

- Asset: A-001 (S3 genomacorp-sequences-prod)
- Threat Source: External adversarial — opportunistic scanner (Table D-2)
- Threat Event: Obtain sensitive data from publicly accessible information systems (Table E-2)
- Vulnerability: S3 Block Public Access not enabled; bucket ACL allows public read.
  Vulnerability Severity: Very High (Table F-2 — exploitable, no remediation in place)
- Likelihood Rationale: High — automated bucket scanners (GrayhatWarfare) index exposed S3
  buckets within hours of creation. No skill barrier to exploit.
- Impact Rationale: Very High — 2TB of raw genomic sequences. GDPR Art.83 fines up to
  4% global turnover; HIPAA OCR breach penalties; reputational destruction.
- Likelihood: High | Impact: Very High | Risk Level: Very High
  (NIST Table I-2: H x VH = VH)
- Regulatory: GDPR Art.32, Art.83; HIPAA 164.312(a)(2)(iv); ISO 27001 A.8.7, A.8.12
- Immediate Action: Enable S3 Block Public Access at account and bucket level.

---

## R-002 — AWS Root Account Used Without MFA

- Asset: A-004 (IAM Configuration)
- Threat Source: External adversarial — targeted, credential theft via phishing
- Threat Event: Craft phishing attacks (Table E-2); compromise IAM configuration
- Vulnerability: Root account credentials used for daily operations (highest AWS privilege).
  No MFA device registered. Severity: Very High.
- Likelihood Rationale: High — phishing of startup employees is common; root account
  has no secondary factor. One successful phish = full AWS account compromise.
- Impact Rationale: Very High — root access allows deletion of all data, creation of
  backdoor IAM users, disabling all controls. Effectively total loss of AWS environment.
- Likelihood: High | Impact: Very High | Risk Level: Very High
- Regulatory: ISO 27001 A.5.16, A.8.2; HIPAA 164.312(d); AWS CIS Benchmark 1.4
- Immediate Action: Enable MFA on root. Lock away root credentials. Create break-glass procedure.

---

## R-003 — RDS Instance Publicly Accessible

- Asset: A-002 (RDS prod-clinical-db)
- Threat Source: External adversarial — opportunistic and targeted
- Threat Event: Exploit poorly configured systems exposed to the Internet (Table E-2)
- Vulnerability: RDS instance has 'Publicly Accessible: Yes'. Port 5432 open to 0.0.0.0/0.
  Only defense is password authentication.
- Likelihood Rationale: High — Shodan and Censys continuously index public RDS endpoints.
  Brute-force and credential stuffing attacks are fully automated.
- Impact Rationale: Very High — clinical metadata of 12,000 patients across EU and US.
  GDPR Art.83(2) max fine: 20M EUR. HIPAA breach reporting + penalties.
- Likelihood: High | Impact: Very High | Risk Level: Very High
- Regulatory: GDPR Art.25, Art.32; HIPAA 164.312(a)(1); ISO 27001 A.8.20, A.8.21
- Immediate Action: Set 'Publicly Accessible: No'. Move to private subnet. Restrict to VPC CIDR.

## R-004 — API Gateway Upload Endpoint Has No Authentication

- Asset: A-003 (API Gateway + Lambda)
- Vulnerability: /v1/upload accepts any POST without API key, JWT, or mutual TLS.
- Likelihood: High | Impact: High | Risk Level: High (H x H = H)
- Regulatory: GDPR Art.32; HIPAA 164.312(a)(2)(i); ISO 27001 A.8.20

---

## R-005 — Lambda Functions Have Admin-Level IAM Roles (Privilege Escalation Path)

- Asset: A-003, A-005 (Lambda functions)
- Vulnerability: Lambda execution roles have iam:* and s3:* (all buckets). If Lambda is
  compromised via SSRF or code injection, attacker has full AWS account control.
- Likelihood: High | Impact: High | Risk Level: High
- Regulatory: ISO 27001 A.5.15, A.8.2; AWS Well-Architected Security Pillar (SEC04)

---

## R-006 — No CloudTrail Logging for Data Access Events

- Assets: A-001, A-002, A-004, A-006
- Vulnerability: CloudTrail not configured. No audit trail of S3 data access, IAM API
  calls, RDS events, or EC2 command execution. Ongoing intrusion would be undetectable
  by definition.
- Likelihood: High | Impact: High | Risk Level: High
- Regulatory: GDPR Art.33 (72h breach notification); HIPAA 164.312(b); ISO 27001 A.8.15, A.8.16

---

## R-007 — No GDPR Data Processing Agreements with EU Hospital Partners

- Asset: A-005 (Partner Data Sharing Pipeline)
- Vulnerability: Genomic data shared with EU hospitals without signed DPAs.
  GDPR Art.28 requires a written contract for every data processor relationship.
  Two of three EU hospital partners have no DPA in place.
- Likelihood: High | Impact: High | Risk Level: High
- Regulatory: GDPR Art.28, Art.83(4) — fines up to 10M EUR or 2% global turnover

---

## R-008 — No Encryption in Transit on Internal VPC API Calls

- Assets: A-001, A-002, A-003
- Vulnerability: Lambda-to-RDS and Lambda-to-S3 calls use HTTP. No SSL enforced on RDS.
  Genomic PHI travels in cleartext on internal network.
- Likelihood: Moderate | Impact: Very High | Risk Level: High
  (NIST Table I-2: M x VH = H)
- Regulatory: GDPR Art.32; HIPAA 164.312(e)(1) transmission security; ISO 27001 A.8.24

---

## R-009 — All Developers Have AdministratorAccess (No Least Privilege)

- Asset: A-004 (IAM Configuration)
- Vulnerability: All 8 IAM users have AWS-managed AdministratorAccess. No role separation
  between dev, ops, and security. Any compromised credential = full account takeover.
- Likelihood: High | Impact: High | Risk Level: High
- Regulatory: GDPR Art.25; HIPAA 164.308(a)(3); ISO 27001 A.5.15, A.5.18

## R-010 — RDS Not Encrypted at Rest

- Likelihood: Low | Impact: Very High | Risk: Moderate (L x VH = M per Table I-2)
- If AWS storage is compromised (snapshot leak), genomic metadata exposed in cleartext.
- Regulatory: GDPR Art.32; HIPAA 164.312(a)(2)(iv); ISO 27001 A.8.24

## R-011 — No MFA for Developer IAM Accounts

- Likelihood: Moderate | Impact: Moderate | Risk: Moderate
- 8 IAM users with no MFA. Credential theft directly compromises accounts.
- Regulatory: ISO 27001 A.5.17; AWS CIS Benchmark 1.10

## R-012 — No VPC Flow Logs

- Likelihood: Moderate | Impact: Moderate | Risk: Moderate
- Cannot detect lateral movement, exfiltration patterns, or unexpected connections.
- Regulatory: ISO 27001 A.8.15, A.8.16

## R-013 — No S3 Object Lock or Versioning (Ransomware Risk)

- Likelihood: Moderate | Impact: Moderate | Risk: Moderate
- Ransomware deletion is unrecoverable. No WORM for compliance retention requirements.
- Regulatory: HIPAA 164.312(c)(1) integrity; ISO 27001 A.8.12

## R-014 — EC2 Analysis Cluster Has Public IP Addresses

- Asset: A-006 (EC2 Bioinformatics Analysis Cluster)
- Likelihood: Moderate | Impact: Moderate | Risk: Moderate
- Should be in private subnet with NAT gateway. SSH exposed to 0.0.0.0/0, shared key
  pair across the team compounds the exposure (see A-006 STRIDE, Spoofing).
- Regulatory: ISO 27001 A.8.20; AWS Well-Architected SEC05

## R-015 — No Automated RDS Backup Restoration Testing

- Likelihood: Low | Impact: Very High | Risk: Moderate (L x VH = M per Table I-2)
- RDS automated backups enabled but never tested. Recovery may fail in a real incident.
- Regulatory: HIPAA 164.308(a)(7); ISO 27001 A.8.13

## R-016 — No AWS Config Rules for Compliance Monitoring

- Likelihood: Moderate | Impact: Moderate | Risk: Moderate
- No automated compliance checks. New resources may not meet security baselines.
- Regulatory: ISO 27001 A.8.8; AWS CIS Benchmark Section 3

## R-017 — No Documented Data Classification Policy

- Likelihood: Moderate | Impact: Moderate | Risk: Moderate
- Root cause of several misconfigurations above. Employees cannot determine what
  data requires what level of protection.
- Regulatory: GDPR Art.5 (data minimization); ISO 27001 A.5.12, A.5.13

## R-018 — Weak IAM Password Policy

- Likelihood: Low | Impact: Moderate | Risk: Low (L x M = L per Table I-2)
- Min 8 chars, no complexity, no rotation. Mitigated if MFA enforced (R-011).
- Regulatory: ISO 27001 A.5.17; AWS CIS Benchmark 1.8-1.11

## R-019 — S3 Access Logs Not Enabled

- Likelihood: Low | Impact: Low | Risk: Low
- Supplementary to CloudTrail (R-006). Reduces forensic capability for bucket access.
- Regulatory: ISO 27001 A.8.15

## R-020 — CloudTrail Logs Stored Without Encryption

- Likelihood: Low | Impact: High | Risk: Low (L x H = L per NIST Table I-2)
- CloudTrail inactive (R-006). When activated, logs must be encrypted with CMK
  to prevent tampering by IAM admin users.
- Regulatory: HIPAA 164.312(a)(2)(iv); ISO 27001 A.8.24

## R-021 — SQL Injection via Lambda Alters Clinical Metadata

- Asset: A-002 (RDS Clinical Metadata Database)
- Likelihood: Moderate | Impact: High | Risk: Moderate (M x H = M per Table I-2)
- Lambda functions query the RDS clinical database without input validation or
  parameterized queries, allowing an attacker who reaches the ingestion pipeline to
  alter or exfiltrate patient records directly.
- Regulatory: HIPAA 164.312(c)(1) integrity; ISO 27001 A.8.28 (secure coding); GDPR Art.32

## R-022 — API Error Responses Expose Internal Infrastructure Details

- Asset: A-003 (API Gateway + Lambda)
- Likelihood: Moderate | Impact: Moderate | Risk: Moderate
- Unhandled exceptions in the Lambda ingestion function return raw stack traces,
  including internal VPC subnet IDs and Lambda ARNs, aiding an attacker in mapping
  the internal network.
- Regulatory: ISO 27001 A.8.26 (application security requirements)

## R-023 — No Rate Limiting on Ingestion API (Lambda Concurrency Exhaustion)

- Asset: A-003 (API Gateway + Lambda)
- Likelihood: Moderate | Impact: Moderate | Risk: Moderate
- API Gateway has no throttling or usage plan configured; a flood of large FASTQ
  uploads can exhaust Lambda concurrency limits, denying service to legitimate
  hospital partners.
- Regulatory: ISO 27001 A.8.26; AWS Well-Architected Reliability Pillar

## R-024 — EC2 Analysis Cluster EBS Volumes Not Encrypted at Rest

- Asset: A-006 (EC2 Bioinformatics Analysis Cluster)
- Likelihood: Low | Impact: High | Risk: Low (L x H = L per Table I-2)
- Genomic data resides temporarily on unencrypted EBS volumes during pipeline
  execution. A compromised snapshot or improperly disposed volume would expose
  PHI in cleartext.
- Regulatory: HIPAA 164.312(a)(2)(iv); ISO 27001 A.8.24; GDPR Art.32

## R-025 — Nextflow Pipeline Executes With Root Privileges

- Asset: A-006 (EC2 Bioinformatics Analysis Cluster)
- Likelihood: Moderate | Impact: High | Risk: Moderate (M x H = M per Table I-2)
- Default Nextflow execution configuration runs all pipeline steps as the root user,
  with no least-privilege execution role. A compromised pipeline dependency would
  grant full control of the instance.
- Regulatory: ISO 27001 A.8.2 (privileged access rights); AWS Well-Architected
  Security Pillar (SEC04)
