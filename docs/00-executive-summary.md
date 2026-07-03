# Executive Summary — GenomaCorp Genomic Data Security Risk Assessment

## Purpose

This assessment evaluates the information security risk posture of GenomaCorp's
AWS-based genomic data platform, following NIST SP 800-30 Rev.1 methodology. It was
conducted in preparation for ISO 27001 certification and in light of GDPR and HIPAA
obligations tied to the processing of genomic and clinical PHI from hospital partners
in the EU and the US.

## Scope

Six key assets were assessed: raw genomic sequence storage (A-001), the clinical
metadata database (A-002), the data ingestion API (A-003), the IAM and access control
configuration (A-004), the partner data-sharing pipeline (A-005), and the EC2
bioinformatics analysis cluster (A-006). Each asset was subjected to STRIDE threat
modeling across all six threat categories, producing 36 threat scenarios.

## Key Findings

The assessment identified 25 risks: 3 Very High, 6 High, 12 Moderate, and 4 Low.
GenomaCorp currently has no documented security controls in place — every asset
assessed operates at, or near, its baseline (unmitigated) risk exposure.

The three Very High risks share a common root cause: absence of basic access controls
at the infrastructure layer.

- A publicly accessible S3 bucket exposes approximately 2TB of raw genomic sequence
  data belonging to patients in the EU and US (R-001).
- The AWS root account operates without multi-factor authentication, and all eight
  developers hold AdministratorAccess (R-002, R-009).
- The clinical metadata database, holding 12,000 patient records, is directly
  reachable from the internet with no network-layer protection (R-003).

Any one of these three risks, exploited independently, would constitute a reportable
breach under both GDPR Article 33 and the HIPAA Breach Notification Rule.

## Regulatory Exposure

GenomaCorp is not currently positioned to pass an ISO 27001 certification audit.
Under GDPR, aggregate exposure from the Very High risks could reach 4% of global
annual turnover (Art. 83) if they materialize simultaneously with the current lack of
breach detection capability (R-006 — no CloudTrail logging). HIPAA safeguard gaps span
both the Administrative (Section 164.308) and Technical (Section 164.312) safeguard
categories.

## What Changed Since the Initial Draft

This version closes two traceability gaps identified during internal technical review:
the EC2 analysis cluster is now a formally inventoried asset (A-006) with its own
STRIDE analysis, and five threats that were identified during STRIDE modeling but had
not yet been assigned a risk ID (R-021 through R-025) are now fully incorporated into
the risk register.

## Recommendation

Remediation of the 3 Very High risks is estimated at under 2 days of combined
engineering effort and should precede any further feature development. A full
30/60/90-day prioritized roadmap is provided in Document 06.

## Risk Summary at a Glance

| Severity | Count | % of total |
| --- | --- | --- |
| Very High | 3 | 12% |
| High | 6 | 24% |
| Moderate | 12 | 48% |
| Low | 4 | 16% |
| **Total** | **25** | **100%** |
