# CDC COVID-19 Severe Outcomes Analysis

A Python data analysis project exploring how **demographics and underlying medical conditions relate to severe COVID-19 outcomes** using the CDC Case Surveillance Public Use dataset.

This project demonstrates a complete **data analytics workflow** including:

* API data acquisition
* data cleaning and preprocessing
* exploratory analysis
* severity rate calculations
* data visualization
* interpretation of results

The analysis was performed using **Python, Pandas, and Matplotlib** within Jupyter notebooks.

---

# Executive Summary

Using a **1 million case sample** from the CDC COVID-19 Case Surveillance dataset, this project analyzes how severe COVID outcomes vary across age groups and underlying medical conditions.

The analysis focuses on three key outcomes:

* **Hospitalization**
* **ICU admission**
* **Death**

Key findings from the dataset sample:

* Cases with **reported underlying medical conditions** show substantially higher rates of hospitalization, ICU admission, and death.
* Severe outcome rates increase across **older age groups within the available sample**.
* Properly handling **missing outcome values** is essential; including unknown outcomes in denominators can significantly underestimate severity rates.

The project demonstrates how real-world public health datasets can be analyzed using reproducible Python workflows.

---

# Example Visualization

Severe Outcomes by Medical Condition

![Severity by Medical Condition](outputs/severity_by_medical_condition.png)

This chart compares hospitalization, ICU admission, and death rates for cases with and without underlying medical conditions.

Additional charts generated in this project include:

* hospitalization rate by age group
* ICU admission rate by age group
* death rate by age group
* severe outcome rates by underlying medical conditions

All figures are generated using **Matplotlib** and exported to the `outputs` directory.

---

# Dataset

Source:

CDC COVID-19 Case Surveillance Public Use Data
[https://data.cdc.gov/Case-Surveillance/COVID-19-Case-Surveillance-Public-Use-Data/vbim-akqf](https://data.cdc.gov/Case-Surveillance/COVID-19-Case-Surveillance-Public-Use-Data/vbim-akqf)

The full dataset contains **tens of millions of case records**.

To make the analysis manageable locally, this project retrieves a **1 million row sample** using the **Socrata Open Data API**.

The cleaned dataset is stored locally as:

```
data/processed/cdc_clean_1M.parquet
```

Large raw datasets are intentionally **not stored in the repository**, but the pipeline for retrieving and cleaning the data is included in the notebooks.

---

# Project Structure

```
cdc-covid-project
│
├── data
│   ├── raw
│   │   └── .gitkeep
│   └── processed
│       └── .gitkeep
│
├── notebooks
│   ├── 01_data_acquisition_cleaning_q1.ipynb
│   ├── 02_hospitalization_analysis.ipynb
│   ├── 03_icu_analysis.ipynb
│   ├── 04_death_analysis.ipynb
│   └── 05_risk_factor_analysis.ipynb
│
├── outputs
│   ├── hospitalization_by_age.csv
│   ├── hospitalization_by_age.png
│   ├── icu_by_age.csv
│   ├── icu_by_age.png
│   ├── death_by_age.csv
│   ├── death_by_age.png
│   ├── hospitalization_by_medcond.csv
│   ├── icu_by_medcond.csv
│   └── death_by_medcond.csv
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

# Analysis Workflow

The project follows a structured analytics pipeline.

## 1 Data Acquisition

Data is downloaded from the CDC Socrata API using Python.

Pagination is used to retrieve a **1 million record sample** of the dataset.

---

## 2 Data Cleaning

Cleaning steps include:

* converting report dates to datetime
* creating a `case_month` variable
* standardizing age group labels
* filtering valid case statuses
* removing invalid outcome values
* preparing the dataset for downstream analysis

The cleaned dataset is saved as a **Parquet file** for efficient analysis.

---

## 3 Outcome Analysis

Three notebooks analyze each severe outcome separately.

| Notebook | Analysis                          |
| -------- | --------------------------------- |
| 02       | Hospitalization rate by age group |
| 03       | ICU admission rate by age group   |
| 04       | Death rate by age group           |

Each analysis:

1. filters rows with **known outcome values**
2. groups cases by **age group**
3. calculates severity rates
4. exports results as CSV tables
5. generates visualizations

---

## 4 Risk Factor Analysis

The final notebook evaluates how **underlying medical conditions affect severe outcomes**.

The analysis compares hospitalization, ICU admission, and death rates between cases with and without reported medical conditions.

This section acts as the **capstone analysis** of the project.

---

# Methodology

Severity rates are calculated using **only known outcome values**.

For example:

```
hospitalization_rate =
hospitalized_cases / cases_with_known_hospitalization_status
```

Rows containing **Missing** or **Unknown** outcome values are excluded from denominators to avoid underestimating severity rates.

---

# Limitations

The Socrata API returns rows in **database order**, not as a random sample.

As a result:

* the 1M row sample does not contain all possible age groups
* some demographic groups may be underrepresented

Therefore, the results should be interpreted as **exploratory analysis rather than population-level estimates**.

---

# Technologies Used

Python
Pandas
Requests
PyArrow
Matplotlib
Jupyter Notebook
Git / GitHub

---

# Skills Demonstrated

* API data retrieval using the **Socrata Open Data API**
* Data cleaning and preprocessing using **Pandas**
* Handling missing values and defining **analytical denominators**
* Grouped aggregations and severity rate calculations
* Data visualization using **Matplotlib**
* Efficient dataset storage using **Parquet**
* Reproducible analytics workflows using **Jupyter and Git**

---

# How to Run the Project

Clone the repository:

```
git clone https://github.com/gurbaj8226/cdc-covid-project.git
```

Navigate to the project folder:

```
cd cdc-covid-project
```

Install dependencies:

```
pip install -r requirements.txt
```

Run notebooks in order:

```
01_data_acquisition_cleaning_q1.ipynb
02_hospitalization_analysis.ipynb
03_icu_analysis.ipynb
04_death_analysis.ipynb
05_risk_factor_analysis.ipynb
```

---

# Author

Gurbaj Singh
Computer Science Student

GitHub
[https://github.com/gurbaj8226](https://github.com/gurbaj8226)

---
