# Loan Defaulters Analysis — Tableau Dashboard

[![Tableau](https://img.shields.io/badge/Tableau-Desktop%202024.3-E97627?style=flat&logo=tableau&logoColor=white)](https://www.tableau.com/)
[![Excel](https://img.shields.io/badge/Data-Microsoft%20Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)](https://www.microsoft.com/excel)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## What This Project Is About

I built this dashboard to answer a question that keeps lending teams up at night: **who is actually going to default, and why?**

Blanket interest rate adjustments and generic credit score cutoffs are the industry default — but they miss the nuance. A 28-year-old self-employed borrower with a credit score of 300 carries a very different risk profile than a 50-year-old salaried employee with the same score. This project digs into that nuance across 10+ borrower dimensions to surface segments where default risk is genuinely elevated — and where the business can act on it.

The workbook covers three dashboards: a Loan vs. Income analysis, a Defaults vs. Credit Score deep-dive, and a 4-point story that walks through the highest and lowest risk segments with specific numbers behind each finding.

---

## Live Dashboard

**Tableau Public:** *(Coming soon — will be linked once published)*

---

## The Data

Two Excel tables, joined on `LoanID`:

**Loan_default** — the transaction-level table

| Field | Type | What It Captures |
|---|---|---|
| LoanID | String | Primary key |
| Age | Integer | Borrower age |
| Income | Integer | Annual income |
| LoanAmount | Integer | Amount disbursed |
| CreditScore | Integer | Score from 300–850 |
| MonthsEmployed | Integer | Employment tenure |
| NumCreditLines | Integer | Active credit lines |
| InterestRate | Float | Loan rate (%) |
| LoanTerm | Integer | Duration in months |
| DTIRatio | Float | Debt-to-Income ratio |

**Borrower_Demographics** — the profile table

| Field | Type | What It Captures |
|---|---|---|
| LoanID | String | Foreign key |
| Education | String | Bachelor's / Master's / PhD / High School |
| EmploymentType | String | Full-time / Part-time / Self-employed / Unemployed |
| MaritalStatus | String | Divorced / Married / Single |
| HasMortgage | String | Whether borrower carries a mortgage |
| LoanPurpose | String | Auto / Business / Education / Home / Other |
| Default | Integer | 1 = defaulted, 0 = did not |

---

## What the Dashboards Show

### Story: Loan Defaulters Analysis
Four story points that walk through the most important segments:

| Story Point | What I Found |
|---|---|
| 1 | Ages 28–37, credit score 300, single and self-employed: **10.45% default rate** on business loans |
| 2 | Ages 48–57, credit score 500–550, education loans: **lowest default rates** despite carrying higher interest rates |
| 3 | Ages 48–57, unemployed with inheritance income: **highest average loan amounts** in the book |
| 4 | Ages 28–37, credit score 850, married and part-time: **12.05% default rate** on auto loans — high score doesn't mean low risk |

### Dashboard 1 — Loan vs. Income Analysis
- Age group 38–47 carries the highest total loan volume at **$1,644.4M**
- Self-employed borrowers represent the largest employment segment at **$2,124.43M** — over 30% of the total book
- Income is nearly identical across all education levels (range: $1,342M–$1,369M), suggesting education is not a meaningful income predictor here

### Dashboard 2 — Defaults vs. Credit Score Analysis
- Interest rates are flat across all credit score bands (~3.4–3.8%), pointing to a **risk-based pricing gap**
- Defaulters consistently have lower average credit scores across every loan purpose
- Single borrowers carry the longest loan terms among defaulters
- Part-time workers aged 28–37 have the highest employment-based default rate at **8.54%**

---

## Key Takeaways

A few things stood out that I wasn't expecting going in:

- **High credit score ≠ low risk.** Part-time 28–37 year olds with near-perfect credit scores still default at 12% on auto loans. Income volatility is the real driver, not the score.
- **The pricing model is blind to risk.** Interest rates don't change meaningfully across credit score bands. That's a structural issue for any lender using this portfolio.
- **Self-employed concentration is a hidden risk.** $2.1B in exposure from one employment type — with no differentiated underwriting — is worth a closer look.
- **Education loans to the 48–57 cohort are the safest segment in the book.** If there's a segment to grow, this is it.
- **DTI stays flat (~16.5–16.75%) regardless of marital status or mortgage.** That surprised me — I expected married borrowers with mortgages to show higher DTI.

---

## What the Business Should Do

Findings are only useful if they lead somewhere. Here's what I'd recommend based on this analysis:

1. **Tighten underwriting for part-time 28–37 borrowers on auto loans.** A 12% default rate on this segment exceeds typical 7–8% industry thresholds. Require 24+ months employment history or a co-signer.
2. **Introduce risk-based pricing tied to credit score bands.** Flat rates across a 300–850 range leave yield on the table for lower-risk borrowers and misprices risk for higher-risk ones.
3. **Add income stability checks for self-employed borrowers.** Proof of 2-year business continuity or collateral requirements for scores under 500.
4. **Cap self-employed portfolio exposure at 25%.** Current concentration at 30%+ creates correlated default risk.
5. **Grow the 48–57 education loan segment.** Lowest default rate, stable income profile, proven repayment behavior — this is the segment to market to.
6. **Remove education level from scoring models.** Income is identical across all education tiers. It's adding noise, not signal.

---

## How to Use This Repo

**Open the workbook:**
```bash
# Rename and extract the .twbx if you have it
cp Loan_Analysis.twbx Loan_Analysis.zip
unzip Loan_Analysis.zip -d extracted/
```

**Or inspect the XML directly:**
```bash
# List all worksheets
grep -o '<worksheet name="[^"]*"' Loan_Analysis.twb

# View calculated fields
grep -A3 '<calculation' Loan_Analysis.twb | head -60
```

**Note:** The `.twb` file is 432KB / 6,269 lines and exceeds GitHub's inline renderer on some views. Click **Raw** to inspect the full XML, or download and open in any text editor.

---

## Project Structure

```
loan-defaulters-analysis/
├── Loan_Analysis.twb          # Tableau XML workbook (version-controllable)
├── WORKBOOK_NOTES.md          # Analysis notes and workbook guide
├── data/
│   ├── Loan_default.xlsx
│   └── Borrower_Demographics.xlsx
├── screenshots/               # Dashboard exports (PNG)
├── .gitignore
└── README.md
```

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Tableau Desktop 2024.3 | Dashboard authoring |
| Microsoft Excel | Source data |
| Git / GitHub | Version control |
| Tableau Public | Sharing *(coming soon)* |

---

## License

MIT — see [LICENSE](LICENSE) for details.
