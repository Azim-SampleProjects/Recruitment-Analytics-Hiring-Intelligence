# Recruitment Analytics & Hiring Intelligence

## Business Questions and Statistical Analysis Plan

**Project objective:** Develop a focused recruitment intelligence analysis that explains recruitment funnel performance, sourcing effectiveness, job-category differences, recruitment efficiency, candidate experience and offer acceptance. The deliverables are six business questions, three inferential investigations and one executive dashboard.

**Data source:** Synthetic recruitment dataset with six related tables: `companies` (320), `jobs` (2,000), `candidates` (9,000), `applications` (15,000), `interviews` (7,196) and `offers` (938). Record totals should not be interpreted as unique candidate counts.

## Core business questions

| # | Business question                                                                                                                                                                                 | Metric / analytical approach                                    | Visualization                              |
| - | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- | ------------------------------------------ |
| 1 | **Recruitment funnel effectiveness:** Where do applications drop off between application, interview, offer, and acceptance?                                                                 | Count distinct applications at each milestone; conversion rates | Funnel or horizontal bar chart             |
| 2 | **Recruitment source effectiveness:** Which sources generate higher offer and acceptance rates?                                                                                             | Applications and conversions by source                          | Bar chart of source-level conversion rates |
| 3 | **Hiring difficulty by job type:** Which role families or seniority levels show lower offer conversion?                                                                                     | Offer rate by role family and job level                         | Grouped bar chart or heatmap               |
| 4 | **Recruitment process efficiency:** How long does it take from application to first scheduled interview and to offer?                                                                       | Median days and distribution by job group                       | Histograms, grouped duration chart, comparative boxplots        |
| 5 | **Candidate experience and selection effectiveness:** Do applicants receiving offers differ in years of experience from other applicants, and how do experience profiles vary across job seniority levels? | Experience by offer outcome and job seniority                   | Boxplots and median-experience heatmap             |
| 6 | **Salary competitiveness and acceptance:** Is offer acceptance associated with an offer's position inside the advertised salary range?                                                      | Acceptance rates by relative salary position and role family   | Binned bar chart and role-family heatmap           |

## Inferential statistical investigations

| Hypothesis | Research question                                                                 | Candidate method                                             | Business purpose      |
| ---------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------ | --------------------- |
| H1         | Offer rates vary across recruitment sources.                                      | Chi-square test; Cramer's V                                  | Sourcing strategy     |
| H2         | Years of experience differ between applicants receiving and not receiving offers. | Welch's t-test or Mann-Whitney U, depending on distributions | Candidate screening   |
| H3         | Offer acceptance is associated with salary position within the posted range.      | Logistic regression with appropriately chosen covariates     | Offer competitiveness |

### Offer salary position

`salary_position = (base_salary - salary_low) / (salary_high - salary_low)`

A value of 0 is the advertised minimum and 1 is the maximum. Verify salary-range widths and out-of-range observations before analysis.

## Data quality and interpretation safeguards

- Preserve all original records and dates; **217 accepted offers have acceptance dates preceding offer dates** and remain flagged as chronology exceptions.
- Separately, **29 offers have a first scheduled interview after the offer date**. Keep the records, but exclude negative interview-to-offer durations only from that duration summary (909 remaining).
- Do not use negative acceptance durations for observed offer response-time metrics; report the valid sample size and the limitation this introduces.
- The dataset shows a systematic shared time-of-day pattern and whole-day offsets across related recruitment timestamps. Interpret timeline results as synthetic demonstration metrics rather than observed real-world behaviour.
- Count **distinct applications** for funnel conversion rates; several interview rows may belong to one application.
- Preserve the original application `source` values; `outbound` and `recruiter` are ambiguous, so the reporting labels are descriptive rather than verified definitions.
- Candidate total years of experience are not the same as job-relevant experience or quality of hire.
- Relative salary position within a posted range is not an external market-competitiveness benchmark.
- Do not equate an issued or accepted offer with a confirmed hire.
- Make no causal claims from observational relationships. Inferential findings are about synthetic data and may depend on company and job characteristics.
- Use effect sizes and confidence intervals alongside statistical significance.

## Planned technical workflow

1. Reload the six unchanged source CSV files in `Analysis.ipynb`.
2. Convert date strings to datetime and recreate the documented analytical validation fields without changing originals.
3. Define distinct-application recruitment funnel metrics and answer the six questions.
4. Conduct three targeted statistical investigations in `Statistics.ipynb`, verifying assumptions and potential confounding; repeated applications by the same candidate warrant attention.
5. Create one concise executive dashboard with findings and limitations.
6. Document methodology, results, assumptions and recommendations in the GitHub README.

**Project scope:** Six questions, three hypothesis investigations and one executive dashboard. Machine learning is optional, not required.
