I've checked the file and stopped before scoring. The scorer requires your confirmation of the framework version before it attaches any guideline text, and won't pick one by default.

## Version check

- **Mode:** `caiq`. The sheet `CAIQv4.0.2` has the standard columns (`CSP CAIQ Answer`, `SSRM Control Ownership`, `CCM Control ID`, `CCM Domain Title`, and so on).
- **Questionnaire version:** CAIQ v4.0.2. Cell A1 just says `v4.0.2`; there's no JSON metadata.
- **Proposed reference file:** CCM 4.0.13. All **197 of 197** control IDs and titles in the questionnaire match it exactly, with none missing or different.
- **Questions found:** 261, across 17 domains. I skipped the "End of Standard" and copyright rows because they aren't questions.

**Do you confirm CCM 4.0.13 as the framework version?** Reply "yes" and I'll score all 261 questions, then save `CAIQ-v4.0.2-sample_scored.xlsx` and `CAIQ-v4.0.2-sample_scoring_report.pdf` next to the input file.

## Expect low scores

You should know this before I go ahead:

- **The descriptions are boilerplate.** Every row has the same text in both the CSP Implementation Description and the CSC Responsibilities field: *"Additional details regarding security controls for [Vendor] products and [Vendor] as an enterprise can be found on the [Vendor] Trust Portal: [vendor URL]"*. It's a placeholder link with no detail about how any control is implemented. It's long enough to pass the automatic checks, so the AI scoring step will handle it, but it will score low (around 0–1) on every question.
- **Answers:** 231 Yes, 30 NA, no "No".
- **Ownership:** 260 rows are `CSP-owned`. CEK-08.1 is blank, but it's answered `NA`, so the rules allow that and it won't be marked down for ownership.

The scorer's working files are in `/tmp/caiqscore.BSfD`. I'll delete that folder once the outputs are written.
