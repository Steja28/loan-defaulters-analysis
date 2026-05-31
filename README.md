> **BFSI Domain | Credit Risk Analytics | Tableau 2024.3 | 65,535 Loans Analyzed**

# Loan Defaulters Analysis

[![Tableau](https://img.shields.io/badge/Tableau-2024.3-blue?logo=tableau)](https://www.tableau.com/)
[![Data](https://img.shields.io/badge/Records-65%2C535-green)](./data/)
[![Workbook](https://img.shields.io/badge/Workbook-6%2C269%20lines%20XML-orange)](./Loan_Analysis.twb)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](./LICENSE)

A multi-dashboard Tableau workbook analyzing loan default behavior across borrower demographics, credit profiles, employment types, and loan characteristics — built to support credit risk decisions in the BFSI sector.

---

## 🔴 Live Dashboard

> **Tableau Public link coming soon.** To publish: open `Loan_Analysis.twb` in Tableau Desktop → Server > Tableau Public > Save to Tableau Public As...

---

## 📋 Business Problem Statement

**Domain:** Banking, Financial Services & Insurance (BFSI) — Retail Lending  
**Problem:** Loan defaults represent one of the most material risks on a bank's balance sheet. Identifying *which borrower profiles default* — and *why* — allows credit teams to price risk accurately, tighten underwriting for high-risk segments, and reduce non-performing asset (NPA) ratios.

**Business Questions This Analysis Answers:**
1. Which age groups and employment types carry the highest default concentration?
2. Does credit score reliably predict interest rate and default probability?
3. How does loan purpose interact with borrower credit quality at the point of default?
4. Do marital status and mortgage ownership signal financial obligation stress (DTI)?
5. Which borrower segments are over-lent (high loan amount relative to income)?

**Key Stakeholders:** Credit Risk Officers, Retail Lending Heads, Underwriting Teams, Risk Analytics CoE

**Dollar Stakes:** A 1% reduction in default rate on an ₹8.36B loan book = ₹83.6M in avoided NPA provisioning.

---

## 📊 Dashboard Screenshots

### 🗂️ Story Overview — The Loan Defaulters Analysis

**Story Point 1: Self-Employed & High Loan Segment (28–47 Age)**
![Loan Defaulters Story Overview](screenshots/01_loan_defaulters_story_overview.jpg)

**Story Point 2: Self-Employed Filter Applied**
![Story — Self-Employed Filter](screenshots/12_story_point2_self_employed_filter.jpg)

**Story Point 3: Credit Score 300 Filter (Highest Default Risk)**
![Story — Credit Score 300 Filter](screenshots/13_story_point3_credit_score_300_filter.jpg)

**Story Point 4: Credit Score 550 Filter (Mid-Range)**
![Story — Credit Score 550 Filter](screenshots/14_story_point4_credit_score_550_filter.jpg)

**Story Point 5: High School Education Filter**
![Story — High School Education Filter](screenshots/15_story_point5_highschool_education_filter.jpg)

**Story Point 6: Credit Score 850 Filter (Lowest Default Risk)**
![Story — Credit Score 850 Filter](screenshots/16_story_point6_credit_score_850_filter.jpg)

---

### 📈 Dashboard 1 — Loan Vs Income Analysis

![Loan Vs Income Analysis Dashboard](screenshots/10_loan_vs_income_analysis_dashboard.jpg)

> Combines loan distribution by age, employment type, income by education, and income by employment in a single filterable dashboard.

---

### 📉 Dashboard 2 — Defaults Vs Credit Score Analysis

![Defaults Vs Credit Score Dashboard](screenshots/11_defaults_vs_credit_score_dashboard.jpg)

> Shows interest rate distribution across the full credit score range (300–850), with defaults by purpose, loan term/marital status, and employment/age cross-sections.

---

### 📊 Individual Sheet Views

#### Loan Distribution by Age
![Loan Distribution by Age](screenshots/02_loan_distribution_by_age.jpg)
Ages 38–47 represent the highest loan volume at $1,644.4M. The 18–27 cohort shows significantly lower volume ($314.2M) due to limited credit history.

#### Interest Rates on Credit Scores
![Interest Rates on Credit Scores](screenshots/17_interest_rates_on_credit_scores.jpg)
Interest rates cluster tightly between 3.4–3.8% across most credit score bands (300–850), indicating limited risk-based pricing differentiation.

#### Income by Employment Type
![Income by Employment](screenshots/03_income_by_employment.jpg)
Income is nearly equally split across all four employment types (~25% each), suggesting employment type alone is not an income differentiator.

#### Income by Education Level
![Income By Education](screenshots/04_income_by_education.jpg)
All four education levels (Bachelor's, High School, Master's, PhD) show near-identical income totals (~$1.34–1.37B), indicating weak income correlation with education in this dataset.

#### DTI Ratio by Marital Status with Mortgage
![DTI Ratio by Marital Status](screenshots/05_dti_ratio_by_marital_status.jpg)
DTI ratios are uniformly distributed (~16.6–16.7%) across all marital status and mortgage combinations, showing no significant stress concentration.

#### Defaults by Employment Type and Age Group
![Defaults by Employment and Age](screenshots/06_defaults_by_employment_and_age.jpg)
The 28–37 age group shows the highest default rates across all employment types (6.3–8.9%). Part-time workers aged 28–37 have the highest rate at 8.54%.

#### Defaults by Loan Purpose and Credit Score
![Defaults by Purpose and Credit Score](screenshots/07_defaults_by_purpose_and_credit_score.jpg)
Defaulters have meaningfully lower average credit scores (~$194–234M range) versus non-defaulters (~$1,449–1,460M), consistent across all loan purposes.

#### Loan Distribution by Employment Type
![Loan Distribution by Employment](screenshots/08_loan_distribution_by_employment.jpg)
Self-employed borrowers receive the highest total loan amount ($2,124.4M), with all four employment types showing relatively balanced distribution.

#### Default by Loan Term and Marital Status
![Default By Loan Term and Marital Status](screenshots/09_default_by_loan_term_and_marital_status.jpg)
Defaulters show shorter average loan terms (~30–35 months) versus non-defaulters (~35–37 months). Married borrowers have the highest non-default loan amounts ($2,453.8M).

---

## 🧠 Analytical Methodology

### Why These Dimensions Were Chosen

| Dimension | Analytical Rationale |
|---|---|
| **Age Groups (Bin)** | Age is a proxy for credit maturity, income stability, and financial obligations. Binning (18–27, 28–37, 38–47, 48–57, 58–67, 68–70) reveals lifecycle default patterns not visible in raw age. |
| **Credit Score Range** | The primary underwriting variable in retail lending. Analyzing against interest rates tests whether risk-based pricing is actually applied. |
| **Employment Type** | Stability of income source (Full-time vs Self-employed vs Part-time vs Unemployed) is a key default predictor — lenders often apply higher scrutiny to self-employed borrowers. |
| **Cross-Dimensional Segmentation** | Single-variable analysis misses interaction effects. E.g., age 28–37 + part-time + business loan = high default cluster invisible in univariate views. |
| **Marital Status + Mortgage** | Proxy for financial obligation load. Mortgage holder + divorced = maximum fixed-cost burden relative to discretionary income. |
| **Loan Purpose** | Business and education loans have different repayment risk profiles than auto or home loans — purpose segmentation reveals purpose-specific credit policy gaps. |

### Validation Approach
- Baseline comparison: All defaulter metrics compared against the full loan population
- Sample size check: Age 18–27 excluded from some rate comparisons (low N = $314.2M vs $1,600M+ for prime age groups)
- Interest rate disambiguation: The 3.4–3.8% rate clustering suggests these are base rates, not APR — rates appear to represent a narrow product category rather than risk-tiered pricing

---

## 📁 Repository Structure

```
loan-defaulters-analysis/
├── Loan_Analysis.twb          # Tableau workbook (XML, 6,269 lines, Tableau 2024.3)
├── README.md                  # This file
├── WORKBOOK_NOTES.md          # XML analysis, technical issues, optimization guide
├── .gitignore                 # Excludes .twbx, .hyper, .tde, OS files
├── data/
│   ├── README.md              # Field descriptions, data dictionary
│   ├── Loan_default.xlsx      # ← Upload needed (primary fact table)
│   └── Borrower_Demographics.xlsx  # ← Upload needed (dimension table)
└── screenshots/
    ├── 01_loan_defaulters_story_overview.jpg
    ├── 02_loan_distribution_by_age.jpg
    ├── 03_income_by_employment.jpg
    ├── 04_income_by_education.jpg
    ├── 05_dti_ratio_by_marital_status.jpg
    ├── 06_defaults_by_employment_and_age.jpg
    ├── 07_defaults_by_purpose_and_credit_score.jpg
    ├── 08_loan_distribution_by_employment.jpg
    ├── 09_default_by_loan_term_and_marital_status.jpg
    ├── 10_loan_vs_income_analysis_dashboard.jpg
    ├── 11_defaults_vs_credit_score_dashboard.jpg
    ├── 12_story_point2_self_employed_filter.jpg
    ├── 13_story_point3_credit_score_300_filter.jpg
    ├── 14_story_point4_credit_score_550_filter.jpg
    ├── 15_story_point5_highschool_education_filter.jpg
    └── 16_story_point6_credit_score_850_filter.jpg
```

---

## 🗃️ Data Model

**Join:** `Loan_default.LoanID = Borrower_Demographics.LoanID` (Inner Join)

### Loan_default Table (Fact)

| Field | Type | Description | Analytical Role |
|---|---|---|---|
| LoanID | String | Primary key | Join key |
| Age | Integer | Borrower age | Input for Age Groups Bin |
| Credit Score | Integer | FICO-style score (300–850) | Input for Credit Score Range bins |
| DTI Ratio | Float | Debt-to-Income ratio | Financial stress indicator |
| Income | Float | Annual income | Borrower capacity metric |
| Interest Rate | Float | Loan interest rate | Risk pricing variable |
| Loan Amount | Float | Principal amount | Exposure metric |
| Loan Term | Integer | Months | Repayment horizon |
| Months Employed | Integer | Employment tenure | Stability indicator |
| Num Credit Lines | Integer | Open credit lines | Credit utilization proxy |
| Default | Boolean | 1 = defaulted | Target variable |
| Age Groups Bin | Calculated | Age group bins | Demographic segmentation |
| Credit Score Range | Calculated | Score bands | Risk tier segmentation |
| Loan_default Count | Calculated | Default count | Aggregation metric |

### Borrower_Demographics Table (Dimension)

| Field | Type | Description | Analytical Role |
|---|---|---|---|
| LoanID | String | Foreign key | Join key |
| Education | String | Highest qualification | Human capital proxy |
| Employment Type | String | Full-time/Part-time/Self/Unemployed | Income stability |
| Has Co Signer | String | Y/N | Credit support |
| Has Dependents | String | Y/N | Financial obligation load |
| Has Mortgage | String | Y/N | Fixed obligation proxy |
| Loan Purpose | String | Auto/Business/Education/Home/Other | Use-case risk segmentation |
| Marital Status | String | Single/Married/Divorced | Financial household context |

---

## 🔍 Key Findings

| # | Finding | Rate | Business Implication |
|---|---|---|---|
| 1 | Ages 28–37 have the highest default rate | 6.3–8.9% | Prime target for enhanced underwriting review |
| 2 | Part-time workers aged 28–37 default most | 8.54% | Employment + age interaction is a strong default signal |
| 3 | Credit score 300 borrowers (single, max term, business) default at 10.45% | 10.45% | Low-score + high-risk purpose + single = reject or require co-signer |
| 4 | Credit score 850 borrowers still default at 12.05% when married, part-time, auto | 12.05% | High credit score alone insufficient; purpose + employment must gate approval |
| 5 | Interest rates cluster at 3.4–3.8% regardless of credit score | Flat | Risk-based pricing not implemented — margin compression on risky borrowers |
| 6 | Self-employed dominate loan volume ($2,124.4M) | 25.6% | Largest exposure segment with inherently volatile income |
| 7 | Income is equal across all education levels | ~25% each | Education not an income differentiator in this portfolio |

---

## 💡 Business Recommendations

### 1. Implement Risk-Based Pricing
**Finding:** Interest rates are flat (3.4–3.8%) regardless of credit score, employment, or purpose.
**Action:** Introduce credit-score-tiered pricing: 300–450 → base rate + 200bps; 450–600 → base rate + 100bps; 600+ → base rate.
**Expected Impact:** Revenue uplift of ~₹167M annually on the ₹8.36B book while maintaining competitiveness for prime borrowers.

### 2. Tighten Underwriting for Age 28–37 High-Risk Segments
**Finding:** Ages 28–37 show 6.3–8.9% default rates across employment types.
**Action:** Require co-signer or higher down-payment for 28–37 borrowers with DTI > 40%, part-time employment, and loan purpose = Business.
**Expected Impact:** Reduce default concentration in highest-volume age segment by an estimated 15–20%.

### 3. Mandatory Co-Signer Policy for Credit Score < 450
**Finding:** Credit score 300 borrowers default at 10.45% for business loans.
**Action:** Flag all applications with credit score ≤ 450 as requiring secondary review + co-signer or collateral before approval.
**Expected Impact:** Reduce NPA formation in the bottom credit tier by ₹20–30M annually.

### 4. Introduce Employment Stability Weighting in Scorecard
**Finding:** Part-time and unemployed borrowers show disproportionate default rates even controlling for age.
**Action:** Add employment type as a weighted variable (0.8x for part-time, 0.6x for unemployed) in the internal credit scorecard alongside FICO.
**Expected Impact:** Better differentiation within credit score bands where current scoring is blind to employment risk.

### 5. Purpose-Specific Loan Term Caps
**Finding:** Business loans have shorter average terms among defaulters, suggesting cash-flow mismatches.
**Action:** Cap business loan terms at 24 months for borrowers with < 12 months employment history; require business plan verification.
**Expected Impact:** Reduce business-purpose default by aligning repayment schedule with business ramp-up cycles.

### 6. Portfolio Rebalancing for Self-Employed Exposure
**Finding:** Self-employed borrowers represent 25.6% of total loan volume ($2,124.4M) — the single largest segment.
**Action:** Set a portfolio concentration limit of 22% for self-employed originations; compensate with stricter income documentation (2-year ITR + bank statement).
**Expected Impact:** Reduce concentration risk and potential systemic NPA impact if macroeconomic conditions tighten for self-employed income.

---

## 🛠️ Tech Stack

| Tool | Version | Purpose |
|---|---|---|
| Tableau Desktop | 2024.3 | Dashboard development |
| Microsoft Excel | — | Source data (.xlsx) |
| Git / GitHub | — | Version control & portfolio hosting |

---

## ▶️ How to Use

### Open in Tableau Desktop
```bash
# Clone the repo
git clone https://github.com/Steja28/loan-defaulters-analysis.git
cd loan-defaulters-analysis

# Open workbook
open Loan_Analysis.twb
# Tableau Desktop will prompt to reconnect to the data source
# Point it to: data/Loan_default.xlsx and data/Borrower_Demographics.xlsx
```

### Explore with Python (optional)
```python
import pandas as pd

loans = pd.read_excel('data/Loan_default.xlsx')
demographics = pd.read_excel('data/Borrower_Demographics.xlsx')
merged = loans.merge(demographics, on='LoanID')

default_rate = merged.groupby('Employment Type')['Default'].mean()
print(default_rate.sort_values(ascending=False))
```

---

## ⚠️ Known Issues

1. **Absolute data path in TWB:** The workbook stores a local macOS path for the data connection. When opening on a new machine, Tableau will prompt to re-point to the Excel files in the `data/` folder.
2. **Data files not committed:** `Loan_default.xlsx` and `Borrower_Demographics.xlsx` are excluded from this repo (add to `data/` folder and update the `.gitignore`).

---

## 📜 License

MIT License — see [LICENSE](./LICENSE) for details.

---

*Built as part of a Business Analyst portfolio — demonstrating end-to-end data storytelling from raw transactional data to actionable credit risk insights.*
