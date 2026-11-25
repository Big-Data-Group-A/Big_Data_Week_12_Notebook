# Week 12 Lab: Big Data Tools (SQL, APIs, PySpark)

**Course:** Introduction to Big Data Analytics, AUCA
**Name:** ____Aimable Ndizeye________  **Student ID:** ____29369________ 

## Overview
This lab analyses 2,000 mobile-money transactions from Rwanda (November 2026, total value 119.9M RWF) with three tools: SQL (SQLite), a web API (Open-Meteo, with an offline JSON fallback) and PySpark.

## Files
| File | Purpose |
|---|---|
| `Week12_YourName.ipynb` | The completed notebook (rename with your own name) |
| `week12_transactions.csv` | Dataset: `txn_id, date, hour, district, txn_type, amount_rwf, flagged` |
| `week12_api_sample.json` | Offline copy of Kigali's 24-hour forecast |
| `momo.db` | SQLite database created by the notebook (table `transactions`) |

## How to run
1. Put the notebook, the CSV and the JSON file in the same folder.
2. On Colab, run `!pip install pyspark` in the first cell (2-3 minutes). Locally, install Java and run `pip install pyspark`.
3. Run all cells from top to bottom. Spark startup warnings are normal.

## Results
| Exercise | Result |
|---|---|
| 1.1 Flagged transactions | 11 |
| 1.2 Top transaction type | `send`, about 62.0M RWF |
| 1.2 Top district | Muhanga, about 21.1M RWF |
| 1.3 Busiest hour | Hour 20, 185 transactions (SQL and Pandas agree) |
| 3.2 Transactions above 100,000 RWF | 451 |
| 3.3 Spark vs SQL | Totals per type and per district are identical |
| 2.2 Hottest hour (sample file) | 27.9 °C at 2026-11-22T14:00; minimum 15.9 °C; 24 hours |
| 2.3 Hours at or above 25 °C | 7 |

## Structure
- **Part 1, SQL:** `SELECT/WHERE/ORDER BY`, `GROUP BY` and hourly counts through `pd.read_sql`.
- **Part 2, APIs and JSON:** `json.loads`, the Open-Meteo call with a `try/except` fallback to the sample file, and a Pandas DataFrame of time and temperature.
- **Part 3, PySpark:** load the CSV, filter, `groupBy`, and the bonus (`createOrReplaceTempView` with the same SQL).
- **Part 4, Choose your weapon:** SQL, Pandas, Spark and API, each with a one-sentence justification.
- **Part 5, Project proposal:** a one-paragraph draft for the final project (classification of flagged transactions).

## Notes
- The dates in the original xlsx are Excel serial numbers. The CSV stores them as `YYYY-MM-DD`.
- Exercise 2.1 uses the JSON string from the original lab notebook. Check that it is pasted in.
- Live API data changes daily, so Part 2 numbers differ from the sample file when the live call succeeds.
