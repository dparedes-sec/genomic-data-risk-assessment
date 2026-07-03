# Compliance Gap Analysis — GenomaCorp

## GDPR (General Data Protection Regulation)

| Article | Requirement | Gap Identified | Linked Risks | Severity |
| --- | --- | --- | --- | --- |
| Art.9 | Special category data (genomic) requires explicit consent and extra protection | No consent management; genomic data stored without per-patient consent tracking | R-001, R-003 | Very High |
| Art.25 | Data protection by design and by default | Architecture built without security controls: public S3, public RDS, no IAM least privilege | R-001, R-003, R-009 | Very High |
| Art.28 | Data Processing Agreements with processors | No DPA with 2 of 3 EU hospital partners sharing genomic data | R-007 | Very High |
| Art.32 | Appropriate technical measures for security of processing | No encryption at rest (RDS), no encryption in transit (Lambda-RDS), no access controls on APIs, no input validation on DB queries | R-003, R-008, R-010, R-021, R-025 | Very High |
| Art.33 | Breach notification within 72 hours | No breach detection capability (no CloudTrail); no notification SOP | R-006 | High |
| Art.83 | Administrative fines | Cumulative exposure from Art.9/25/28/32 violations: up to 4% global turnover | All VH/H | Very High |

## HIPAA Security Rule (45 CFR Part 164)

| Safeguard | Section | Requirement | Gap | Linked Risks | Severity |
| --- | --- | --- | --- | --- | --- |
| Administrative | 164.308(a)(1) | Risk analysis | No formal risk assessment conducted before this project | All | Very High |
| Administrative | 164.308(a)(3) | Workforce access management | All employees have admin access; no role-based access | R-009 | High |
| Administrative | 164.308(a)(7) | Contingency plan | No DR test for RDS backup restoration | R-015 | Moderate |
| Technical | 164.312(a)(1) | Access control | No unique user identification; shared credentials in use | R-009 | High |
| Technical | 164.312(a)(2)(iv) | Encryption and decryption | RDS not encrypted at rest; no MFA on accounts; EC2 EBS volumes not encrypted | R-002, R-010, R-024 | High |
| Technical | 164.312(b) | Audit controls | No CloudTrail; no RDS audit logging configured | R-006 | High |
| Technical | 164.312(c)(1) | Integrity controls | No S3 Object Lock; no checksums on data exchange; no input validation on DB queries | R-013, R-021 | Moderate |
| Technical | 164.312(e)(1) | Transmission security | No TLS enforcement on Lambda-to-RDS; no HTTPS internal | R-008 | High |

## ISO 27001:2022 Annex A

| Control | Description | Gap | Linked Risks |
| --- | --- | --- | --- |
| A.5.12 | Information classification | No classification policy defined | R-017 |
| A.5.15 | Access control policy | No policy; all users have AdministratorAccess | R-009 |
| A.5.16 | Identity management | Root account active; no lifecycle management | R-002 |
| A.5.17 | Authentication information | Weak password policy; no MFA | R-011, R-018 |
| A.8.2 | Privileged access rights | Lambda roles over-privileged; developers have admin; Nextflow runs as root | R-005, R-009, R-025 |
| A.8.7 | Protection against malware | No bucket-level data integrity controls | R-013 |
| A.8.12 | Data leakage prevention | No DLP; public S3 bucket; no data masking | R-001 |
| A.8.13 | Information backup | Backups never tested for restoration | R-015 |
| A.8.15 | Logging | No CloudTrail; no application logging | R-006 |
| A.8.20 | Networks security | Public RDS; EC2 with public IPs; no network segmentation | R-003, R-014 |
| A.8.21 | Security of network services | No TLS enforcement on internal communications | R-008 |
| A.8.24 | Use of cryptography | No encryption at rest (RDS); no CMK on logs; EC2 EBS unencrypted | R-010, R-020, R-024 |
| A.8.26 | Application security requirements | No error sanitization on API responses; no rate limiting on ingestion endpoint | R-022, R-023 |
| A.8.28 | Secure coding | No input validation or parameterized queries in Lambda database access | R-021 |
