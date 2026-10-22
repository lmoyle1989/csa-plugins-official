# caiq-scorer plugin: test results

Test evidence for PR #22 (`add-caiq-scorer`). This branch is for review only, and none of it is part of the plugin release.

**Short answer: it works.** The plugin installed from GitHub and loaded as `caiq-scorer:caiq-scorer`. It ran end to end on three real questionnaires (CAIQ v4.0.2, CAIQ v4.1.0 and AI-CAIQ v1.0.2) and produced a valid scored workbook and PDF report for each. With no hints, it stopped and asked about the CAIQ 4.0.2 version mismatch, which is the known issue noted in the PR.

## Test inputs

The three inputs are real, publicly posted CAIQs downloaded from the CSA STAR Registry. They were anonymized before testing, and the inputs and scored outputs are not included here.

- Company and product names were replaced with `[Vendor]`.
- Vendor URLs were replaced with `[vendor URL]`.
- Cell comments and document author metadata were removed.

| Input | Questionnaire | Framework | Answer profile |
|---|---|---|---|
| A | CAIQ v4.0.2 | CCM v4.0 (scored with CCM 4.0.13 reference data) | 261 questions, identical boilerplate in every description |
| B | CAIQ v4.1.0 | CCM 4.1.0 (JSON metadata in A1) | 283 questions, substantive answers |
| C | AI-CAIQ v1.0.2 | AICM 1.0.2 (JSON metadata in A1) | 311 questions, substantive answers |

## Environment

- macOS, Claude Code 2.1.308, model `claude-opus-5-5`
- Python 3 with `openpyxl` 3.1.5 and `reportlab` 4.4.10 already installed
- The standalone copy of the skill in `~/.claude/skills/` was moved aside during the test, so only the plugin version could load.
- Unrelated plugins were disabled for the test sessions.

## 1. Install

The install used the same path real users will take, pulling the marketplace from GitHub on the PR branch. The full output is in [`install.log`](install.log).

```
claude plugin marketplace add lmoyle1989/csa-plugins-official#add-caiq-scorer   ✔ added marketplace csa-plugins-official
claude plugin install caiq-scorer@csa-plugins-official                          ✔ installed (scope: user)
claude plugin details caiq-scorer                                               caiq-scorer 1.1.0, Skills (1) caiq-scorer
```

- The plugin adds about 200 tokens to every session, and about 5.3k each time the skill runs.
- Uninstalling and removing the marketplace afterwards were also clean ([`uninstall.log`](uninstall.log)).

## 2. Scoring runs

Each run was a fresh headless session (`claude -p`) in an empty directory containing one input file, started with the plugin's slash command. Because a headless session can't answer questions, runs 2–4 put the version confirmation in the prompt, standing in for the user's reply. The exact prompts and each session's final response are in [`runs/`](runs/).

| Run | Input | Version given | Result | Overall | Time | Cost | Turns |
|---|---|---|---|---|---|---|---|
| 1 | A: CAIQ v4.0.2 | none | Stopped at version check (expected) | — | 26 s | $0.16 | 4 |
| 2 | A: CAIQ v4.0.2 | CCM 4.0.13 | ✔ workbook + 12-page PDF | 1.00/4 (25.0%) FAIL | 4.2 min | $1.04 | 23 |
| 3 | B: CAIQ v4.1.0 | CCM 4.1.0 | ✔ workbook + 9-page PDF | 2.22/4 (55.5%) PASS | 9.8 min | $7.11 | 16 (+5 subagents) |
| 4 | C: AI-CAIQ v1.0.2 | AICM 1.0.2 | ✔ workbook + 8-page PDF | 0.95/4 (23.8%) FAIL | 12.9 min | $3.18 | 38 |

### Output checks (runs 2–4)

All passed:

- The workbook headers exactly match the column list in SKILL.md step 8, for both CAIQ and AI-CAIQ mode.
- An `About` sheet is present, giving the package version, prompt set, mode, confirmed framework version, date and disclaimer.
- Every score is an integer from 0 to 4.
- No "No" answer scores above 2.
- Every PDF page has the `CSA Valid-AI-ted Scorer v1.1.0` footer.
- The disclaimer is on the final page.
- No vendor names appear anywhere in the output.

