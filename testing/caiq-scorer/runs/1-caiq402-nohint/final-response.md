I haven't scored the questionnaire yet. It declares a framework version that this scorer package has no reference data for, and the scorer's rules say to check with you before going further.

**About the file**
- **Mode:** standard CAIQ, scored against CCM. It has the usual columns (`CSP CAIQ Answer`, `SSRM Control Ownership`, `CCM Control ID`, `CCM Domain Title` and so on).
- **Version:** cell A1 and the sheet name say **CAIQ v4.0.2**. The Introduction sheet says it "aligns with the CCMv4.0.2 control specifications."
- **Contents:** 263 questions. 231 are answered Yes, 30 NA and 2 are blank. 260 have ownership set to `CSP-owned` and 3 have no ownership.

**Why I stopped**
This package (CSA Valid-AI-ted Scorer 1.1.0) only has reference data for **CCM 4.0.13, CCM 4.1.0, AICM 1.0.2 and AICM 1.1.0**. There's nothing for CCM 4.0.2. Scoring against one of the other versions would attach that version's guideline text, which may not match the controls your questionnaire was written against. The scorer is built to stop and flag this case rather than switch versions on its own. A newer version of the scorer with CCM 4.0.2 data may be available from the Cloud Security Alliance.

**Choose one**
1. **Get a matching scorer.** Check with CSA for a release that covers CCM 4.0.2.
2. **Score against CCM 4.0.13 anyway.** This is the closest 4.0.x data in the package, but it's your call, not mine. If you choose it, I'll state the version mismatch in the workbook's About sheet and in the PDF. I'll also list any control IDs that aren't in the 4.0.13 file instead of guessing their text.
3. **Score against CCM 4.1.0.** Same caveats as option 2, with a bigger version gap.

Once you pick one, I'll run the full scoring and produce the scored workbook (.xlsx) and the summary report (PDF).

One thing to know in advance: nearly every answer reuses the same text, "Additional details … can be found on the [Vendor] Trust Portal: [vendor URL]." Under this methodology, an answer that points elsewhere instead of describing what's actually done will likely score low whichever version you choose.
