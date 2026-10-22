I scored the CAIQ against CCM 4.1.0. Overall it averages **2.22 / 4 (55.5%), a PASS**, but three domains fail. The workbook and PDF are written, and I checked the workbook's structure and looked at the rendered PDF pages.

PASS here only means the written answers are complete and consistent. It does not show the controls are actually in place. A well-explained "No" scores 2 out of 4, exactly 50%, so a domain answered entirely "No" would still pass.

**Overall**
- 283 questions: 278 Yes, 3 No, 2 NA. The "End of Standard" and copyright rows at the bottom of the sheet aren't questions, so I left them out.
- Score spread: 8 scored 4, 119 scored 3, 117 scored 2, 5 scored 1, 34 scored 0.
- No answer contradicts its explanation, and all three "No" rows stay within the cap of 2.

**Why three domains fail**

The CSC Responsibilities column is empty on every row. That's acceptable for the 249 rows declared CSP-owned. The other 34 rows are declared shared or third-party, so the method requires a customer-side description and automatically scores each of them 0:

| Domain | Score | Rows scored 0 for a blank customer description |
|---|---|---|
| Datacenter Security | 19.6% | 21 of 28 |
| Supply Chain Management | 43.4% | 5 |
| Logging and Monitoring | 48.7% | 4 |

The 249 rows that were judged on their content average 2.52 / 4 (63.1%). Filling in the customer column for those 34 rows should lift all three failing domains above 50%.

The best-scoring domains are Security Incident Management (68.8%), Audit & Assurance (65.6%) and Application & Interface Security (65.4%).

**What keeps most other rows at 2**
- The same generic policy paragraph is reused across domains, including a review statement that says only that policies "may be updated if necessary".
- Many answers are just a pointer to another row, such as "See A&A-01.1".
- Some answers address a different question or copy another row: IAM-13.2, TVM-10.1 (a copy of TVM-09.1), DSP-14.1, CEK-05.1 and CEK-07.1.
- "[Vendor]" and "[vendor URL]" placeholders were never replaced.
- CEK-12.1 to CEK-15.1 mention "customer managed" keys, which looks like it conflicts with CEK-08.1 saying customers can't bring their own keys.

The questionnaire doesn't name the organization or the cloud service, so the report leaves those fields out. I split the 249 content-judged rows across five parallel scoring agents with identical instructions; each batch came back with roughly equal numbers of 2s and 3s.

One agent wrote its working scripts to a `score3_scripts/` folder one level above `plugintest/`, outside this directory. It isn't needed and can be deleted.

Files are in the current directory:
- `CAIQ-v4.1.0-sample_scored.xlsx`
- `CAIQ-v4.1.0-sample_scoring_report.pdf`
- `_work/` (intermediate data and build scripts)
