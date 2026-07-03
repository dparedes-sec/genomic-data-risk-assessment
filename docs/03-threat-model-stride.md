# Threat Model — STRIDE per Element

## A-001 — S3 Genomic Sequences Bucket

| STRIDE | Threat | Specific Vulnerability | Likelihood | Impact | Linked Risk |
| --- | --- | --- | --- | --- | --- |
| Spoofing | Attacker uses stolen AWS credentials to access bucket | No MFA on IAM accounts | High | Very High | R-002, R-004 |
| Tampering | Malicious FASTQ overwrites valid sequence | No S3 Object Lock or versioning | Moderate | High | R-013 |
| Repudiation | Cannot prove who accessed genomic data | No S3 access logging or CloudTrail data events | High | High | R-006 |
| Info. Disclosure | Bucket publicly accessible — any internet user can download sequences | S3 Block Public Access not enabled | High | Very High | R-001 |
| Denial of Service | Ransomware actor deletes all objects | No S3 Object Lock (WORM), no versioning | Moderate | Very High | R-013 |
| Elevation of Privilege | Lambda role with s3:* allows access to all buckets | Over-privileged IAM role | High | High | R-005 |

## A-002 — RDS Clinical Metadata Database

| STRIDE | Threat | Specific Vulnerability | Likelihood | Impact | Linked Risk |
| --- | --- | --- | --- | --- | --- |
| Spoofing | Brute-force against DB admin password | RDS publicly accessible, no IP whitelist | High | Very High | R-003 |
| Tampering | SQL injection via Lambda alters patient records | No input validation in Lambda DB queries | Moderate | High | R-021 |
| Repudiation | DB admin denies unauthorized access | No PostgreSQL audit logging configured | Moderate | Moderate | R-006 |
| Info. Disclosure | Physical AWS storage compromise exposes cleartext data | No encryption at rest on RDS | Low | Very High | R-010 |
| Denial of Service | Connection pool exhaustion blocks clinical access | No connection pooling or max connections limit | Low | High | N/A |
| Elevation of Privilege | Application user has full DB admin rights | No role separation in PostgreSQL | Moderate | High | R-009 |

## A-003 — API Gateway + Lambda (Ingestion Endpoint)

| STRIDE | Threat | Specific Vulnerability | Likelihood | Impact | Linked Risk |
| --- | --- | --- | --- | --- | --- |
| Spoofing | Attacker impersonates hospital and uploads malicious data | No authentication on /v1/upload endpoint | High | High | R-004 |
| Tampering | Attacker injects corrupted genomic sequences | No payload integrity validation (no HMAC/signature) | High | High | R-004 |
| Repudiation | Partner denies uploading specific file; no audit trail | API Gateway access logging not enabled | Moderate | Moderate | R-006 |
| Info. Disclosure | API error responses expose internal Lambda/VPC details | No error sanitization in exception handlers | Moderate | Moderate | R-022 |
| Denial of Service | Flood of large FASTQ uploads exhausts Lambda concurrency | No throttling or rate limiting on API Gateway | Moderate | Moderate | R-023 |
| Elevation of Privilege | Lambda role allows s3:* on all buckets | Over-privileged IAM role (not resource-scoped) | High | High | R-005 |

## A-004 — IAM Configuration

| STRIDE | Threat | Specific Vulnerability | Likelihood | Impact | Linked Risk |
| --- | --- | --- | --- | --- | --- |
| Spoofing | Root account compromise via phishing | Root account used daily; no MFA enforced | High | Very High | R-002 |
| Tampering | IAM policy modified to expand attacker permissions | Developers with AdministratorAccess can modify any policy | High | Very High | R-009 |
| Repudiation | CloudTrail not enabled — IAM API calls not logged | No CloudTrail configuration | High | High | R-006 |
| Info. Disclosure | Long-lived access keys committed to GitHub | No key rotation policy; no secret scanning in CI | Moderate | Very High | R-002 |
| Denial of Service | Accidental IAM lockout denies all access | No break-glass account documented | Low | High | N/A |
| Elevation of Privilege | Developer creates new IAM user with AdministratorAccess | All devs have iam:CreateUser, iam:AttachUserPolicy | High | Very High | R-009 |

## A-005 — Partner Data Sharing Pipeline

| STRIDE | Threat | Specific Vulnerability | Likelihood | Impact | Linked Risk |
| --- | --- | --- | --- | --- | --- |
| Spoofing | Shared API key used by multiple partners — cannot distinguish | No per-partner unique credentials | Moderate | Moderate | R-007 |
| Tampering | Exported genomic package modified in transit | No end-to-end integrity check (no checksum by recipient) | Moderate | High | R-008 |
| Repudiation | Cannot prove what data was shared with which partner | No audit log of data exports | High | High | R-007 |
| Info. Disclosure | Data shared with EU partners without GDPR DPA | No DPA in place for 2 of 3 EU hospital partners | High | Very High | R-007 |
| Denial of Service | Partner timeout floods Lambda retry queue | No dead-letter queue on Lambda retry | Low | Low | N/A |
| Elevation of Privilege | Shared export key grants access beyond agreed scope | Key not scoped to specific S3 prefix per partner | Moderate | High | R-007 |

## A-006 — EC2 Bioinformatics Analysis Cluster

| STRIDE | Threat | Specific Vulnerability | Likelihood | Impact | Linked Risk |
| --- | --- | --- | --- | --- | --- |
| Spoofing | Attacker uses a stolen or shared SSH key to impersonate a legitimate cluster user | Single SSH key pair shared across the team; port 22 open to 0.0.0.0/0 | Moderate | Moderate | R-014 |
| Tampering | Pipeline scripts modified in place to silently alter analysis results | No integrity verification (checksums/signing) on Nextflow scripts pulled from S3 | Low | Moderate | N/A |
| Repudiation | Cannot determine which team member ran a given analysis job | Shared OS-level access; no per-user session logging on the instance | Moderate | Moderate | R-006 |
| Info. Disclosure | Genomic data readable from an exposed or leaked disk snapshot | EBS volumes not encrypted at rest | Low | High | R-024 |
| Denial of Service | Large batch jobs exhaust all instance resources, blocking other analyses | No auto-scaling group; no per-job resource limits | Low | Moderate | N/A |
| Elevation of Privilege | A compromised pipeline dependency gains root on the host | Nextflow pipeline executes as root by default; no least-privilege execution role | Moderate | High | R-025 |
