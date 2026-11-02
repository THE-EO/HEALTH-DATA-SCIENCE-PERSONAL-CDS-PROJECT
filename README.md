# Know Your Numbers: Blood Pressure and Blood Glucose Screening Analysis

A reproducible descriptive and exploratory analysis of a 50-participant community blood pressure and blood glucose screening held in Asaba, Delta State, Nigeria, on 12 to 15 September 2026 at three locations (Echolab, DDPA Estate and Lifestream). Built as an NYSC Community Development Service (CDS) health project and a data science portfolio piece.

> **Educational project. Not medical advice, not a diagnostic tool, not clinically validated, and not a prevalence study.** Nobody is diagnosed with hypertension or diabetes anywhere in this work.

## Questions

1. How good are the screening records, and what had to be corrected or flagged?
2. Who was screened, and how are glucose and blood pressure distributed?
3. How uncertain are the descriptive numbers?
4. Do readings differ by sex or location, does age explain any difference, and how reliable are such comparisons?
5. What can and cannot be concluded from this dataset?

## What the notebook does

`HEALTH_DATA_SCIENCE_CODE.ipynb` has two parts in one notebook.

**Part 1: data preparation and exploration**
- Inspects the raw records before changing anything; works on a copy
- Cleans and documents every step: renames columns, flags and converts the three "Ad" (adult, no exact age) entries to missing ages, derives pulse pressure and age groups
- No participant is deleted; unusual values are flagged by the IQR rule and a glucose threshold, then kept
- Univariate, bivariate and multivariate exploration (23 figures), Spearman correlation matrix

**Part 2: statistics**
- Study design and confounding check, extended descriptive statistics, Shapiro-Wilk tests and Q-Q plots
- Bootstrap confidence intervals for medians, t intervals for means, Wilson intervals for percentages
- Mann-Whitney, Welch t, Kruskal-Wallis and Fisher exact tests; Cohen's d; Spearman correlations with bootstrap intervals
- Holm correction across 13 declared main tests; all other tests labelled supplementary and uncorrected
- Sample-size sensitivity calculation for a two-sample t-test
- Linear regression adjusting for age, with least squares written out in linear algebra (explanatory, not predictive)
- Blood pressure bands (2017 ACC/AHA) and numeric glucose bands, used as descriptive labels only

## Main finding (exploratory)

| Result | Value |
|---|---|
| Median systolic BP, women vs men | 114 vs 126 mmHg (difference 12, bootstrap 95% CI 2 to 25) |
| Mann-Whitney p / Holm-adjusted p | 0.002 / 0.027 |
| Age-adjusted difference (47 with known age) | 13.6 mmHg (95% CI 5.1 to 22.2) |
| Without the highest reading (46 people) | about 10.5 mmHg (95% CI 3.3 to 17.8) |

In these 50 participants, recorded systolic blood pressure was higher in men than in women, and the difference stayed in the same direction after adjusting for age and removing the highest reading. Given the small convenience sample, this is an **exploratory association**, not evidence that sex causes higher blood pressure and not a statement about the wider community.

Differences in glucose or diastolic pressure and differences between locations were **not detected**. With 50 people that does not mean they are absent: a difference of about 0.8 standard deviations would be needed to be detected 80% of the time.

## Limitations

- Convenience sample of people who chose to attend; no statement about the community is possible.
- **Repeat readings:** when a first reading was elevated, some participants were measured again and a later reading judged more suitable was kept. Original and intermediate readings were not consistently retained, so the data hold one value per person but not one measurement per person, and retained values may lean towards the expected range.
- Devices, measurement protocol and glucose test type (fasting or random) were not recorded, so bands are numeric groupings and not diagnoses.
- Age differs between locations and three ages are missing, so location comparisons are unreliable.
- No weight, height, medication or medical history.
- Pseudonymisation (names replaced by codes) is not full anonymisation.

## Data and privacy

The real participant data are **not in this repository and will not be shared**. The `.gitignore` blocks the raw file and prepared participant-level files. Everything here can be run on invented data instead.

The Nigeria Data Protection Act 2023 applies to this kind of data. Nothing in this project claims full legal compliance.

## Repository contents

| Path | What it is |
|---|---|
| `HEALTH_DATA_SCIENCE_CODE.ipynb` | Part 1 and Part 2 (outputs cleared) |
| `make_synthetic_data.py` | Creates invented data in the same layout as the private file |
| `docs/data_dictionary.csv` | Description of every variable (no participant data) |
| `LICENSE` | MIT License for the code |

## How to run it

1. Install Python 3.10 or later, or Anaconda, which includes everything needed.
2. Install the packages: `pip install -r requirements.txt`
3. Create the invented dataset: `python make_synthetic_data.py` (writes `outputs/synthetic_demo_data.csv`)
4. Open the notebook in Jupyter. In the loading cell, change the file name to `outputs/synthetic_demo_data.csv`.
5. Choose Kernel, then Restart & Run All.

**Results from the synthetic file mean nothing.** It exists only so the code can run. The written interpretations in the notebook describe the real data, so with synthetic data the sentences will not match the tables.

## Tools

Python, pandas, numpy, scipy, matplotlib, seaborn.

## Author

Ozemoya Eunice Oshioreamhe. Contact: [oshioreamheeunice@gmail.com]

## License

Code is released under the MIT License (see `LICENSE`). The real data are not covered by it and are not distributed.

## References

- Whelton PK, et al. 2017 ACC/AHA guideline for the prevention, detection, evaluation, and management of high blood pressure in adults. Hypertension. 2018;71(6):e13-e115.
