# ECON-UB-232-Data-Bootcamp-EDA-Case-Study
> A comprehensive EDA case study on loan default risk analytics in banking and financial services, identifying key drivers behind client payment difficulties using Python and data visualization.

---

## 📌 Project Overview

This project applies **Exploratory Data Analysis (EDA)** to a real-world loan application dataset. The goal is to understand patterns that indicate whether a client is likely to default on their loan, enabling a consumer finance company to make smarter, data-backed lending decisions.

The analysis covers:
- Data quality assessment and missing value treatment
- Class imbalance analysis of the target variable
- Univariate and bivariate analysis of categorical and numerical features
- Correlation analysis to identify the strongest predictors of default
- Integration of historical application behavior into risk profiling

---

## 🗂️ Repository Structure

```
├── EDA_Loan_Default_CaseStudy.ipynb   # Main Jupyter Notebook (full EDA)
├── EDA_Loan_Default.pptx              # Academic presentation
├── README.md
├── requirements.txt                         
└── data/                              # Place your data files here (see below)
    ├── application_data.csv
    ├── previous_application.csv
    └── columns_description.xlsx
```

---

## 📁 Dataset

This project uses the **Home Credit Default Risk** dataset with 3 files:

| File | Description |
|------|-------------|
| `application_data.csv` | Client information at the time of loan application (307,511 records, 122 features) |
| `previous_application.csv` | Historical loan application records per client |
| `columns_description.xlsx` | Data dictionary describing all variables |

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/ht2604/ECON-UB-232-Data-Bootcamp-EDA-Case-Study.git
cd ECON-UB-232-Data-Bootcamp-EDA-Case-Study
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Add the data files

Place `application_data.csv` and `previous_application.csv` in the `data/` folder (or update the file paths inside the notebook).

### 4. Run the notebook

```bash
jupyter notebook EDA_Loan_Default_CaseStudy.ipynb
```

Or open it directly in **Google Colab** by uploading to your Drive and mounting it.

---

## 🔍 Key Findings

| Finding | Detail |
|---------|--------|
| **Class Imbalance** | Only **8.07%** of clients defaulted — dataset is severely imbalanced |
| **Strongest Predictors** | `EXT_SOURCE_1`, `EXT_SOURCE_2`, `EXT_SOURCE_3` (external credit scores) show the highest negative correlation with default |
| **Age Effect** | Older applicants default significantly less; clients under 30 are highest risk |
| **Education** | Clients with secondary education default more than university graduates |
| **Gender** | Male clients have a slightly higher default rate than female clients |
| **Prior Refusals** | Clients with 5+ previous refusals default at **14.9%** vs. **7%** for those with none |
| **Data Quality** | `DAYS_EMPLOYED` contains 365,243 placeholder values (≈1,000 years) that must be treated as NaN |

---

## 📈 Analysis Sections

The notebook is organized into the following sections:

1. **Environment Setup** — Library imports and display settings
2. **Data Loading** — Reading `application_data.csv` and `previous_application.csv`
3. **Basic EDA** — Shape, data types, statistical summary
4. **Missing Value Analysis** — 67 columns have missing data; top columns exceed 69% missing
5. **Target Variable Analysis** — Class imbalance visualization
6. **Univariate Analysis** — Distribution of categorical and numerical variables
7. **Bivariate Analysis** — Feature comparisons split by default status
8. **Correlation Analysis** — Heatmap and top correlated features
9. **Previous Application Merge** — Aggregating historical data per client
10. **Conclusions** — Summary of findings and business recommendations

---

## 💡 Business Recommendations

Based on the EDA findings:

- **Risk-based pricing**: Apply higher interest rates to high-risk applicants (young, low-education, multiple prior refusals)
- **Credit limit adjustment**: Reduce approved amounts for borderline applicants
- **Feature prioritization**: Use `EXT_SOURCE_1/2/3` as primary features in any downstream predictive model
- **Imbalance handling**: Apply **SMOTE** oversampling or `class_weight='balanced'` when training classifiers
