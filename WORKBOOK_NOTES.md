# Workbook Technical Notes — Loan_Analysis.twb

This document captures the internal structure, known issues, and optimization notes for the `Loan_Analysis.twb` Tableau workbook derived from direct XML inspection.

---

## File Metadata

| Property | Value |
|---|---|
| File | `Loan_Analysis.twb` |
| Tableau Build | 2024.3.0 (20243.24.1010.1014) |
| XML Version | 18.1 |
| Source Platform | macOS |
| Total Lines | 6,269 |
| Encoding | UTF-8 |

---

## Datasource Structure

### Connection Type

```
class: federated (inline)
  └── named-connection: excel-direct
          └── Loan_default.xls (Sheet: Loan_default$)
                  └── Borrower_Demographics (Sheet: Borrower_Demographics$)
                  ```

                  ### Join Definition

                  ```xml
                  <relation join='inner' type='join'>
                    <clause type='join'>
                        <expression op='='>
                              [Loan_default].[LoanID] = [Borrower_Demographics].[LoanID]
                                  </expression>
                                    </clause>
                                    </relation>
                                    ```

                                    Join type: **Inner join** on `LoanID`

                                    ### Column Mapping (cols section)

                                    The workbook maps the following fields from the federated datasource:

                                    | Mapped Field | Source Table | Source Column |
                                    |---|---|---|
                                    | [Age] | Loan_default | Age |
                                    | [CreditScore] | Loan_default | CreditScore |
                                    | [DTIRatio] | Loan_default | DTIRatio |
                                    | [Income] | Loan_default | Income |
                                    | [InterestRate] | Loan_default | InterestRate |
                                    | [LoanAmount] | Loan_default | LoanAmount |
                                    | [LoanID] | Loan_default | LoanID |
                                    | [LoanTerm] | Loan_default | LoanTerm |
                                    | [MonthsEmployed] | Loan_default | MonthsEmployed |
                                    | [NumCreditLines] | Loan_default | NumCreditLines |
                                    | [Default] | Borrower_Demographics | Default |
                                    | [Education] | Borrower_Demographics | Education |
                                    | [EmploymentType] | Borrower_Demographics | EmploymentType |
                                    | [HasCoSigner] | Borrower_Demographics | HasCoSigner |
                                    | [HasDependents] | Borrower_Demographics | HasDependents |
                                    | [HasMortgage] | Borrower_Demographics | HasMortgage |
                                    | [LoanID (Borrower!Demographics)] | Borrower_Demographics | LoanID |
                                    | [LoanPurpose] | Borrower_Demographics | LoanPurpose |
                                    | [MaritalStatus] | Borrower_Demographics | MaritalStatus |

                                    ---

                                    ## Known Issues and Optimization Recommendations

                                    ### Issue 1 — Hardcoded Absolute File Path (Critical)

                                    **Problem:** The datasource connection contains a machine-specific absolute path:

                                    ```
                                    filename='/Users/saitejaellendula/Downloads/Loan_Analysis.twb Files/Data/Project/Loan_default.xls'
                                    ```

                                    **Impact:** The workbook will fail to open on any machine other than the original author's Mac without re-pointing to the data.

                                    **Fix Options:**

                                    Option A — Fix via Tableau Desktop (Recommended):
                                    1. Open `Loan_Analysis.twb` in Tableau Desktop
                                    2. Go to **Data menu > [datasource name] > Edit Connection**
                                    3. Navigate to the new data file location
                                    4. Save the workbook — Tableau updates the path

                                    Option B — Fix in XML directly (Advanced):
                                    ```bash
                                    # Replace the hardcoded path with a relative reference
                                    sed -i 's|filename='"'"'/Users/saitejaellendula/Downloads/Loan_Analysis.twb Files/Data/Project/Loan_default.xls'"'"'|filename='"'"'./data/Loan_default.xlsx'"'"'|g' Loan_Analysis.twb
                                    ```

                                    Then convert the `.xls` source file to `.xlsx` and place it in `/data`.

                                    ---

                                    ### Issue 2 — Datasource Caption Formatting

                                    **Problem:** The datasource caption has a formatting inconsistency:

                                    ```xml
                                    caption='Loan_default+ Borrower_Demographics'
                                    ```

                                    **Fix:** Rename in Tableau Desktop by right-clicking the datasource in the Data pane and selecting Rename.

                                    Suggested name: `Loan Analysis (Loan_default + Borrower_Demographics)`

                                    ---

                                    ### Issue 3 — Inner Join May Exclude Unmatched Records

                                    **Problem:** An inner join on LoanID means any LoanID present in one table but not the other will be silently dropped.

                                    **Recommendation:** Verify row counts match expectations. If they do not:
                                    1. Switch to a Left join (keeping all Loan_default records)
                                    2. Add a calculated field to flag missing demographic data:

                                    ```
                                    // Flag records missing demographic data
                                    ISNULL([Education])
                                    ```

                                    ---

                                    ### Issue 4 — gridOrigin Uses Legacy Range Format

                                    **Problem:** Column ranges use the legacy Excel format:

                                    ```xml
                                    gridOrigin='A1:J65536:no:A1:J65536:0'
                                    ```

                                    **Recommendation:** When re-connecting to updated `.xlsx` files, Tableau will modernize these ranges automatically. No manual action needed.

                                    ---

                                    ### Issue 5 — Source File Extension Mismatch

                                    **Problem:** The datasource references `.xls` (legacy Excel format) but the repo documents `.xlsx` files.

                                    **Fix:** Convert `Loan_default.xls` to `Loan_default.xlsx` (File > Save As in Excel), then update the connection in Tableau Desktop.

                                    ---

                                    ## Document-Format-Change-Manifest Flags

                                    The workbook enables the following modern Tableau features:

                                    | Flag | Description |
                                    |---|---|
                                    | AccessibleZoneTabOrder | Enables tab-order accessibility in dashboard zones |
                                    | AnimationOnByDefault | Enables mark animations by default |
                                    | AutoCreateAndUpdateDSDPhoneLayouts | Auto-creates phone layouts for dashboards |
                                    | MarkAnimation | Enables animated mark transitions |
                                    | ObjectModelEncapsulateLegacy | Uses encapsulated object model for legacy compatibility |
                                    | ObjectModelTableType | Uses table-type object model |
                                    | SchemaViewerObjectModel | Enables schema viewer integration |
                                    | SetMembershipControl | Enables set membership controls |
                                    | SheetIdentifierTracking | Enables sheet identifier tracking |
                                    | WindowsPersistSimpleIdentifiers | Persists simple identifiers on Windows |
                                    | ZoneBackgroundTransparency | Enables transparent zone backgrounds in dashboards |

                                    ---

                                    ## Calculated Fields — Optimization Notes

                                    ### Age Groups Bin

                                    Suggested improvement — replace with a CASE statement for cleaner output:

                                    ```
                                    // Current (inferred bin logic):
                                    INT([Age] / 10) * 10

                                    // Recommended — explicit labeled bins:
                                    CASE TRUE
                                      WHEN [Age] >= 18 AND [Age] <= 27 THEN "18-27"
                                        WHEN [Age] >= 28 AND [Age] <= 37 THEN "28-37"
                                          WHEN [Age] >= 38 AND [Age] <= 47 THEN "38-47"
                                            WHEN [Age] >= 48 AND [Age] <= 57 THEN "48-57"
                                              WHEN [Age] >= 58 AND [Age] <= 67 THEN "58-67"
                                                WHEN [Age] >= 68 THEN "68-70"
                                                  ELSE "Unknown"
                                                  END
                                                  ```

                                                  ### Credit Score Range

                                                  Suggested improvement:

                                                  ```
                                                  // Current (inferred bin logic):
                                                  INT([CreditScore] / 25) * 25

                                                  // Recommended — labeled range bins:
                                                  CASE TRUE
                                                    WHEN [CreditScore] >= 300 AND [CreditScore] < 400 THEN "300-399 (Very Poor)"
                                                      WHEN [CreditScore] >= 400 AND [CreditScore] < 500 THEN "400-499 (Poor)"
                                                        WHEN [CreditScore] >= 500 AND [CreditScore] < 600 THEN "500-599 (Fair)"
                                                          WHEN [CreditScore] >= 600 AND [CreditScore] < 700 THEN "600-699 (Good)"
                                                            WHEN [CreditScore] >= 700 AND [CreditScore] < 750 THEN "700-749 (Very Good)"
                                                              WHEN [CreditScore] >= 750 THEN "750-850 (Exceptional)"
                                                                ELSE "Unknown"
                                                                END
                                                                ```

                                                                ### Suggested New Calculated Fields

                                                                ```
                                                                // Default Rate %
                                                                SUM([Default]) / COUNT([LoanID])

                                                                // High-Risk Flag (credit score below 500)
                                                                [CreditScore] < 500

                                                                // Income-to-Loan Ratio
                                                                [Income] / [LoanAmount]

                                                                // Risk Tier
                                                                CASE TRUE
                                                                  WHEN [CreditScore] < 500 AND [EmploymentType] = "Part-time" THEN "High Risk"
                                                                    WHEN [CreditScore] >= 700 AND [EmploymentType] = "Full-time" THEN "Low Risk"
                                                                      ELSE "Medium Risk"
                                                                      END
                                                                      ```

                                                                      ---

                                                                      ## Portability Checklist

                                                                      Before sharing this workbook:

                                                                      - [ ] Fix hardcoded file path (Issue 1 above)
                                                                      - [ ] Rename datasource caption (Issue 2 above)
                                                                      - [ ] Verify join produces expected row count (Issue 3 above)
                                                                      - [ ] Convert .xls source to .xlsx (Issue 5 above)
                                                                      - [ ] Export screenshots to /screenshots
                                                                      - [ ] Package as .twbx if sharing with embedded data
                                                                      - [ ] Test workbook opens cleanly on a second machine

                                                                      ---

                                                                      *Generated from direct XML inspection of `Loan_Analysis.twb` (6,269 lines)*
                                                                      
