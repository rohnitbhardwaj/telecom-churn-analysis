# Data Cleaning Log

**Dataset:** Telco Customer Churn (Kaggle), a sample dataset from a fictional telecom company
**Tool:** Excel Power Query
**Raw data:** 7,043 rows, 21 columns
**Cleaned data:** 7,043 rows, 22 columns (21 original + `TenureGroup`), saved as `Telecom_Clean_Data.csv`

## Cleaning steps

| # | Column | Issue | Rows affected | Fix applied |
|---|--------|-------|---------------|-------------|
| 1 | `customerID` | Identifier, not a number to calculate with | All | Set data type to Text so it is treated as a category/label |
| 2 | `TotalCharges` | Stored as text, with blank values (all belong to customers with tenure = 0) | 11 | Changed type to Decimal Number, set the blanks to 0 |
| 3 | `SeniorCitizen` | Coded 0/1 while other flag columns use Yes/No | All | Changed type to Text, then replaced 0 with No and 1 with Yes |
| 4 | `MultipleLines` and the six internet service columns (`OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies`) | Extra labels "No phone service" / "No internet service" alongside Yes/No | 7 columns | Standardised to "No" so every service column is Yes/No |
| 5 | `PaymentMethod` | "(automatic)" in the names of two methods | All | Shortened to "Bank transfer" and "Credit card" |
| 6 | New column: `TenureGroup` | Needed to analyse churn by customer age | All | Added four groups from `tenure`: 0-12, 13-24, 25-48 and 49+ months |

## Notes on decisions

- **`TotalCharges` blanks set to 0:** the 11 blank rows are customers with tenure of 0 months, so they have not been billed yet. 0 is the logical value.
- **"No internet service" and "No phone service" set to "No":** the `InternetService` and `PhoneService` columns already show who has no internet or phone, so no information is lost. This gives every service column the same two values.

## Validation

After cleaning and export, the CSV was loaded in Python:

- 7,043 rows, as in the raw data
- Data types, null values and duplicates checked
- Overall churn rate of 26.5% and churn rates by contract, tenure group, internet service, payment method and tech support match the Excel pivot tables

## Columns created after cleaning (not part of the Power Query step)

| Column | Created in | Definition |
|--------|-----------|------------|
| `ChurnFlag` | Python | 1 if `Churn` = Yes, otherwise 0 |
| `Segment` | Python | "High-Risk Customers" if Contract = Month-to-month, InternetService = Fiber optic and PaymentMethod = Electronic check; otherwise "All Other Customers" |
