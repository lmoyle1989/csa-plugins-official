The CAIQ is scored. The overall result is **2.01 out of 4 (50.2%) — PASS**, but only just above the 50% line. Both output files are in the current directory.

**Files**
- `CAIQ-v4.1.0-sample_scored.xlsx`: one row per question with all 22 required columns, plus an `About` sheet with the scorer version, mode, CCM 4.1.0, the date and the disclaimer.
- `CAIQ-v4.1.0-sample_scoring_report.pdf`: 9 pages. Overview, a chart of the domain scores, a breakdown for each domain, and the disclaimer page.

**Setup checks**
- The file is a standard CAIQ (cloud provider questionnaire), and its first cell names CCM 4.1.0 and CAIQ 4.1.0.
- All 207 controls match the CCM 4.1.0 reference data exactly, by control ID and title.
- There are 283 questions: 278 "Yes", 3 "No" and 2 "NA".

**Main finding: 34 rows were automatically scored 0.** Their ownership is "3rd-party outsourced", "Shared CSP and 3rd-party" or "Shared CSP and CSC". The method requires a description of the customer's responsibilities for those types, and that column is empty on every row. 21 of these zeros are in Datacenter Security, where physical security is handled by AWS. The other 249 rows average 2.28 (57.0%), so filling in or correcting the ownership details would raise the score a lot.

| Domain | Score | Status |
|---|---|---|
| Audit & Assurance | 65.6% | PASS (highest) |
| Application & Interface Security | 63.5% | PASS |
| Change Control and Configuration Management | 62.5% | PASS |
| Business Continuity Management and Operational Resilience | 48.7% | FAIL |
| Cryptography, Encryption & Key Management | 45.7% | FAIL |
| Logging and Monitoring | 44.7% | FAIL |
| Supply Chain Management, Transparency, and Accountability | 42.1% | FAIL |
| Datacenter Security | 18.8% | FAIL (lowest) |

The other 9 domains score between 50.0% and 60.0% and pass. Human Resources sits exactly on the line at 50.0%.

**Other patterns in the answers**
- The first policy question in many domains reuses the same generic text ending "See more generally A&A-01.1".
- Several answers are only a pointer to another question, for example HRS-02.2, IAM-02.2, SEF-02.2 and GRC-03.1.
- Some wording is non-committal, such as "may be updated if necessary", and some placeholders like "[vendor URL]" were never filled in.
- Only one answer (A&A-05.1) scored 4 out of 4.
- All three "No" answers (BCR-10.2, CEK-08.1, LOG-08.1) are well explained and score 2, the most a "No" can get.
- The "NA" answer to CCC-05.1 scored 1, because the control appears to apply.

PASS/FAIL here measures how complete and consistent the written answers are, not whether the controls are actually in place. A well-written "No" scores 2/4 (50%), so a domain can pass without the control being implemented.

The scoring of the 249 rows was split across five AI agents working in parallel from the same CSA scoring prompt and instructions. The rules were then rechecked on every row: scores are whole numbers from 0 to 4, "No" answers are capped at 2, and none of the 34 automatic zeros was overridden. Working files were kept outside your folder and deleted.

This is an informational, AI-assisted assessment of the written answers only. It is not a CSA certification, audit, attestation, STAR Registry listing or badge; the full disclaimer is the last page of the PDF.
