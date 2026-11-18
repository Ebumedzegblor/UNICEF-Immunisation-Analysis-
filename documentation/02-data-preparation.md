# Data Preparation and Import

## 1. Dataset Acquisition

The WUENIC 2025 revision dataset was downloaded in Excel (.xlsx) format from UNICEF's official immunisation data resources.

## 2. File Conversion

The Excel dataset was opened in Google Sheets and converted into CSV format for import into SQLiteOnline.

## 3. Data Import

The CSV file was imported into SQLiteOnline.

The imported table was named:

`wuenic2025rev_webupdatexlsxcountry`

During import, SQLiteOnline assigned generic column names (`c1`, `c2`, `c3`, etc.).

The original column headers were also imported as the first data row.

## 4. Creation of the Cleaned Table

A new table named `wuenic_clean` was created using SQL.

The original column names were assigned as meaningful column aliases.

The imported header row was excluded using:

`WHERE c1 <> 'iso3c'`

The original imported table was preserved.

## 5. SQL Used

```sql
CREATE TABLE wuenic_clean AS
SELECT
    c1 AS iso3c,
    c2 AS country,
    c3 AS vaccine,
    c4 AS year,
    c5 AS target_grp,
    c6 AS coverage,
    c7 AS source,
    c8 AS unicef,
    c9 AS gavi,
    c10 AS world_bank,
    c11 AS world_bank_fcv,
    c12 AS who,
    c13 AS who_fcv,
    c14 AS african_union,
    c15 AS european_union
FROM wuenic2025rev_webupdatexlsxcountry
WHERE c1 <> 'iso3c'; '''


## 6. Verification
The original imported table contained 58,292 rows.

The cleaned table contained 58,291 rows.

The difference of one row corresponds to the original header row that was excluded.


## 7. Preparation Decision
The original imported table was retained to preserve the raw import.

The cleaned table will be used for subsequent data quality checks and analysis.