### Score distributions

| Run | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| 2 (CAIQ 4.0.2) | 0 | 261 | 0 | 0 | 0 |
| 3 (CAIQ 4.1.0) | 34 | 5 | 117 | 119 | 8 |
| 4 (AI-CAIQ 1.0.2) | 94 | 150 | 55 | 12 | 0 |

### Per-domain results

<details><summary>Run 2: CAIQ v4.0.2 / CCM 4.0.13. Every domain scored 25.0% (all rows 1/4).</summary>

All 17 domains averaged 1.00 (25.0%) and every row scored 1. Every answer is the same one-sentence referral to an external trust portal, so the scorer had no implementation detail to tell rows or domains apart.
</details>

<details><summary>Run 3: CAIQ v4.1.0 / CCM 4.1.0</summary>

| Domain | Qs | Pass | Fail | Avg | % |
|---|---|---|---|---|---|
| Audit & Assurance | 8 | 8 | 0 | 2.62 | 65.6% |
| Application & Interface Security | 13 | 13 | 0 | 2.62 | 65.4% |
| Business Continuity Mgmt & Operational Resilience | 19 | 17 | 2 | 2.26 | 56.6% |
| Change Control & Configuration Mgmt | 12 | 11 | 1 | 2.50 | 62.5% |
| Cryptography, Encryption & Key Mgmt | 23 | 20 | 3 | 2.22 | 55.4% |
| Datacenter Security | 28 | 7 | 21 | 0.79 | 19.6% |
| Data Security & Privacy Lifecycle Mgmt | 24 | 21 | 3 | 2.33 | 58.3% |
| Governance, Risk & Compliance | 10 | 10 | 0 | 2.50 | 62.5% |
| Human Resources | 20 | 20 | 0 | 2.60 | 65.0% |
| Identity & Access Mgmt | 19 | 19 | 0 | 2.58 | 64.5% |
| Interoperability & Portability | 8 | 8 | 0 | 2.50 | 62.5% |
| Infrastructure Security | 15 | 15 | 0 | 2.60 | 65.0% |
| Logging & Monitoring | 19 | 15 | 4 | 1.95 | 48.7% |
| Security Incident Mgmt, E-Discovery & Cloud Forensics | 16 | 16 | 0 | 2.75 | 68.8% |
| Supply Chain Mgmt, Transparency & Accountability | 19 | 14 | 5 | 1.74 | 43.4% |
| Threat & Vulnerability Mgmt | 15 | 15 | 0 | 2.40 | 60.0% |
| Universal Endpoint Mgmt | 15 | 15 | 0 | 2.40 | 60.0% |

The three failing domains are driven by the mechanical rules. 34 rows declared shared or third-party ownership but left the customer-side column empty, so each was auto-scored 0. The rows that were judged on their content average 2.52 (63.1%).
</details>

<details><summary>Run 4: AI-CAIQ v1.0.2 / AICM 1.0.2</summary>

