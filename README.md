<div align="center">

# 🗄️ Week 12 Lab — Big Data Tools
### SQL · APIs · PySpark
**AUCA · Introduction to Big Data Analytics · November 2026**

---

![Python](https://img.shields.io/badge/Python-3.14-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-4.3.0-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-momo.db-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete%20✓-2ea44f?style=for-the-badge)

**Student:** Godson Mugisha &nbsp;|&nbsp; **Group D** &nbsp;|&nbsp; **Score target:** 35 + 3 bonus

</div>

---

## 📁 Repository Contents

```
week 12/
├── 📓 Week12_GodsonMugisha.ipynb   ← main notebook (all 5 parts)
├── 📊 week12_transactions.csv      ← 2,000 MoMo transactions, Nov 2026
├── 🌤️  week12_api_sample.json       ← offline fallback for Open-Meteo API
├── 🗃️  momo.db                      ← SQLite database (auto-generated)
├── 📄 Lab 6.pdf                    ← original lab instructions
└── 📸 Screenshots/                 ← 23 output screenshots (1–23.png)
```

---

## 🗂️ Dataset at a Glance

| Field | Detail |
|---|---|
| **File** | `week12_transactions.csv` |
| **Rows** | 2,000 mobile-money transactions |
| **Period** | November 2026 |
| **Total value** | 119.9M RWF |
| **Columns** | `txn_id` · `date` · `hour` · `district` · `txn_type` · `amount_rwf` · `flagged` |
| **Transaction types** | send · receive · paybill · airtime |
| **Districts** | Gasabo · Nyarugenge · Muhanga · Huye · Kicukiro · Musanze · Rubavu · Nyagatare |

---

## ✅ Results Summary

### Part 1 — SQL with Python *(12 / 12 marks)*

<details>
<summary><b>Exercise 1.1 — SELECT / WHERE / ORDER BY</b></summary>

**1.1 (a)** Top-5 `send` transactions above 100,000 RWF:

| txn_id | district | amount_rwf |
|--------|----------|-----------|
| T01708 | Kicukiro | 140,000 |
| T00364 | Huye | 139,000 |
| T00895 | Muhanga | 139,000 |
| T00899 | Nyarugenge | 139,000 |
| T01314 | Gasabo | 139,000 |

**1.1 (b)** Flagged transactions: **11** ✓ *(checkpoint passed)*

📸 Screenshots: `3.png` · `4.png`

</details>

<details>
<summary><b>Exercise 1.2 — GROUP BY</b></summary>

**1.2 (a)** Amount per transaction type:

| txn_type | n_transactions | total_rwf | avg_rwf |
|----------|---------------|-----------|---------|
| **send** | 868 | **62,049,000** | 71,485 |
| paybill | 330 | 29,970,000 | 90,818 |
| receive | 489 | 27,024,000 | 55,264 |
| airtime | 313 | 833,000 | 2,661 |

→ `send` moves the most money: **62.0M RWF** ✓

**1.2 (b)** Leading district: **Muhanga** with **21.08M RWF** ✓ *(checkpoint passed)*

📸 Screenshots: `5.png` · `6.png`

</details>

<details>
<summary><b>Exercise 1.3 — Busiest Hour</b></summary>

Both SQL and Pandas independently found **hour 20** as the busiest with **185 transactions**.

```
SQL result  → hour 20: 185 transactions ✓
Pandas result → hour 20: 185 transactions ✓
assert passed — both engines agree!
```

📸 Screenshots: `7.png` · `8.png`

</details>

---

### Part 2 — APIs & JSON *(10 / 10 marks)*

<details>
<summary><b>Exercise 2.1 — JSON by Hand</b></summary>

Parsed the JSON string `{"city": "Kigali", "readings": [21.5, 24.0, 26.2, 23.8, 22.1]}`:

```
(a) City:              Kigali
(b) Number of readings: 5
(c) Average temperature: 23.52 °C
```

📸 Screenshot: `9.png`

</details>

<details>
<summary><b>Exercise 2.2 — Live API Call (Open-Meteo)</b></summary>

The live API call to Open-Meteo **succeeded** (HTTP 200) — no fallback needed.

```
(a) Hourly readings:  24
(b) Max temperature:  26.2 °C
    Min temperature:  14.9 °C
(c) Hottest hour:     2026-11-25T12:00
```

📸 Screenshot: `10.png`

</details>

<details>
<summary><b>Exercise 2.3 — API Data Meets the Toolbox</b></summary>

Weather DataFrame built from live API data (2026-11-25, Kigali):

```
Temperature statistics (temp_c):
  count   24.00
  mean    19.94 °C
  std      3.74
  min     14.90 °C
  25%     17.08
  50%     19.35
  75%     22.85
  max     26.20 °C

Hours ≥ 25 °C:  3  (10:00 → 25.5°C, 11:00 → 25.9°C, 12:00 → 26.2°C)
```

📸 Screenshots: `11.png` · `12.png` · `13.png`

</details>

---

### Part 3 — PySpark *(10 / 10 marks)*

> **Spark version: 4.3.0** · Running in local mode on `mug1sha-HP-EliteBook-840-G3`

<details>
<summary><b>Exercise 3.1 — Load & Inspect</b></summary>

```
Row count: 2,000

Schema:
root
 |-- txn_id:     string  (nullable = true)
 |-- date:       date    (nullable = true)
 |-- hour:       integer (nullable = true)
 |-- district:   string  (nullable = true)
 |-- txn_type:   string  (nullable = true)
 |-- amount_rwf: integer (nullable = true)
 |-- flagged:    integer (nullable = true)
```

📸 Screenshots: `14.png` · `15.png` · `16.png`

</details>

<details>
<summary><b>Exercise 3.2 — Filter</b></summary>

```
Transactions above 100,000 RWF:  451  ✓  (checkpoint passed)
```

5 flagged transactions shown (all send type, amounts 127,000–134,000 RWF across Muhanga, Huye, Gasabo, Rubavu).

📸 Screenshots: `17.png` · `18.png`

</details>

<details>
<summary><b>Exercise 3.3 — groupBy</b></summary>

Spark totals per `txn_type`:

```
+--------+---------------+
|txn_type|sum(amount_rwf)|
+--------+---------------+
|    send|       62049000|
| paybill|       29970000|
| receive|       27024000|
| airtime|         833000|
+--------+---------------+
```

Spark totals per district (→ Pandas): Muhanga leads at **21.08M RWF** — identical to SQL.

**SQL vs Spark verification:**
```
SQL   send total  (send):    62,049,000
Spark send total  (send):    62,049,000  ✓

SQL   district total (Muhanga): 21,076,000
Spark district total (Muhanga): 21,076,000  ✓

✓ Spark and SQL answers match!
```

📸 Screenshots: `19.png` · `20.png` · `21.png`

</details>

<details>
<summary><b>🏆 Bonus — Spark Speaks SQL (+3 marks)</b></summary>

Registered `sdf` as a temp view and ran the identical Part 1.2(a) `GROUP BY` query via `spark.sql()` — zero modifications:

```
+--------+-------------+---------+-------+
|txn_type|n_transactions|total_rwf|avg_rwf|
+--------+-------------+---------+-------+
|    send|          868| 62049000|71485.0|
| paybill|          330| 29970000|90818.0|
| receive|          489| 27024000|55264.0|
| airtime|          313|   833000| 2661.0|
+--------+-------------+---------+-------+
```

> **Why is this powerful?** SQL is the question language; the engine underneath can be anything — a 1 GB SQLite file on a laptop today, or a 100 TB Spark cluster tomorrow. You write the query once and the right engine runs it at whatever scale is needed. Skills transfer completely.

📸 Screenshot: `22.png`

</details>

---

### Part 4 — Choose Your Weapon *(3 / 3 marks)*

| # | Scenario | Tool | One-sentence justification |
|---|---|:---:|---|
| 1 | Ministry of Health PostgreSQL — monthly totals per district | **SQL** | The data already lives in a relational database, so SQL queries it in place without moving it. |
| 2 | 40,000-row CSV — cleaning, 3 charts, regression | **Pandas** | 40k rows fits in memory, and Pandas integrates directly with Matplotlib/Plotly and Scikit-learn in one workflow. |
| 3 | Telecom nationwide call records — 40 TB on cluster | **Spark** | 40 TB will not fit in any single machine's RAM; Spark distributes the computation across the cluster where the data already lives. |
| 4 | Dashboard showing today's weather, always live | **API** | A weather API (like Open-Meteo) delivers the current forecast on demand, keeping the dashboard live without manual uploads. |

📸 Screenshot: `23.png`

---

### Part 5 — Final-Project Proposal *(Group D)*

> **Road Accident Risk Analysis in Rwanda**
>
> Our group will investigate what patterns are associated with road accidents in Rwanda and whether available data can predict accident severity or risk. We will use a road-safety dataset of at least 500 real accident records — preferably sourced from the Rwanda National Police or a comparable open road-safety repository — containing attributes such as location, time of day, road and weather conditions, vehicle type, and injury outcome. Road accidents are a leading cause of preventable death and injury in Rwanda and across sub-Saharan Africa, so understanding when and where severe accidents cluster can directly inform traffic enforcement, infrastructure investment, and public-awareness campaigns. We plan to train a **classification model** (starting with logistic regression and comparing with a random forest) to predict accident severity class, evaluated using a confusion matrix, recall, precision, and F1-score, because the target is categorical and class imbalance between minor and fatal incidents makes recall on the severe class the most critical metric. Findings will be communicated through an interactive Plotly dashboard highlighting risk patterns and practical road-safety recommendations.

---

## 📸 Screenshot Index

| # | What it shows |
|---|---|
| `1.png` | Setup — libraries imported ✓ |
| `2.png` | CSV → SQLite: 2,000 rows loaded, first 5 rows preview |
| `3.png` | Ex 1.1(a) — Top-5 send transactions > 100k RWF |
| `4.png` | Ex 1.1(b) — Flagged count = 11 ✓ |
| `5.png` | Ex 1.2(a) — Per-type totals; send leads at 62.0M RWF ✓ |
| `6.png` | Ex 1.2(b) — Per-district totals; Muhanga leads at 21.08M ✓ |
| `7.png` | Ex 1.3(a) — SQL busiest hour: hour 20, 185 transactions |
| `8.png` | Ex 1.3(b) — Pandas confirms hour 20, 185 transactions ✓ |
| `9.png` | Ex 2.1 — JSON parsed: Kigali, 5 readings, avg 23.52 °C |
| `10.png` | Ex 2.2 — Live API (HTTP 200): max 26.2°C, min 14.9°C, hottest 12:00 |
| `11.png` | Ex 2.3 — Weather DataFrame first 5 rows |
| `12.png` | Ex 2.3(a) — temperature describe() stats |
| `13.png` | Ex 2.3(b) — 3 hours ≥ 25 °C (live data) |
| `14.png` | Spark setup — SparkSession ready, version 4.3.0 |
| `15.png` | Ex 3.1 — Row count 2,000 and schema |
| `16.png` | Ex 3.1 — First 5 rows via sdf.show(5) |
| `17.png` | Ex 3.2(a) — 451 transactions above 100k RWF ✓ |
| `18.png` | Ex 3.2(b) — 5 flagged transactions |
| `19.png` | Ex 3.3(a) — groupBy txn_type totals (Spark) |
| `20.png` | Ex 3.3(b) — Per-district totals Spark → Pandas |
| `21.png` | Ex 3.3(c) — SQL vs Spark totals match exactly ✓ |
| `22.png` | Bonus — spark.sql() runs Part 1.2 query unchanged |
| `23.png` | Part 4 — Choose Your Weapon table |

---

## ☑️ Submission Checklist

- [x] Notebook renamed `Week12_GodsonMugisha.ipynb`
- [x] All cells executed with output saved
- [x] SQL and Spark answers agree — verified in cell [21]
- [x] API exercise ran live (HTTP 200, no fallback needed)
- [x] Part 4 scenarios answered with justifications
- [x] Part 5 group project proposal written (Group D)
- [x] 23 screenshots captured
- [x] README written

---

## 🔑 Key Findings

| Finding | Value |
|---|---|
| Total transactions | 2,000 |
| Total value | ~119.9M RWF |
| Flagged transactions | **11** |
| Top transaction type (by volume) | **send** — 62.0M RWF (868 transactions) |
| Leading district | **Muhanga** — 21.08M RWF |
| Busiest hour | **Hour 20** — 185 transactions |
| Transactions above 100k RWF | **451** |
| Kigali max temp (2026-11-25) | **26.2 °C** at 12:00 |
| Spark version | **4.3.0** |

---

<div align="center">

**AUCA · Introduction to Big Data Analytics**
Instructor: Dr Prince Ishimwe · [Prince.ishimwe@auca.ac.rw](mailto:Prince.ishimwe@auca.ac.rw)

*Week 12 Lab · November 2026*

</div>
