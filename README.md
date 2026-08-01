# Loan Default Risk — An EDA Case Study

> Exploratory analysis of 307,511 consumer loan applications and 1.67 million prior
> applications, identifying which client characteristics precede payment difficulty.

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://python.org)
[![pandas](https://img.shields.io/badge/pandas-2.2-150458?logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![Dataset](https://img.shields.io/badge/Data-Home%20Credit%20Default%20Risk-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/competitions/home-credit-default-risk/data)
![License](https://img.shields.io/badge/License-MIT-green)

**Author:** He (Helen) Tu
**Course:** ECON-UB 232 Data Bootcamp, NYU Stern — Spring 2026

---

## Overview

Consumer finance companies lose money two ways: by lending to clients who default,
and by refusing clients who would have repaid. This case study works the first
problem from the data side — given everything known at application time, which
client characteristics precede payment difficulty?

The dataset is the Home Credit Default Risk release: one row per application with
122 attributes, plus a second file recording every prior application the same
clients made.

## Dataset

| File | Rows | Columns |
| --- | --- | --- |
| `application_data.csv` | 307,511 | 122 |
| `previous_application.csv` | 1,670,214 | 37 |
| Merged (aggregated, left-joined) | 307,511 | 127 |

## Key Findings

### The target is severely imbalanced

| Outcome | Clients | Share |
| --- | --- | --- |
| Repaid on time | 282,686 | 91.93% |
| **Payment difficulty** | **24,825** | **8.07%** |

At **11.4 : 1**, this imbalance dominates every modelling decision downstream.
Accuracy is meaningless as a metric here — a model predicting "will repay" for
every applicant is already 91.93% accurate and completely useless.

### Why prior applications were refused

Of the refused prior applications, the recorded rejection reasons break down as:

| Reason code | Count | Share |
| --- | --- | --- |
| `HC` | 175,231 | 60.3% |
| `LIMIT` | 55,680 | 19.2% |
| `SCO` | 37,467 | 12.9% |
| `SCOFR` | 12,811 | 4.4% |
| `XNA` | 5,236 | 1.8% |
| `VERIF` | 3,535 | 1.2% |
| `SYSTEM` | 717 | 0.2% |
| `XAP` | 1 | <0.1% |

A single code, `HC`, accounts for **60% of all refusals** — meaning most rejection
history compresses into one largely opaque category, which limits how much signal
prior refusals can carry.

### Joining the two files correctly

The relationship between applications and prior applications is one-to-many, so a
direct merge on `SK_ID_CURR` inflates 307,511 rows to over a million through
fan-out. The notebook aggregates each client's prior applications first —
counting total, approved, refused and cancelled applications, and averaging prior
credit — and then left-joins, preserving the row count at 307,511 and adding five
engineered features.

This is documented explicitly in the notebook because it is the kind of mistake
that silently corrupts every downstream number if it goes unnoticed.

---

## Getting Started

### 1. Clone and install

```bash
git clone https://github.com/HelenTu05/ECON-UB-232-Data-Bootcamp-EDA-Case-Study.git
cd ECON-UB-232-Data-Bootcamp-EDA-Case-Study
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Get the data

Download from Kaggle into `data/` — see [data/README.md](data/README.md):

```bash
kaggle competitions download -c home-credit-default-risk -p data/ --unzip
```

### 3. Run

```bash
jupyter lab notebooks/01_loan_default_eda.ipynb
```

The notebook resolves `../data` by default and writes its plots to `figures/`.
Set `LOAN_DATA_DIR` to read from elsewhere, including a mounted Drive on Colab.

---

## Repository Structure

```
ECON-UB-232-Data-Bootcamp-EDA-Case-Study
├── notebooks/
│   └── 01_loan_default_eda.ipynb   # the full analysis
├── data/
│   ├── README.md                   # download instructions
│   └── columns_description.xlsx    # data dictionary for all 122 columns
├── figures/                        # populated on a full notebook run
├── reports/
│   └── presentation.pdf            # slide deck
├── requirements.txt
└── LICENSE
```

## Analysis Structure

1. **Data understanding** — shapes, dtypes, and a missing-value survey
2. **Target analysis** — class balance and its implications
3. **Univariate analysis** — categorical and numerical distributions
4. **Bivariate analysis** — income, age, employment, and phone-change recency
   against default status
5. **Correlation analysis** — feature relationships and the strongest correlates
   of `TARGET`
6. **Prior application analysis** — aggregation, joining, and rejection-reason
   breakdown
7. **Conclusions**

## A note on reproducibility

Several cells in the committed notebook were saved without their outputs, so a
few analyses — the missing-value table, the `TARGET` correlation ranking, and the
default-rate comparison for clients with prior refusals — render blank on GitHub
even though the code is present.

Rather than quote numbers that cannot be traced to a saved output, this README
reports only results that are visible in the notebook as committed. Re-running it
end-to-end against the Kaggle data will populate the remaining figures and let the
findings above be extended.

## Limitations & Future Work

- **`DAYS_EMPLOYED` needs a placeholder check.** The Home Credit data encodes
  "not employed" as 365243 days (~1000 years); this should be detected and
  treated as missing before the employment features are used.
- **No statistical tests.** Group differences are read off plots rather than
  tested; chi-square for categoricals and Mann-Whitney for continuous variables
  would put the comparisons on firmer ground.
- **The imbalance is untreated.** Any modelling built on this EDA needs
  resampling or class weighting, and should be evaluated on precision/recall or
  AUC rather than accuracy.
- **The analysis is descriptive.** No predictive model is fitted; that is the
  natural next step.

## License

MIT — see [LICENSE](LICENSE).
