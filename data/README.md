# Data Directory

  This folder contains the source data files used in the Loan Defaulters Analysis Tableau workbook.

  ## Files

  | File | Description |
  |---|---|
  | `Loan_default.xlsx` | Transaction-level loan records (LoanID, Age, Income, LoanAmount, CreditScore, MonthsEmployed, NumCreditLines, InterestRate, LoanTerm, DTIRatio) |
  | `Borrower_Demographics.xlsx` | Demographic attributes per loan (LoanID, Education, EmploymentType, MaritalStatus, HasMortgage, LoanPurpose, Default flag) |

  ## Join Key

  Both tables are joined on `LoanID` (String — Primary/Foreign key).

  ## Notes

  - `Loan_default.xlsx` is the transaction-level table (~10,000 rows)
  - `Borrower_Demographics.xlsx` contains demographic enrichment data
  - Data files are excluded from version control if large (see `.gitignore`)
  - Add your own data files here when reproducing the analysis
  
