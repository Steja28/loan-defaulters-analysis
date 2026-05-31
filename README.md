# Loan Defaulters Analysis — Tableau Multi-Dashboard Workbook

A multi-dashboard Tableau workbook analyzing loan default behavior across borrower demographics, credit profiles, employment types, and loan characteristics.

[![Tableau](https://img.shields.io/badge/Tableau-Desktop%202023.x-E97627?style=flat&logo=tableau&logoColor=white)](https://www.tableau.com/)
[![Excel](https://img.shields.io/badge/Data-Microsoft%20Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)](https://www.microsoft.com/excel)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## Project Structure

```
loan-defaulters-analysis/
├── Loan_Analysis.twbx               # Packaged Tableau workbook (with embedded data)
├── Loan_Analysis.twb                # Extracted XML workbook (for version control diffs)
├── data/
│   ├── Loan_default.xlsx
│   └── Borrower_Demographics.xlsx
├── screenshots/
│   ├── 01_loan_defaulters_story.png
│   ├── 02_loan_vs_income_dashboard.png
│   ├── 03_defaults_vs_credit_score.png
│   └── ...
└── README.md
```

---

## Data Model

Two source tables joined on `LoanID`:

### Loan_default — Transaction-level

| Field | Type | Description |
|---|---|---|
| LoanID | String | Primary key |
| Age | Integer | Borrower age |
| Income | Integer | Annual income |
| LoanAmount | Integer | Loan disbursement amount |
| CreditScore | Integer | Credit score (300-850) |
| MonthsEmployed | Integer | Employment tenure in months |
| NumCreditLines | Integer | Number of active credit lines |
| InterestRate | Float | Loan interest rate (%) |
| LoanTerm | Integer | Loan duration (months) |
| DTIRatio | Float | Debt-to-Income ratio |

### Borrower_Demographics — Demographic attributes

| Field | Type | Description |
|---|---|---|
| LoanID | String | Foreign key |
| Education | String | Bachelor's / Master's / PhD / High School |
| EmploymentType | String | Full-time / Part-time / Self-employed / Unemployed |
| MaritalStatus | String | Divorced / Married / Single |
| HasMortgage | String | Mortgage flag |
| LoanPurpose | String | Auto / Business / Education / Home / Other |
| Default | Integer | Default flag (1 = defaulted) |

---

## Dashboards and Sheets

### Story: Loan Defaulters Analysis

| Story Point | Insight |
|---|---|
| 1 | Ages 28-37, credit score 300, single and self-employed — 10.45% default rate for business loans |
| 2 | Ages 48-57, credit score 500-550, education loans — lowest default rates despite highest interest rates |
| 3 | Ages 48-57, unemployed with inheritance income — highest loan amounts |
| 4 | Ages 28-37, credit score 850, married and part-time — 12.05% default rate on auto loans |

### Dashboard 1: Loan Vs Income Analysis

| Chart | Key Insight |
|---|---|
| Loan Distribution by Age | Age 38-47 has highest loan volume ($1,644.4M) |
| Loan Distribution by Employment | Self-employed leads at $2,124.43M |
| Income By Education | Near-equal across all education levels (~$1,341-1,369M) |
| Income by Employment | ~25% split across all 4 employment types |

### Dashboard 2: Defaults Vs Credit Score Analysis

| Chart | Key Insight |
|---|---|
| Interest Rates on Credit Scores | Rates flat ~3.4-3.8% across all credit score bands |
| Defaults by Purpose and Credit Score | Defaulters have consistently lower avg. credit scores |
| Default By Loan Term and Marital Status | Single defaulters carry the highest loan terms |
| Defaults by Employment and Age | Part-time 28-37 show highest default rate (8.54%) |

---

## Calculated Fields

| Field | Logic |
|---|---|
| Age Groups Bin | Age binned in 10-year buckets: 18-27, 28-37, 38-47, 48-57, 58-67, 68-70 |
| Credit Score Range | Credit Score binned in 25-point buckets (300-850) |
| Loan_default (Count) | COUNT of records in Loan_default table |

---

## Key Findings

- **Highest-risk segment:** Borrowers aged 28-37, married, part-time employed, credit score ~850 → 12.05% auto loan default rate despite low interest rates. Income instability is the driver.
- - **Business loan risk:** Ages 28-37, credit score 300, single → 10.45% default probability on business loans.
  - - **Lowest default segment:** Ages 48-57, credit score 500-550, education loans — highest rates, lowest defaults.
    - - **Employment matters:** Part-time workers aged 28-37 → highest default % (8.54%) across all employment types.
      - - **Income is education-neutral:** Income nearly identical across Bachelor's ($1,369M), Master's ($1,346M), PhD ($1,348M), and High School ($1,342M).
        - - **DTI is stable:** DTI hovers at ~16.5-16.75% regardless of marital status or mortgage presence.
          - - **Self-employed dominates volume:** $2,124.43M in loans — highest of any employment type.
           
            - ---

            ## How to Use

            ### Open in Tableau Desktop

            ```bash
            # Direct open
            tableau-desktop Loan_Analysis.twbx

            # Extract the .twb for inspection
            cp Loan_Analysis.twbx Loan_Analysis.zip
            unzip Loan_Analysis.zip -d extracted/
            ```

            ### Inspect XML without Tableau

            ```bash
            # List all worksheets
            grep -o '<worksheet name="[^"]*"' Loan_Analysis.twb

            # View calculated fields
            grep -A3 '<calculation' Loan_Analysis.twb | head -60

            # Count total sheets
            grep -c '<worksheet' Loan_Analysis.twb
            ```

            ### Python — Extract metadata programmatically

            ```python
            import zipfile, xml.etree.ElementTree as ET

            with zipfile.ZipFile("Loan_Analysis.twbx", "r") as z:
                twb_name = [f for f in z.namelist() if f.endswith(".twb")][0]
                with z.open(twb_name) as f:
                    tree = ET.parse(f)

            root = tree.getroot()

            # List all worksheets
            worksheets = [ws.get("name") for ws in root.iter("worksheet")]
            print("Sheets:", worksheets)

            # List calculated fields
            calcs = [(c.get("name"), c.get("formula"))
                     for c in root.iter("column")
                     if c.get("formula")]
            for name, formula in calcs:
                print(f"{name}: {formula}")
            ```

            ---

            ## Git Best Practices for This Project

            ```bash
            # Store .twb (XML), not .twbx (binary) for meaningful diffs
            git add Loan_Analysis.twb
            git add screenshots/
            git add README.md
            git commit -m "feat: add loan defaulters analysis dashboard"
            ```

            ---

            ## Tech Stack

            | Tool | Purpose |
            |---|---|
            | Tableau Desktop 2023.x+ | Dashboard authoring |
            | Microsoft Excel / CSV | Source data |
            | Tableau Public (optional) | Publishing / sharing |

            ---

            ## Dashboard Screenshots

            Screenshots will be added to `/screenshots` after workbook export.

            | Preview | Dashboard |
            |---|---|
            | `01_loan_defaulters_story.png` | Story — Loan Defaulters Analysis |
            | `02_loan_vs_income_dashboard.png` | Dashboard 1 — Loan vs Income |
            | `03_defaults_vs_credit_score.png` | Dashboard 2 — Defaults vs Credit Score |

            ---

            ## Roadmap

            **Upcoming improvements:**

            - Add Tableau Public embed link
            - - Export dashboard screenshots to /screenshots
              - - Replace placeholder .twb with actual XML workbook export
                - - Add source data files to /data
                  - - Publish to Tableau Public
                   
                    - ---

                    ## License

                    This project is licensed under the [MIT License](LICENSE).

                    *Built with Tableau Desktop · Data Analysis and Visualization*
                    
