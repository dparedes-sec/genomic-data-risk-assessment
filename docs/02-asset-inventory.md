# Asset Inventory — GenomaCorp

## Data Classification Policy (preliminary)

| Level | Definition | Examples |
| --- | --- | --- |
| Restricted — Genomic PHI | Genomic sequences linkable to individuals | FASTQ files with patient ID |
| Restricted — PHI/PII | Identifiable patient data | Name, DOB, diagnosis, genomic ID |
| Restricted — IAM credentials | AWS access keys and IAM configuration | Root credentials, developer IAM |
| Confidential | Business-sensitive, non-patient data | API keys, partner contracts, endpoints |
| Internal | Non-sensitive operational data | Logs without PII, system metrics |

## Key Asset Inventory

| Asset ID | Name | Type | Classification | AWS Component | Data Subjects |
| --- | --- | --- | --- | --- | --- |
| A-001 | Raw Genomic Sequences (FASTQ) | Data store | Restricted — Genomic PHI | S3: genomacorp-sequences-prod | EU + US patients |
| A-002 | Clinical Metadata Database | Data store | Restricted — PHI/PII | RDS PostgreSQL: prod-clinical-db | EU + US patients |
| A-003 | Genomic Data Ingestion API | Service | Confidential — API keys/endpoint | API Gateway + Lambda | N/A |
| A-004 | IAM & Access Control Configuration | System | Restricted — IAM credentials | AWS IAM | N/A |
| A-005 | Partner Data Sharing Pipeline | Service | Restricted | Lambda + S3 transfer | EU + US patients (indirect) |
| A-006 | EC2 Bioinformatics Analysis Cluster | Compute | Restricted — Genomic PHI in processing | EC2 (Nextflow) | EU + US patients (indirect) |

## Asset Details

### A-001 — Raw Genomic Sequences

- Volume: ~2TB, growing 200GB/month
- Retention: 10 years (clinical requirement)
- Current controls: None — bucket is publicly accessible
- Regulatory relevance: GDPR Art.9 (special category data), HIPAA 164.312(a)(2)(iv)

### A-002 — Clinical Metadata Database

- Records: ~12,000 patients across EU and US cohorts
- Fields: patient_id, dob, diagnosis_code, genomic_file_ref, consent_status
- Current controls: Password authentication, no encryption at rest
- Regulatory relevance: GDPR Art.25 (data protection by design), HIPAA 164.312(a)

### A-003 — Genomic Data Ingestion API

- Endpoint: https://api.genomacorp.io/v1/upload
- Usage: Hospital partners POST encrypted FASTQ archives
- Current controls: No authentication on upload endpoint (a significant gap — the
  endpoint accepts unauthenticated POST requests from any source)
- Regulatory relevance: GDPR Art.32 (security of processing)

### A-004 — IAM Configuration

- Current state: Root account used for daily operations. 8 IAM users, all with
  AdministratorAccess.
- MFA: Not enforced on any account
- Key rotation: Never
- Regulatory relevance: ISO 27001 A.5.15, A.5.16, A.8.2

### A-005 — Partner Data Sharing Pipeline

- Usage: Exports processed genomic data to hospital and research partners
- Current controls: Shared API key across all partners; no per-partner scoping
- Regulatory relevance: GDPR Art.28 (data processing agreements)

### A-006 — EC2 Bioinformatics Analysis Cluster

- Usage: Runs the Nextflow pipeline that processes raw FASTQ files into analyzed
  genomic variant calls. Genomic data resides on this instance's local storage and
  in memory for the duration of each processing job.
- Current controls: Public IP address assigned; SSH (port 22) open to 0.0.0.0/0 using
  a single SSH key pair shared across the engineering team; EBS volumes not encrypted
  at rest; the Nextflow pipeline executes with root privileges by default.
- Regulatory relevance: ISO 27001 A.8.20 (network security), A.8.24 (use of
  cryptography), A.8.2 (privileged access rights)