| Domain | Qs | Pass | Fail | Avg | % |
|---|---|---|---|---|---|
| Audit & Assurance | 7 | 3 | 4 | 1.14 | 28.6% |
| Application & Interface Security | 18 | 8 | 10 | 1.22 | 30.6% |
| Business Continuity Mgmt & Operational Resilience | 16 | 0 | 16 | 0.88 | 21.9% |
| Change Control & Configuration Mgmt | 11 | 4 | 7 | 1.09 | 27.3% |
| Cryptography, Encryption & Key Mgmt | 23 | 1 | 22 | 0.17 | 4.3% |
| Datacenter Security | 24 | 0 | 24 | 1.00 | 25.0% |
| Data Security & Privacy Lifecycle Mgmt | 30 | 12 | 18 | 1.23 | 30.8% |
| Governance, Risk & Compliance | 18 | 6 | 12 | 1.28 | 31.9% |
| Human Resources | 22 | 0 | 22 | 1.00 | 25.0% |
| Identity & Access Mgmt | 23 | 8 | 15 | 1.17 | 29.3% |
| Interoperability & Portability | 5 | 0 | 5 | 0.80 | 20.0% |
| Infrastructure Security | 12 | 3 | 9 | 0.58 | 14.6% |
| Logging & Monitoring | 20 | 7 | 13 | 0.95 | 23.8% |
| Model Security | 19 | 11 | 8 | 1.53 | 38.2% |
| Security Incident Mgmt, E-Discovery & Cloud Forensics | 13 | 2 | 11 | 0.77 | 19.2% |
| Supply Chain Mgmt, Transparency & Accountability | 18 | 1 | 17 | 0.83 | 20.8% |
| Threat & Vulnerability Mgmt | 17 | 1 | 16 | 0.24 | 5.9% |
| Universal Endpoint Mgmt | 15 | 0 | 15 | 1.00 | 25.0% |

Three things drove the low score:
- **24 contradictions.** "Yes" answers whose explanation says the control isn't implemented were each scored 0.
- **127 generic NA answers.** They all share one sentence with ownership "Not Determined". 37 were scored 0 where the stated reason didn't fit the control.
- **Answers numbered to the wrong version.** 44 rows appear to have been answered against AICM 1.1.0 control numbering, although the questionnaire is 1.0.2. The scorer spotted this itself. It's the same 1.0.x/1.1.x ID drift the SKILL.md warns about.
</details>

## What the tests confirm works

- The plugin installs from a GitHub marketplace, is namespaced correctly, and loads only from the plugin cache.
- The scorer detects the mode (CAIQ vs AI-CAIQ) from the columns, and the version from the A1 JSON or the sheet header.
- **Mechanical pre-scoring** auto-scored 0 the shared-ownership rows that had no customer description (run 3).
- **The "No" cap** held: no "No" answer scored above 2 in any run.
- **Contradictions** (a "Yes" whose explanation says the control isn't implemented) were scored 0 (run 4).
- The workbook layout, `About` sheet, PDF layout, footer and disclaimer all match the SKILL.md spec in both modes.

## Issues found

None of these blocked a run. All of them are in the skill itself, not in how it's packaged as a plugin.

1. **CAIQ vs CCM versioning.** With no hint, the CAIQ v4.0.2 file is refused (run 1). The scorer did find that all 197 controls match CCM 4.0.13 exactly, but it still asks the user to choose. It needs a CAIQ→CCM version mapping.
2. **Footer rows counted as questions.** In run 1 the scorer counted the "End of Standard" and copyright rows as 2 blank questions (263 instead of 261). Runs 2–4 left them out on their own initiative. SKILL.md should say to skip them.
3. **Output location and naming aren't specified.** File names varied between runs (`-scored.xlsx` vs `_scored.xlsx`). Run 3 left a `_work/` folder, and one of its subagents wrote scratch scripts outside the working directory.
4. **AI-CAIQ ownership rules are ambiguous.** The rules for which descriptions are required are written only for CAIQ ownership values. In run 4 the scorer had to decide for itself how to apply them to AI-CAIQ values such as `Shared CSP-MP` and `Not Determined`, and it said so.
5. **Execution strategy and cost vary.** Run 3 fanned out to 5 parallel subagents ($7.11), while run 4 scored everything in one session ($3.18) on a slightly larger questionnaire. SKILL.md doesn't say how to handle 250–300 rows of judgment scoring.
6. **It needs non-stdlib Python libraries.** `openpyxl` and `reportlab` were already installed here. On a machine without them, the outcome is untested.

## Caveat: the anonymization affected scoring

The `[Vendor]` / `[vendor URL]` substitutions are not in the real questionnaires. In runs 2 and 3 the scorer noticed them and listed "unreplaced placeholders" as a weakness in its feedback. Scores are unaffected in run 2, where every row is identical, and the effect is probably small in run 3. Still, the run 3 and 4 numbers should be read as a test of the pipeline, not as a definitive score for those questionnaires.
