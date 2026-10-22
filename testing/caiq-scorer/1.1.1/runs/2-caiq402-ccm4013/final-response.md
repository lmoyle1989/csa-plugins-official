The questionnaire scored **0.96 / 4 (23.9%), FAIL**. All 17 domains fail. The low score is not a formatting problem: the questionnaire was never actually filled in.

**Version check:** CAIQ v4.0.2 was scored against CCM 4.0.13, as you confirmed. All 197 control IDs and titles in the questionnaire match the CCM 4.0.13 reference data exactly. There are 261 questions: 231 answered "Yes" and 30 answered "NA".

**Main finding:** every one of the 261 rows gives the same template sentence in both the CSP and CSC columns: *"Additional details regarding security controls for [Vendor] products and [Vendor] as an enterprise can be found on the [Vendor] Trust Portal: [vendor URL]"*. The vendor name and URL placeholders were never replaced. Every row passes the mechanical checks: ownership is valid on all rows, and the one blank ownership (CEK-08.1) is on an NA row, where that is allowed. But no row describes how a control is actually implemented.

**How rows were scored:**
- **"Yes" rows score 1/4 (weak).** The answers don't contradict anything, but they contain no detail about any control.
- **11 "NA" rows score 0/4.** Each NA is contradicted by the respondent's own "Yes" answers on the same or a linked control:
  - AIS-01.2 (AIS-01.1 is "Yes")
  - BCR-08.1 and 08.2: backups are "NA", but BCR-08.3 says backups can be restored
  - IPY-01.1 to 01.4 (IPY-01.5 is "Yes")
  - IVS-03.2 (the other IVS-03 questions are "Yes")
  - STA-02.1, 03.1 and 04.1 (STA-01, 05 and 06 confirm the shared-responsibility model exists)

  Scoring these as contradictions is my judgment call, since the conflict is between rows rather than within one row's answer and explanation. If you treated them like the other NA rows instead, the overall score would rise only slightly, to about 25%, and every domain would still fail.
- **The other 19 "NA" rows score 1/4.** None gives any reason why the control doesn't apply.
- **Ownership:** every row that has an ownership value says "CSP-owned", but CCM guidance treats most of these controls as shared with the customer. I flagged this in the feedback columns.

| Domain | Score |
|---|---|
| 12 domains, including Audit & Assurance, Datacenter Security and IAM | 25.0% |
| Infrastructure & Virtualization Security | 23.2% |
| Application & Interface Security | 22.7% |
| Business Continuity Mgmt & Operational Resilience | 22.2% |
| Supply Chain Mgmt, Transparency & Accountability | 20.0% |
| Interoperability & Portability | 12.5% (lowest) |

PASS/FAIL here measures only how well the written answers are written and whether they agree with each other. It doesn't show whether any control is actually in place. A domain answered entirely with well-written "No" responses would score 50% and show as PASS. This is an informational, AI-assisted assessment, not a CSA certification, audit or STAR listing; the disclaimer is on the last page of the PDF.

One thing to be aware of: another session was using the same scratch folder, `/tmp/caiqwork`, for an AI-CAIQ run. None of the files clashed, and I moved my work into a private temporary folder, re-ran it there and deleted it afterwards. I didn't touch the other session's files.

Files are in the current directory:
- `CAIQ-v4.0.2-sample_scored.xlsx`
- `CAIQ-v4.0.2-sample_scoring_report.pdf`
