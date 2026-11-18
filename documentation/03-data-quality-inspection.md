# Data Quality Inspection

## 1. Initial Record Inspection

The first 10 records of the cleaned table were examined.

The records showed repeated country and vaccine combinations across different years.

This suggested that records need to be examined in relation to multiple dimensions, including country, vaccine, year and target group.

## 2. Target Group Inspection

The distinct values in `target_grp` were examined.

Observed values included:

- births
- SI
- y01 through y14

The exact definitions of these codes still need to be confirmed using official WUENIC documentation.

## 3. Coverage Data Type Inspection

The SQLite `typeof()` function was used to inspect the storage type of `coverage`.

Result:

| Data type | Records |
|---|---:|
| TEXT | 58,291 |

All records in the `coverage` column were reported as TEXT.

Numerical conversion is therefore required when performing numerical calculations.

## 4. Missing Value Inspection

The coverage column was checked for:

- NULL values
- Empty strings
- Common missing-value representations such as NA, N/A and -

No records matching these conditions were returned.

A separate NULL check also returned zero records.

This does not establish that every coverage value is valid or accurate.

## 5. Unexpected Character Inspection

The following query was used:

```sql
SELECT DISTINCT coverage
FROM wuenic_clean
WHERE TRIM(coverage) GLOB '*[^0-9.]*';
```

No rows were returned.

This indicates that no coverage values containing characters outside digits and decimal points were identified by this check.

An additional check was performed to identify values containing no digits:

```sql
SELECT DISTINCT coverage
FROM wuenic_clean
WHERE TRIM(coverage) NOT GLOB '*[0-9]*';
```

No rows were returned.

This indicates that every distinct coverage value contains at least one digit.

## 6. Multiple Decimal Point Inspection

The following query was used:

```sql
SELECT DISTINCT coverage
FROM wuenic_clean
WHERE coverage GLOB '*.*.*';
```

No rows were returned.

No coverage values containing two or more decimal points were identified.

## 7. Coverage Range Inspection

The following query was used:

```sql
SELECT DISTINCT coverage
FROM wuenic_clean
WHERE CAST(coverage AS REAL) < 0
   OR CAST(coverage AS REAL) > 100;
```

No rows were returned.

No values below 0 or above 100 were identified.

## 8. Coverage Value Distribution

The distinct coverage values and their record counts were examined using:

```sql
SELECT coverage, COUNT(*) AS records
FROM wuenic_clean
GROUP BY coverage
ORDER BY CAST(coverage AS REAL);
```

The value 0 appeared in 49 records.

The query also returned other coverage values and their corresponding record counts.

## 9. Preliminary Numerical Summary

The following numerical summary was obtained using `CAST()`:

| Measurement | Result |
|---|---:|
| Minimum coverage | 0 |
| Maximum coverage | 99 |
| Average coverage | Approximately 83.3428 |

These are preliminary overall summaries across the 58,291 records.

They should not yet be interpreted as final analytical findings.

## 10. Outstanding Checks

- Verify WUENIC variable definitions and target group codes using official documentation.
- Inspect the remaining variables for missing values and inconsistencies.
- Examine the dataset's structure and record uniqueness.
- Continue validating the data before conducting detailed analysis.

## 11. Data Quality Assessment Status

The initial inspection of the `coverage` column found no NULL values, unexpected characters identified by the checks performed, multiple decimal points or values outside the 0–100 range.

Further checks are still required before considering the dataset fully validated.
