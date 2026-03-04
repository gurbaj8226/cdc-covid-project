# CDC COVID-19 Severity Analysis (Python / Pandas)

This project analyzes case severity patterns in the **CDC COVID-19 Case Surveillance Public Use Dataset** using Python and pandas.

The goal is to explore how **hospitalization, ICU admission, and death outcomes vary across demographics and time.**

---

# Dataset

Source:  
CDC COVID-19 Case Surveillance Public Use Data  
https://data.cdc.gov

Dataset ID:
```
vbim-akqf
```

The full dataset contains **~106 million rows**, which is too large for local analysis on most machines.

Instead, a **1,000,000 row sample** was retrieved using the **Socrata API** and stored locally as a **Parquet file** for efficient processing.

---

# Columns Used

The analysis focuses on the following variables:

| Column | Description |
|------|-------------|
| `cdc_report_dt` | Date the case was reported to the CDC |
| `age_group` | Age category of the case |
| `sex` | Reported sex |
| `race_ethnicity_combined` | Combined race/ethnicity classification |
| `hosp_yn` | Whether the case was hospitalized |
| `icu_yn` | Whether the case required ICU admission |
| `death_yn` | Whether the case resulted in death |
| `medcond_yn` | Presence of underlying medical conditions |
| `current_status` | Case classification (confirmed or probable) |

Each row represents a **de-identified patient case**.

---

# Data Cleaning

The following preprocessing steps were applied:

1. Converted `cdc_report_dt` from string to datetime
2. Created a monthly aggregation variable `case_month`
3. Filtered the dataset to include only
   - Laboratory-confirmed cases
   - Probable cases
4. Preserved `"Missing"` and `"Unknown"` outcome values for transparency
5. Created filtered subsets where necessary for **known-only denominators**

Example cleaning code:

```python
df_clean = df.copy()

df_clean["cdc_report_dt"] = pd.to_datetime(
    df_clean["cdc_report_dt"], errors="coerce"
)

df_clean["case_month"] = (
    df_clean["cdc_report_dt"]
    .dt.to_period("M")
    .astype(str)
)

df_clean = df_clean[
    df_clean["current_status"].isin(
        ["Laboratory-confirmed case", "Probable Case"]
    )
]
```

---

# Research Questions

This project answers four analytical questions:

### 1️ Hospitalization Rates by Age Group
How hospitalization risk changes across age categories.

Two denominators are reported:
- Known hospitalization outcomes only
- All cases including missing/unknown values

---

### 2️ Death Rates by Underlying Conditions
Compare fatality rates for cases with and without recorded medical conditions.

---

### 3️ ICU Admission Patterns by Demographics
Analyze ICU admission differences across:
- Sex
- Race / Ethnicity

---

### 4️ Case Severity Trends Over Time
Track monthly changes in:
- Hospitalization rates
- ICU admissions
- Death rates

---

# Key Limitation

Many outcome fields contain large shares of `"Missing"` or `"Unknown"` values.

For example, the underlying medical condition variable in the sample dataset contains:

- Missing: ~80%
- Unknown: ~9%

This requires careful interpretation of denominator definitions in rate calculations.

---

# Project Structure

```
cdc-covid-project
│
├── data/
│   └── processed/
│       └── cdc_sample_1M.parquet
│
├── notebooks/
│   └── 01_data_acquisition_cleaning_q1.ipynb
│
├── outputs/
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

# Tools Used

- Python
- pandas
- matplotlib
- Jupyter Notebook
- Socrata Open Data API

---

# Future Improvements

- Analyze full dataset using cloud compute
- Stratify outcomes by vaccination period
- Build interactive dashboards for severity metrics