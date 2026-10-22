I scored the questionnaire against CCM 4.0.13, the version you confirmed. The overall score is **1.00 / 4 (25.0%), FAIL**. All 17 domains scored exactly 25% and failed.

The scores are uniform because the source file has no real answers. All 261 questions use the same single sentence for both the CSP implementation description and the CSC responsibilities:

> *"Additional details regarding security controls for [Vendor] products and [Vendor] as an enterprise can be found on the [Vendor] Trust Portal: [vendor URL]"*

Every row got through the mechanical checks, since none was blank, a bare "yes"/"no", or had an invalid ownership value. Each then scored **1 (Weak / Minimal)**. The text doesn't contradict the answers, but it says nothing about any specific control, and the vendor name and link are still template placeholders. It reads like an unfilled template. Run it again on a completed CAIQ if you want scores that tell domains apart.

**About the questionnaire:**
- **Answers:** 231 Yes, 30 NA, no No.
- **Ownership:** every answered row says "CSP-owned". The CCM 4.0.13 guidance describes about 213 of the 261 questions as shared between provider and customer, so the customer's duties are never stated. I estimated that count by matching keywords in the CCM ownership notes and correcting a few by hand. It only affects the written feedback, not the scores.
- **NA answers:** none of the 30 explains why the control doesn't apply. Several conflict with other answers:
  - STA-02.1, STA-03.1 and STA-04.1 say SSRM (shared responsibility model) delineation is NA, yet the questionnaire assigns ownership to every control.
  - AIS-01.2 and DCS-02.2 are NA although their parent policy questions are answered Yes.
  - IVS-03.2 (encrypting traffic between environments) is NA although IVS-03.1 confirms those environments communicate.

  I kept these at 1 rather than 0. The scoring rules' automatic 0 is for an answer that contradicts its own explanation, and these instead conflict with other answers in the questionnaire. Each case is flagged in the row's justification.
- **CEK-08.1:** ownership is blank, which is allowed on an NA row.
- **Missing details:** the file doesn't name the organization or the cloud service, so the report shows them as "Not stated".

**What PASS/FAIL means:** it rates the quality and consistency of the written answers, not whether controls are actually in place. A well-explained "No" scores 2/4 (50%), which counts as a pass on this scale.

This is an informational, AI-assisted assessment. It is not a CSA certification, audit, attestation, STAR Registry listing or badge. The full disclaimer is the last page of the PDF.

Files are in the current directory:
- `CAIQ-v4.0.2-sample-scored.xlsx`: the scored workbook with the 22 required columns, plus an `About` sheet.
- `CAIQ-v4.0.2-sample-scoring-report.pdf`: the 12-page report (overview, domain chart, one block per domain, disclaimer).
