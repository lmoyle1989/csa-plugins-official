I scored the AI-CAIQ against AICM 1.0.2 and saved the scored workbook and the PDF report in the current directory.

**Overall result: 0.95 / 4 (23.8%), FAIL.** All 18 domains are below the 50% line. PASS/FAIL here measures how good and consistent the written answers are, not whether the controls are actually in place. A well-written "No" scores 2/4, which counts as a pass.

- **Answers:** 184 Yes, 127 NA, no No answers.
- **Score spread:** 12 rows scored 3, 55 scored 2, 150 scored 1 and 94 scored 0. No row scored 4.
- **Mechanical checks:** no row failed them; every row was scored on its content.
- **Highest domains:** Model Security 38.2%, Governance, Risk and Compliance 31.9%, Data Security and Privacy 30.8%.
- **Lowest domains:** Cryptography 4.3%, Threat & Vulnerability Management 5.9%, Infrastructure Security 14.6%.

**What pulled the score down:**
1. **Answers numbered to the wrong version.** 36 controls (44 rows) were answered using AICM 1.1.0 control numbers, while the questionnaire is 1.0.2. For example, the "Output Validation" question (AIS-09) holds the input-validation answer. TVM-04 to TVM-13, LOG-08 to LOG-15 and IAM-12 to IAM-18 are each shifted by one control. The right content often sits in the next row, so re-numbering the answers to 1.0.2 would recover many of the zeros.
2. **"Yes" contradicted by the explanation (24 rows, all scored 0).** Most are policy questions answered "Yes" alongside text such as "does not define organizational … policies". Others include CEK-03 ("does not implement at-rest encryption"), DSP-02 (secure disposal) and MDS-01 ("operates at inference time, not training time").
3. **Generic NA answers.** All 127 NA rows use the same sentence and leave ownership as "Not Determined". I scored 37 of them 0 because the reason given ("organizational, physical, or HR") doesn't fit the control and the respondent's own answers elsewhere show it applies. The key-management controls are an example: the answers elsewhere describe signing keys and key rotation. The other 90 scored 1.
4. **Product features in place of organizational practice.** Many answers list source-file paths and pattern counts rather than policies, owners, review cycles and evidence. The customer column holds two stock statements, and one of them conflicts with the declared "Shared CSP-MP" ownership.

**Choices I made because the run couldn't stop for questions:**
- **Rows with no provider description.** The skill's rules on which descriptions are required are written for the older CAIQ ownership values. I applied them to the AI-CAIQ values. Under that reading, "Not Determined" ownership makes neither description mandatory. So the NA rows with an empty provider field were scored on their stated reason rather than given an automatic 0.
- **How the row text was produced.** I scored each row myself against the question and the AICM control and guidelines, and wrote a note for each one. The feedback, guideline quotes and readability comments in the workbook are built from those notes using standard phrasing.
- **Name not specified.** The file doesn't name the organization or the service (the answers use a "[Vendor]" placeholder), so the report says "Not specified".

The report ends with the required disclaimer. This is an informational, AI-assisted assessment, not a CSA certification, audit, STAR listing or badge.

Files are in the current directory:
- AI-CAIQ-v1.0.2-sample_scored.xlsx
- AI-CAIQ-v1.0.2-sample_scoring_report.pdf
