# caiq-scorer 1.1.1: test results

These are re-runs of the [1.1.0 tests](../README.md) against `caiq-scorer-1.1.1 @ 7d801b0`. They use the same anonymized inputs, prompts and environment, so the two sets can be compared run for run. This branch is for review only, and none of it is part of the plugin release.

**Short answer: the 1.1.1 fixes work, and it still scores.**
- Run 1 (CAIQ v4.0.2, no hints) now proposes CCM 4.0.13 after matching all 197 controls, rather than refusing.
- All three scoring runs produced a valid workbook and PDF.

The re-runs also exposed run-to-run variation in the judgment scores, which is worth knowing about before relying on PASS/FAIL near the 50% line. 1.1.1 does not change scoring, so this variation was already present in 1.1.0. See [Scores vary between runs](#scores-vary-between-runs).

## Install

The install pulled the 1.1.1 branch from GitHub ([`install.log`](install.log)):

```
claude plugin marketplace add lmoyle1989/csa-plugins-official#caiq-scorer-1.1.1   ✔
claude plugin install caiq-scorer@csa-plugins-official                            ✔
claude plugin details caiq-scorer                                                 caiq-scorer 1.1.1, Skills (1)
```

The skill now costs about 6.2k tokens each time it runs, up from 5.3k, because SKILL.md grew. Uninstalling was clean ([`uninstall.log`](uninstall.log)).

## Runs

The inputs are the same as for 1.1.0: real CAIQs posted on the STAR Registry, anonymized and not published. The prompts and each session's final response are in [`runs/`](runs/).

| Run | Input | 1.1.0 result | 1.1.1 result | 1.1.1 time | 1.1.1 cost |
|---|---|---|---|---|---|
| 1 | CAIQ v4.0.2, no hints | Refused: "no reference data for CCM 4.0.2" | **Proposed CCM 4.0.13** (197/197 controls match), asked to confirm | 38 s | $0.24 |
| 2 | CAIQ v4.0.2 → CCM 4.0.13 | 1.00/4 (25.0%) FAIL | 0.96/4 (23.9%) FAIL | 4.5 min | $1.13 |
| 3 | CAIQ v4.1.0 → CCM 4.1.0 | 2.22/4 (55.5%) PASS | 2.01/4 (50.2%) PASS | 7.2 min | $5.05 |
| 4 | AI-CAIQ v1.0.2 → AICM 1.0.2 | 0.95/4 (23.8%) FAIL | 1.23/4 (30.6%) FAIL | 25.0 min | $11.87 |

## The 1.1.1 fixes, checked

| Fix | 1.1.0 behaviour | 1.1.1 behaviour |
|---|---|---|
| CAIQ vs CCM version handling | Refused CAIQ v4.0.2 | Proposed CCM 4.0.13 with a 197/197 match, then waited for confirmation. Run 4 also reported 243/243 AICM 1.0.2 matches. |
| Footer rows | Run 1 counted 263 questions | 261 / 283 / 311 questions in every run. Footer rows skipped and said so. |
| Output names and location | Mixed `-scored` and `_scored`; one run left a `_work/` folder | `<name>_scored.xlsx` and `<name>_scoring_report.pdf` next to the input in every run, with nothing else left in the folder |
| Library check | None | No problems; `openpyxl` and `reportlab` were present |

All output checks from 1.1.0 passed again in runs 2–4:

- The workbook headers exactly match SKILL.md step 8.
- The `About` sheet shows `Package version 1.1.1`.
- All scores are integers from 0 to 4, and no "No" answer scored above 2.
- The `CSA Valid-AI-ted Scorer v1.1.1` footer is on every PDF page.
- The disclaimer, with "package version 1.1.1", is on the final page.
- No vendor names appear anywhere in the output.

## Scores vary between runs

1.1.1 changes nothing in the scoring prompts, rubric, mechanical rules or reference data. Even so, the same inputs scored differently:

| Run | Score distribution 1.1.0 (0/1/2/3/4) | Score distribution 1.1.1 (0/1/2/3/4) | Overall change |
|---|---|---|---|
| 2 | 0 / 261 / 0 / 0 / 0 | 11 / 250 / 0 / 0 / 0 | 25.0% → 23.9% |
| 3 | 34 / 5 / 117 / 119 / 8 | 34 / 31 / 118 / 99 / 1 | 55.5% → 50.2% |
| 4 | 94 / 150 / 55 / 12 / 0 | 27 / 199 / 73 / 12 / 0 | 23.8% → 30.6% |

What stayed the same and what moved:

- **The mechanical rules are deterministic.** The 34 automatic zeros in run 3 are identical in both versions.
- **The judgment calls move.**
  - **Run 2:** this time the scorer treated 11 NA answers as contradictions with other answers and scored them 0. In 1.1.0 it had noted the same conflicts but kept them at 1.
  - **Run 4:** the number of 0s fell from 94 to 27. The two runs read differently which NA justifications and "Yes" contradictions deserved a 0.
  - **Run 3:** fewer 3s and 4s overall. Two domains moved from PASS to FAIL: Business Continuity (56.6% → 48.7%) and Cryptography (55.4% → 45.7%). Human Resources fell from 65.0% to exactly 50.0%, which still counts as a pass.
- **The way the work was split up also differed.** Run 3 used 5 parallel subagents in both versions. Run 4 used one session in 1.1.0 but 6 subagents in 1.1.1, which also explains why its cost rose from $3.18 to $11.87.

Two runs per input aren't enough to measure the spread properly. Even so, an overall score can move by about 5–7 points, and domain PASS/FAIL near 50% isn't stable from run to run. If that matters, the next step is to score one questionnaire 3–5 times on the same version and measure the spread.

## New issues found

1. **Shared temp folder.** Runs 2 and 4 both used `/tmp/caiqwork` at the same time. Run 2 noticed and moved to a private folder, but the risk is real when two scorings run at once. SKILL.md should say to create a *unique* temp directory, for example with `mktemp -d`.
2. **Cleanup when the skill stops early.** Run 1 stopped at the version confirmation and left its temp folder (`/tmp/caiqscore.*`) behind. A helper also left `/tmp/caiqwork_s1`. Both were deleted by hand after the test. Cleanup should also happen when the skill stops to ask a question.
3. **Run-to-run score variation.** See above. This is a methodology and reproducibility question, not something a packaging fix addresses.
4. **Cost and approach still vary**, as in 1.1.0: $0.24 to $11.87 per run, with or without subagents. A scorer at this cost should probably say how to split the row scoring.

Still open from 1.1.0, and deliberately left for later: the AI-CAIQ ownership requirement rules. Run 4 again had to interpret them, and said so.
