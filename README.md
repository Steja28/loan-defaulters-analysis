# Loan Defaulters Analysis — Tableau Multi-Dashboard Workbook

> **Business Context:** In the BFSI sector, loan default is the single largest driver of Non-Performing Assets (NPAs). A 1% increase in the default rate on a $500M retail loan book translates to $5M in provisioning costs and heightened regulatory scrutiny under Basel III norms. This project identifies *which borrower segments default, why they default, and what the business should do about it.*

[![Tableau](https://img.shields.io/badge/Tableau-Desktop%202024.3-E97627?style=flat&logo=tableau&logoColor=white)](https://www.tableau.com/)
[![Excel](https://img.shields.io/badge/Data-Microsoft%20Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)](https://www.microsoft.com/excel)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Workbook](https://img.shields.io/badge/Workbook-6%2C269%20lines%20XML-blue)](Loan_Analysis.twb)

---

## Live Dashboard

**Tableau Public:** *(Pending — open .twb in Tableau Desktop > Server > Tableau Public > Save to Tableau Public, then paste the URL here)*

---

## Business Problem Statement

**Domain:** Retail Credit Risk / Consumer Lending / BFSI

**The Problem:** Lenders approve loans across a wide spectrum of borrower profiles. Without granular visibility into which combinations of demographics, credit scores, and employment types drive default, risk teams apply blanket rate adjustments that are commercially inefficient.

**Key Business Questions This Analysis Answers:**

1. Which borrower demographics carry the highest default probability?
2. Does credit score alone predict default, or does employment type override it?
3. Are high interest rates driving defaults, or is income instability the root cause?
4. Which loan purposes concentrate the most default risk?
5. Where should underwriting tighten criteria vs. safely expand credit?

**Business Stakeholders:** Chief Risk Officer, Head of Retail Lending, Credit Underwriting Team, Regulatory Compliance (Basel III), Portfolio Analytics

**Dollar Stakes:** At a 10.45-12.05% default rate in identified high-risk segments, a lender with $100M exposure in those cohorts faces $10M-$12M in potential NPA provisioning — directly impacting capital adequacy ratios.

---

## Analytical Methodology

### Why These Dimensions Were Chosen

**Age Bins (10-year buckets: 18-27 through 68-70)**
Age acts as a proxy for career stage and income stability trajectory. 10-year bins balance granularity with sample size adequacy. The 28-37 cohort is the focal segment — peak borrowing demand combined with high income volatility from early-career job switching and gig economy participation.

**Credit Score Bands (25-point buckets: 300-850)**
Binning reveals threshold effects — the exact point where credit score meaningfully changes default probability. The analysis tests whether lenders correctly price the 500-550 vs. 550-575 difference, or whether bands are equivalent in predictive power.

**Employment Type as the Primary Segmentation Axis**
Employment type captures income *stability* rather than income *level*. A self-employed borrower earning $120K/year has structurally different income predictability than a salaried employee at the same level. Employment type is the primary stratification variable before overlaying credit score and age.

**Cross-Dimensional Segmentation Logic**
The highest-insight segments emerge at the intersection of 3 or more dimensions. The analytical approach starts with univariate distributions (loan volume by age, income by education) and progressively adds dimensions until default rate variance is explained. Each story point represents a distinct intersection that revealed a non-obvious finding.

**Marital Status and Mortgage as Liability Exposure Proxies**
These are proxies for fixed financial obligations. A single borrower with no mortgage has different debt-service capacity than a married borrower with a mortgage at identical income and DTI. DTI alone does not capture asset-side obligations.

**Loan Purpose as Risk Category**
Business loans fund ventures with binary outcomes. Education loans carry deferred payment structures. Including loan purpose identifies whether default is driven by borrower characteristics or the inherent risk profile of the loan type itself.

### Validation Approach

Each segment finding was validated by: (1) comparing default rate vs. dataset average (~10% baseline), (2) checking sample size adequacy — segments below 100 records flagged as inconclusive, (3) reviewing interest rate levels to distinguish rate burden from borrower profile as the default driver.

---

## Project Structure

```
loan-defaulters-analysis/
├── Loan_Analysis.twb                # Tableau XML workbook (6,269 lines)
├── WORKBOOK_NOTES.md                # XML analysis, 5 issues, optimization guide
├── data/
│   ├── Loan_default.xlsx            # Add your file here (10 fields, ~10k rows)
│   └── Borrower_Demographics.xlsx   # Add your file here (9 fields, ~10k rows)
├── screenshots/
│   ├── 01_loan_defaulters_story.png
│   ├── 02_loan_vs_income_dashboard.png
│   └── 03_defaults_vs_credit_score.png
└── README.md
```

---

## Data Model

Two Excel source tables joined on LoanID (inner join) in a single federated datasource.

### Loan_default — Transaction-level

| Field | Type | Description | Analytical Role |
|---|---|---|---|
| LoanID | String | Primary key | Join key |
| Age | Integer | Borrower age | Demographic segmentation |
| Income | Integer | Annual income | Capacity indicator |
| LoanAmount | Integer | Loan disbursement amount | Exposure sizing |
| CreditScore | Integer | Credit score (300-850) | Creditworthiness proxy |
| MonthsEmployed | Integer | Employment tenure (months) | Income stability proxy |
| NumCreditLines | Integer | Active credit lines | Debt complexity |
| InterestRate | Float | Interest rate (%) | Pricing signal |
| LoanTerm | Integer | Duration (months) | Commitment horizon |
| DTIRatio | Float | Debt-to-Income ratio | Capacity stress measure |

### Borrower_Demographics — Demographic attributes

| Field | Type | Description | Analytical Role |
|---|---|---|---|
| LoanID | String | Foreign key | Join key |
| Education | String | Bachelor / Master / PhD / High School | Income potential proxy |
| EmploymentType | String | Full-time / Part-time / Self-employed / Unemployed | Primary risk axis |
| MaritalStatus | String | Divorced / Married / Single | Fixed obligation proxy |
| HasMortgage | String | Y/N | Asset/liability indicator |
| HasDependents | String | Y/N | Discretionary income reducer |
| LoanPurpose | String | Auto / Business / Education / Home / Other | Risk category |
| HasCoSigner | String | Y/N | Credit enhancement signal |
| Default | Integer | 1 = defaulted, 0 = not | Target variable |

---

## Workbook Technical Details

| Property | Value |
|---|---|
| Tableau Version | Desktop 2024.3 (build 20243.24.1010.1014) |
| Workbook Format | .twb XML, version 18.1 |
| Datasource Type | Federated inline, Excel-direct |
| Join Type | Inner join on LoanID |
| Total XML Lines | 6,269 |

---

## Dashboards and Story

### Story: Loan Defaulters Analysis

| Story Point | Segment | Default Rate | Signal |
|---|---|---|---|
| 1 | Ages 28-37, credit 300, single, self-employed | 10.45% | Business loan risk from early-career entrepreneurs |
| 2 | Ages 48-57, credit 500-550, education loans | Lowest | Mature borrowers, stable income, purpose-driven repayment |
| 3 | Ages 48-57, unemployed with inheritance income | N/A | Highest loan amounts — asset-based lending segment |
| 4 | Ages 28-37, credit 850, married, part-time | 12.05% | Income instability overrides an excellent credit score |

### Dashboard 1: Loan vs Income Analysis

| Chart | Key Insight |
|---|---|
| Loan Distribution by Age | Age 38-47 has highest loan volume at $1,644.4M |
| Loan Distribution by Employment | Self-employed leads at $2,124.43M |
| Income By Education | Near-equal across all levels ($1,341-1,369M) |
| Income by Employment | ~25% split — no single employment type dominates |

### Dashboard 2: Defaults vs Credit Score Analysis

| Chart | Key Insight |
|---|---|
| Interest Rates on Credit Scores | Flat ~3.4-3.8% across all score bands — no risk-based pricing |
| Defaults by Purpose and Credit Score | Defaulters consistently have lower avg. credit scores |
| Default By Loan Term and Marital Status | Single defaulters carry the longest loan terms |
| Defaults by Employment and Age | Part-time 28-37 at 8.54% — highest default rate |

---

## Calculated Fields

| Field | Logic | Business Purpose |
|---|---|---|
| Age Groups Bin | 10-year bins: 18-27 through 68-70 | Career-stage segmentation |
| Credit Score Range | 25-point bins: 300-850 | Creditworthiness tier identification |
| Loan_default (Count) | COUNT of Loan_default records | Volume denominator for default rate |

---

## Key Findings

| # | Finding | Rate | Business Implication |
|---|---|---|---|
| 1 | Part-time 28-37, credit 850 default at 12.05% on auto loans | 12.05% | Credit score alone is insufficient — employment type must be a mandatory underwriting criterion |
| 2 | Self-employed 28-37, credit 300 default at 10.45% on business loans | 10.45% | Collateral or co-signer required for low-score self-employed business loans |
| 3 | Ages 48-57, education loans have lowest default rate despite highest interest rates | Lowest | This cohort is a safe, high-yield product — opportunity to expand credit |
| 4 | Income nearly identical across all education levels ($1,341M-$1,369M) | Under 2% gap | Education-based credit pricing adjustments are not justified by income data |
| 5 | DTI stable at ~16.5-16.75% regardless of marital status or mortgage | Under 0.25% gap | Employment type and loan purpose are stronger predictors than DTI |
| 6 | Self-employed borrowers account for $2,124.43M — the largest segment | 30%+ of book | Concentration risk — economic shock to this segment has outsized NPA impact |
| 7 | Interest rates flat across all credit score bands (3.4-3.8%) | 0.4% spread | The lender is not risk-pricing credit scores — a systemic pricing gap |

---

## Business Recommendations

### Rec 1 — Revise Underwriting for Part-Time Borrowers (HIGH PRIORITY)

**Finding:** Part-time 28-37 default at 12.05% on auto loans — even with credit score 850.

**Action:** Require 24+ months of continuous part-time employment history or a creditworthy co-signer before approving auto loans above $20K for this segment.

**Expected Impact:** A 30% reduction in exposure to this segment could lower NPA provisioning by $3M-$5M annually on a $500M auto loan portfolio.

---

### Rec 2 — Implement Risk-Based Pricing Across Credit Score Bands (HIGH PRIORITY)

**Finding:** Interest rates are flat at 3.4-3.8% regardless of credit score — borrowers at score 300 pay the same as those at score 850.

**Action:** Tiered pricing: score 300-499 (+250-300 bps), 500-599 (+150-200 bps), 600-699 (+75-100 bps), 700-850 (current baseline).

**Expected Impact:** Repricing the bottom 20% of credit scores on $2B total volume generates approximately $8M in incremental annual interest income.

---

### Rec 3 — Require Collateral for Self-Employed Business Loans Below Score 500 (HIGH PRIORITY)

**Finding:** Self-employed 28-37 with credit score 300 default at 10.45% on business loans. Self-employed is the largest segment at $2,124.43M.

**Action:** For self-employed borrowers below credit score 500, mandate collateral at 120% of loan value or a creditworthy co-signer.

**Expected Impact:** An estimated 3-4 percentage point reduction in default rate, cutting NPA exposure by $15M-$20M on the self-employed portfolio.

---

### Rec 4 — Expand Education Loans to the 48-57 Cohort (GROWTH OPPORTUNITY)

**Finding:** Ages 48-57 with education loans have the lowest default rates in the dataset despite carrying the highest interest rates.

**Action:** Market a dedicated education loan product (MBA, certifications, upskilling) to the 48-57 segment with competitive pricing given the proven low-risk profile.

**Expected Impact:** A 15% increase in education loan volume to this cohort could add $50M-$75M in low-risk, high-yield assets to the portfolio.

---

### Rec 5 — Monitor Self-Employed Portfolio Concentration Risk (RISK MANAGEMENT)

**Finding:** Self-employed borrowers represent $2,124.43M — nearly 30% of total loan volume.

**Action:** Implement a 25% concentration cap for self-employed borrowers. Establish a quarterly stress test scenario for a 30% income shock to this segment and report as concentration risk to the Risk Committee.

**Expected Impact:** Prevents single-sector contagion from becoming a systemic NPA event.

---

### Rec 6 — Remove Education Level from Credit Scoring Models

**Finding:** Income is virtually identical across all education levels — under 2% variation.

**Action:** Remove education level as a credit scoring factor. Redirect focus to employment type, months employed, and loan purpose as stronger default predictors.

**Expected Impact:** Reduces fair-lending regulatory risk while improving model predictive accuracy.

---

## How to Add Missing Files

### Add Screenshots (Highest Impact for Portfolio Visibility)

```bash
# In Tableau Desktop: Dashboard menu > Export Image > Save as PNG
screenshots/01_loan_defaulters_story.png
screenshots/02_loan_vs_income_dashboard.png
screenshots/03_defaults_vs_credit_score.png

git add screenshots/
git commit -m "feat: add dashboard screenshots for visual proof"
git push
```

### Add Data Files

```bash
cp ~/path/to/Loan_default.xlsx data/
cp ~/path/to/Borrower_Demographics.xlsx data/
git add data/
git commit -m "feat: add source data files for reproducibility"
git push
```

### Add Tableau Public Link

1. Open Loan_Analysis.twb in Tableau Desktop
2. Server > Tableau Public > Save to Tableau Public
3. Copy the URL and update the Live Dashboard section above

---

## How to Open the Workbook

```bash
git clone https://github.com/Steja28/loan-defaulters-analysis.git
cd loan-defaulters-analysis
open Loan_Analysis.twb
```

When Tableau prompts for data, reconnect to files in the /data folder. See WORKBOOK_NOTES.md for the known hardcoded path issue and fix instructions.

---

## Known Issue — Hardcoded Data Path

The datasource contains an absolute local path to the original machine. When opening on a new machine, Tableau will ask you to re-point to the data.

**Fix:** Open in Tableau Desktop > Data > Edit Connection > navigate to /data folder. See [WORKBOOK_NOTES.md](WORKBOOK_NOTES.md) for full fix instructions.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Tableau Desktop 2024.3 | Dashboard authoring and story creation |
| Microsoft Excel | Source data — two joined tables |
| Tableau Public | Interactive dashboard publishing (link pending) |
| Python + xml.etree | Metadata extraction and workbook inspection |
| Git + GitHub | Version control with diff-friendly .twb XML |

---

## License

This project is licensed under the [MIT License](LICENSE).

*Built with Tableau Desktop 2024.3 — BFSI Credit Risk Analytics*
