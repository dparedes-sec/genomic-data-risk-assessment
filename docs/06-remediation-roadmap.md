# 90-Day Remediation Roadmap — GenomaCorp

## Phase 1 — Days 1-30: Very High Risks (Priority 1)

Goal: Eliminate the three Very High risks before any new feature development.

| Priority | Action | Risk(s) | Owner | Effort | Verification |
| --- | --- | --- | --- | --- | --- |
| 1 | Enable S3 Block Public Access at account level | R-001 | Cloud Ops | 1 hour | aws s3api get-public-access-block |
| 2 | Enable MFA on root account; generate break-glass procedure | R-002 | Cloud Ops | 2 hours | IAM MFA device status check |
| 3 | Disable RDS public accessibility; move to private subnet | R-003 | Cloud Ops | 4 hours | RDS config: PubliclyAccessible=false |
| 4 | Enable CloudTrail (management + data events for S3 and RDS) | R-006 | Cloud Ops | 2 hours | cloudtrail get-trail-status |
| 5 | Add API key authentication to /v1/upload endpoint | R-004 | Dev | 8 hours | Test unauthenticated POST -> 401 |

## Phase 2 — Days 31-60: High Risks

Goal: Implement least privilege, encryption in transit, and compliance documentation.

| Priority | Action | Risk(s) | Owner | Effort | Verification |
| --- | --- | --- | --- | --- | --- |
| 6 | Create scoped IAM roles for each Lambda (bucket + prefix-specific) | R-005, R-009 | Dev + Cloud Ops | 1 day | IAM Access Analyzer review |
| 7 | Remove AdministratorAccess from all developers; create role-based groups | R-009 | Cloud Ops | 4 hours | IAM policy review |
| 8 | Enforce TLS on Lambda-to-RDS connections; enable RDS SSL | R-008 | Dev | 4 hours | Check pg_stat_ssl on RDS |
| 9 | Draft and sign GDPR Data Processing Agreements with EU hospital partners | R-007 | Legal + Management | 2 weeks | Signed DPA documents |
| 10 | Enable MFA for all developer IAM accounts | R-011 | Cloud Ops | 2 hours | aws iam get-credential-report |

## Phase 3 — Days 61-90: Moderate and Low Risks + Compliance Infrastructure

Goal: Build detection and monitoring; address remaining gaps.

| Priority | Action | Risk(s) | Owner | Effort | Verification |
| --- | --- | --- | --- | --- | --- |
| 11 | Enable RDS encryption at rest (requires snapshot restoration) | R-010 | Cloud Ops | 4 hours + downtime | RDS StorageEncrypted=true |
| 12 | Enforce strong IAM password policy (min length, complexity, rotation) | R-018 | Cloud Ops | 1 hour | aws iam get-account-password-policy shows updated policy |
| 13 | Enable VPC Flow Logs; configure S3 access logs | R-012, R-019 | Cloud Ops | 1 hour | CloudWatch Logs group creation |
| 14 | Enable S3 versioning + Object Lock on genomacorp-sequences-prod | R-013 | Cloud Ops | 2 hours | s3api get-bucket-versioning |
| 15 | Move EC2 cluster to private subnet; restrict SSH to bastion host | R-014 | Cloud Ops | 1 day | Security group review |
| 16 | Enable AWS Config + Security Hub with CIS AWS Benchmark | R-016 | Cloud Ops | 2 hours | Config Rules compliance score |
| 17 | Test RDS backup restoration; document recovery time | R-015 | Cloud Ops | 4 hours | Successful restore + RTO documented |
| 18 | Write data classification policy and data handling procedures | R-017 | Management | 1 week | Policy document published |
| 19 | Encrypt CloudTrail logs with CMK | R-020 | Cloud Ops | 1 hour | cloudtrail describe-trails |
| 20 | Add input validation / parameterized queries to Lambda DB access | R-021 | Dev | 1 day | Static analysis clean + manual SQLi test negative |
| 21 | Sanitize API error responses; implement generic error handling middleware | R-022 | Dev | 4 hours | Error responses reviewed — no stack traces or internal ARNs exposed |
| 22 | Configure API Gateway usage plan with throttling limits | R-023 | Dev + Cloud Ops | 4 hours | Usage plan active; load test confirms throttling triggers correctly |
| 23 | Enable EBS encryption at rest on EC2 analysis cluster | R-024 | Cloud Ops | 2 hours | EBS volumes show StorageEncrypted=true |
| 24 | Reconfigure Nextflow execution with least-privilege IAM instance role and non-root user | R-025 | Dev + Cloud Ops | 1 day | Pipeline runs as non-root; instance role scoped to required actions only |
