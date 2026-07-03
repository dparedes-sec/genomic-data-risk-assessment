# Risk Matrix — GenomaCorp Assessment

**Methodology:** NIST SP 800-30 Rev.1 — Table I-2  
**Scale:** Very High (🔴) · High (🟠) · Moderate (🟡) · Low (🟢) · Very Low (⚪)

---

## Reference Matrix (NIST SP 800-30 Table I-2)

| Likelihood ↓ \ Impact → | **Very Low** | **Low** | **Moderate** | **High** | **Very High** |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **Very High** | ⚪ VL | 🟢 L | 🟡 M | 🟠 H | 🔴 **VH** |
| **High** | ⚪ VL | 🟢 L | 🟡 M | 🟠 H | 🔴 **VH** |
| **Moderate** | ⚪ VL | 🟢 L | 🟡 M | 🟡 M | 🟠 H |
| **Low** | ⚪ VL | ⚪ VL | 🟢 L | 🟢 L | 🟡 M |
| **Very Low** | ⚪ VL | ⚪ VL | ⚪ VL | ⚪ VL | 🟢 L |

---

## GenomaCorp Risk Distribution on Matrix

| Likelihood ↓ \ Impact → | **Very Low** | **Low** | **Moderate** | **High** | **Very High** |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **High** | — | — | — | R-004 R-005<br/>R-006 R-007<br/>R-009 | 🔴 R-001<br/>🔴 R-002<br/>🔴 R-003 |
| **Moderate** | — | — | R-011 R-012<br/>R-013 R-014<br/>R-016 R-017 | — | 🟠 R-008 |
| **Low** | — | ⚪ R-019 | 🟢 R-018 | 🟢 R-020 | 🟡 R-010<br/>🟡 R-015 |

---

## Risk Distribution Summary

| Level | Count | Risk IDs | Immediate Action? |
|---|:---:|---|:---:|
| 🔴 **Very High** | 3 | R-001, R-002, R-003 | **Yes — within 24h** |
| 🟠 **High** | 6 | R-004, R-005, R-006, R-007, R-008, R-009 | Yes — within 30 days |
| 🟡 **Moderate** | 8 | R-010, R-011, R-012, R-013, R-014, R-015, R-016, R-017 | Yes — within 90 days |
| 🟢 **Low** | 2 | R-018, R-020 | Monitor — post-roadmap |
| ⚪ **Very Low** | 1 | R-019 | Monitor — post-roadmap |
| | **20** | | |

> **Note on Very High impact baseline for genomic data:** Genomic sequences and clinical
> metadata involving DNA profiles are classified as special category data (GDPR Art. 9)
> and PHI (HIPAA). Unlike passwords or financial data, a DNA profile is **immutable** —
> a breach cannot be remediated by credential rotation. This characteristic justifies
> treating any exposure of A-001 or A-002 as Very High impact regardless of likelihood,
> and is reflected in the elevated impact scores for R-001, R-002, R-003, R-008, R-010,
> and R-015.

---

*Source: NIST SP 800-30 Rev.1, Appendix I, Table I-2*  
*Full risk descriptions: [../docs/04-risk-register.md](../docs/04-risk-register.md)*