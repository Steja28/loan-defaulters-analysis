# Loan Defaulters Analysis — Tableau Multi-Dashboard Workbook

A multi-dashboard Tableau workbook analyzing loan default behavior across borrower demographics, credit profiles, employment types, and loan characteristics.

[![Tableau](https://img.shields.io/badge/Tableau-Desktop%202024.3-E97627?style=flat&logo=tableau&logoColor=white)](https://www.tableau.com/)
[![Excel](https://img.shields.io/badge/Data-Microsoft%20Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)](https://www.microsoft.com/excel)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Workbook](https://img.shields.io/badge/Workbook-6%2C269%20lines-blue)](Loan_Analysis.twb)

---

## Project Structure

```
loan-defaulters-analysis/
├── Loan_Analysis.twb                # Tableau XML workbook (version-control friendly)
├── data/
│   ├── Loan_default.xlsx            # Transaction-level records (10 fields, ~10k rows)
│   └── Borrower_Demographics.xlsx   # Demographic attributes (9 fields, ~10k rows)
├── screenshots/
│   ├── 01_loan_defaulters_story.png
│   ├── 02_loan_vs_income_dashboard.png
│   └── 03_defaults_vs_credit_score.png
└── README.md
```

> **Note:** The `.twbx` packaged workbook is excluded via `.gitignore` (binary, not diff-friendly).
> > Place `Loan_default.xlsx` and `Borrower_Demographics.xlsx` in the same directory as the `.twb` when opening locally.
> >
> > ---
> >
> > ## Data Model
> >
> > Two Excel source tables joined on `LoanID` (inner join) inside a single federated datasource.
> >
> > ### Loan_default — Transaction-level
> >
> > | Field | Type | Description |
> > |---|---|---|
> > | LoanID | String | Primary key |
> > | Age | Integer | Borrower age |
> > | Income | Integer | Annual income |
> > | LoanAmount | Integer | Loan disbursement amount |
> > | CreditScore | Integer | Credit score (300-850) |
> > | MonthsEmployed | Integer | Employment tenure in months |
> > | NumCreditLines | Integer | Number of active credit lines |
> > | InterestRate | Float | Loan interest rate (%) |
> > | LoanTerm | Integer | Loan duration (months) |
> > | DTIRatio | Float | Debt-to-Income ratio |
> >
> > ### Borrower_Demographics — Demographic attributes
> >
> > | Field | Type | Description |
> > |---|---|---|
> > | LoanID | String | Foreign key (joins on Loan_default.LoanID) |
> > | Education | String | Bachelor's / Master's / PhD / High School |
> > | EmploymentType | String | Full-time / Part-time / Self-employed / Unemployed |
> > | MaritalStatus | String | Divorced / Married / Single |
> > | HasMortgage | String | Y/N — Mortgage flag |
> > | HasDependents | String | Y/N — Dependents flag |
> > | LoanPurpose | String | Auto / Business / Education / Home / Other |
> > | HasCoSigner | String | Y/N — Co-signer flag |
> > | Default | Integer | Default flag (1 = defaulted, 0 = not) |
> >
> > ---
> >
> > ## Workbook Technical Details
> >
> > | Property | Value |
> > |---|---|
> > | Tableau Version | Desktop 2024.3 (build 20243.24.1010.1014) |
> > | Workbook Format | `.twb` XML, version 18.1 |
> > | Datasource Type | Federated (inline, Excel-direct) |
> > | Join Type | Inner join on LoanID |
> > | Total XML Lines | 6,269 |
> > | Source Platform | macOS |
> >
> > ### Opening the Workbook
> >
> > When opening `Loan_Analysis.twb`, Tableau will prompt to locate the data files. Point it to `Loan_default.xlsx` and `Borrower_Demographics.xlsx` in your local `/data` folder.
> >
> > ```bash
> > # Place both files relative to the .twb
> > data/
> >   Loan_default.xlsx
> >   Borrower_Demographics.xlsx
> > ```
> >
> > ---
> >
> > ## Dashboards and Sheets
> >
> > ### Story: Loan Defaulters Analysis
> >
> > | Story Point | Insight |
> > |---|---|
> > | 1 | Ages 28-37, credit score 300, single and self-employed — 10.45% default rate for business loans |
> > | 2 | Ages 48-57, credit score 500-550, education loans — lowest default rates despite highest interest rates |
> > | 3 | Ages 48-57, unemployed with inheritance income — highest loan amounts |
> > | 4 | Ages 28-37, credit score 850, married and part-time — 12.05% default rate on auto loans |
> >
> > ### Dashboard 1: Loan Vs Income Analysis
> >
> > | Chart | Key Insight |
> > |---|---|
> > | Loan Distribution by Age | Age 38-47 has highest loan volume ($1,644.4M) |
> > | Loan Distribution by Employment | Self-employed leads at $2,124.43M |
> > | Income By Education | Near-equal across all education levels (~$1,341-1,369M) |
> > | Income by Employment | ~25% split across all 4 employment types |
> >
> > ### Dashboard 2: Defaults Vs Credit Score Analysis
> >
> > | Chart | Key Insight |
> > |---|---|
> > | Interest Rates on Credit Scores | Rates flat ~3.4-3.8% across all credit score bands |
> > | Defaults by Purpose and Credit Score | Defaulters have consistently lower avg. credit scores |
> > | Default By Loan Term and Marital Status | Single defaulters carry the highest loan terms |
> > | Defaults by Employment and Age | Part-time 28-37 show highest default rate (8.54%) |
> >
> > ---
> >
> > ## Calculated Fields
> >
> > | Field | Logic |
> > |---|---|
> > | Age Groups Bin | Age binned in 10-year buckets: 18-27, 28-37, 38-47, 48-57, 58-67, 68-70 |
> > | Credit Score Range | Credit Score binned in 25-point buckets (300-850) |
> > | Loan_default (Count) | COUNT of records in Loan_default table |
> >
> > ---
> >
> > ## Key Findings
> >
> > - **Highest-risk segment:** Borrowers aged 28-37, married, part-time employed, credit score ~850 — 12.05% auto loan default rate despite low interest rates. Income instability is the driver.
> > - - **Business loan risk:** Ages 28-37, credit score 300, single — 10.45% default probability on business loans.
> >   - - **Lowest default segment:** Ages 48-57, credit score 500-550, education loans — highest rates, lowest defaults.
> >     - - **Employment matters:** Part-time workers aged 28-37 — highest default % (8.54%) across all employment types.
> >       - - **Income is education-neutral:** Income nearly identical across Bachelor's ($1,369M), Master's ($1,346M), PhD ($1,348M), and High School ($1,342M).
> >         - - **DTI is stable:** DTI hovers at ~16.5-16.75% regardless of marital status or mortgage presence.
> >           - - **Self-employed dominates volume:** $2,124.43M in loans — highest of any employment type.
> >            
> >             - ---
> >
> > ## How to Use
> >
> > ### Open in Tableau Desktop
> >
> > ```bash
> > # Clone the repository
> > git clone https://github.com/Steja28/loan-defaulters-analysis.git
> > cd loan-defaulters-analysis
> >
> > # Open workbook (Tableau must be installed)
> > open Loan_Analysis.twb
> > # Or: tableau-desktop Loan_Analysis.twb
> > ```
> >
> > When prompted, reconnect to data files in the `/data` folder.
> >
> > ### Inspect XML without Tableau
> >
> > ```bash
> > # List all worksheets defined in the workbook
> > grep -o '<worksheet name="[^"]*"' Loan_Analysis.twb
> >
> > # View all calculated fields and their formulas
> > grep -A3 '<calculation' Loan_Analysis.twb | head -60
> >
> > # Count total worksheet count
> > grep -c '<worksheet' Loan_Analysis.twb
> >
> > # Find the datasource connection details
> > grep -A5 'named-connection' Loan_Analysis.twb
> >
> > # Extract all column (field) names
> > grep '<remote-name>' Loan_Analysis.twb | sort -u
> > ```
> >
> > ### Python — Extract metadata programmatically
> >
> > ```python
> > import zipfile, xml.etree.ElementTree as ET
> >
> > # If working with the .twbx packaged version:
> > with zipfile.ZipFile("Loan_Analysis.twbx", "r") as z:
> >     twb_name = [f for f in z.namelist() if f.endswith(".twb")][0]
> >     with z.open(twb_name) as f:
> >         tree = ET.parse(f)
> >
> > # If working with the .twb XML directly:
> > # tree = ET.parse("Loan_Analysis.twb")
> >
> > root = tree.getroot()
> >
> > # List all worksheets
> > worksheets = [ws.get("name") for ws in root.iter("worksheet")]
> > print("Sheets:", worksheets)
> >
> > # List datasources
> > datasources = [ds.get("caption") or ds.get("name") for ds in root.iter("datasource")]
> > print("Datasources:", datasources)
> >
> > # List calculated fields (columns with formulas)
> > calcs = [(c.get("caption") or c.get("name"), c.get("formula"))
> >          for c in root.iter("column")
> >          if c.get("formula")]
> > for name, formula in calcs:
> >     print(f"  {name}: {formula}")
> > ```
> >
> > ---
> >
> > ## Git Best Practices for This Project
> >
> > ```bash
> > # Stage the .twb (XML) for diff-friendly version control
> > git add Loan_Analysis.twb
> > git add data/
> > git add screenshots/
> > git add README.md
> >
> > # Commit with conventional commit messages
> > git commit -m "feat: add loan defaulters multi-dashboard workbook"
> > git commit -m "fix: update datasource path for portability"
> > git commit -m "docs: add key findings and data model documentation"
> > ```
> >
> > ---
> >
> > ## Tech Stack
> >
> > | Tool | Purpose |
> > |---|---|
> > | Tableau Desktop 2024.3 | Dashboard authoring and story creation |
> > | Microsoft Excel (.xlsx) | Source data — two joined tables |
> > | Tableau Public (optional) | Publishing and sharing dashboards |
> > | Python + xml.etree | Metadata extraction and workbook inspection |
> >
> > ---
> >
> > ## Dashboard Screenshots
> >
> > Screenshots will be added to `/screenshots` after workbook export.
> >
> > | Preview | Dashboard |
> > |---|---|
> > | `01_loan_defaulters_story.png` | Story — Loan Defaulters Analysis (4 story points) |
> > | `02_loan_vs_income_dashboard.png` | Dashboard 1 — Loan vs Income |
> > | `03_defaults_vs_credit_score.png` | Dashboard 2 — Defaults vs Credit Score |
> >
> > ---
> >
> > ## Known Issue — Hardcoded Data Path
> >
> > The current `Loan_Analysis.twb` contains an **absolute local path** to the data files:
> >
> > ```
> > /Users/saitejaellendula/Downloads/Loan_Analysis.twb Files/Data/Project/Loan_default.xls
> > ```
> >
> > When opening on a different machine, Tableau will ask you to re-point to the data. To fix this permanently, open the workbook in Tableau Desktop and use **Data > Edit Connection** to update and save the path.
> >
> > ---
> >
> > ## Roadmap
> >
> > **Upcoming improvements:**
> >
> > - Fix and update hardcoded datasource path for portability
> > - - Export dashboard screenshots to /screenshots
> >   - - Add Tableau Public embed link
> >     - - Add source data files to /data
> >       - - Publish to Tableau Public
> >        
> >         - ---
> >
> > ## License
> >
> > This project is licensed under the [MIT License](LICENSE).
> >
> > *Built with Tableau Desktop 2024.3 · Data Analysis and Visualization*
> > 
