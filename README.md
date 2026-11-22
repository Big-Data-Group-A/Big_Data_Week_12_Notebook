# Week 12 Lab — Big Data Tools: SQL · APIs · PySpark

AUCA · Introduction to Big Data Analytics · Instructor: Prince Ishimwe
Total: 35 marks (+3 bonus) · Due: end of Week 12

This repository holds everything you need for the Week 12 lab. Read this whole README **before** you start. It explains what is in the repo, how to set up, how to work on each part, and — most importantly — **how to submit using your own branch (no Pull Requests).**

---

## 1. What is in this repository

| File | Purpose |
|---|---|
| `Lab 6.pdf` | The lab handout: all exercises, marks and checkpoints. |
| `week12_transactions.csv` | 2,000 mobile-money transactions, November 2026 (119.9M RWF total). |
| `week12_transactions.xlsx` | The same data as Excel (reference only; the notebook uses the CSV). |
| `week12_api_sample.json` | Offline copy of Kigali's 24-hour weather forecast (Open-Meteo format). Used when the live API is unavailable. |
| `README.md` | This guide. |

### The dataset

`week12_transactions.csv` columns:

| Column | Meaning |
|---|---|
| `txn_id` | Transaction ID (`T00001` …) |
| `date` | Date, `YYYY-MM-DD` |
| `hour` | Hour of day, 0–23 |
| `district` | District name |
| `txn_type` | `send`, `receive`, `paybill` or `airtime` |
| `amount_rwf` | Amount in Rwandan francs |
| `flagged` | 1 if flagged, else 0 |

### The API sample

`week12_api_sample.json` follows the Open-Meteo response format. The values you need are:

- `data["hourly"]["time"]` — list of 24 hourly timestamps
- `data["hourly"]["temperature_2m"]` — list of 24 temperatures in °C

---

## 2. Rules (read carefully)

1. **Never push to `main`.** `main` holds the original lab files only.
2. **Create your own branch** and push **only** to that branch.
3. **Do NOT open a Pull Request.** Your instructor will review your branch directly.
4. Do not edit or delete the provided data files. Add your work alongside them.
5. Name your notebook **`Week12_YourName.ipynb`** (for example `Week12_Jane_Doe.ipynb`).

### Branch naming

Use this format: `firstname-lastname` (lowercase, hyphens), for example:

```
jane-doe
```

---

## 3. Step-by-step workflow

### Step 1 — Clone the repository

```bash
git clone https://github.com/Big-Data-Group-A/Big_Data_Week_12_Notebook.git
cd Big_Data_Week_12_Notebook
```

### Step 2 — Create and switch to your own branch

```bash
git checkout -b firstname-lastname
```

Confirm you are on your branch (it should show a `*` next to it):

```bash
git branch
```

### Step 3 — Set up your notebook

**Option A: Google Colab (recommended)**

1. Go to <https://colab.research.google.com> and create a new notebook.
2. Rename it `Week12_YourName`.
3. Upload `week12_transactions.csv` and `week12_api_sample.json` using the Files panel on the left.
4. In the **first cell**, install PySpark (takes 2–3 minutes):

   ```python
   !pip install pyspark
   ```

5. When finished, use **File → Download → Download .ipynb** and move the file into your cloned repo folder.

**Option B: Local Jupyter**

```bash
pip install pandas pyspark jupyter requests
jupyter notebook
```

Create `Week12_YourName.ipynb` inside the cloned folder, so the data files sit next to it.

### Step 4 — Do the lab

Work through the handout (`Lab 6.pdf`) in order. Summary:

#### Part 1 — SQL with Python (12 marks)

Load the CSV into SQLite (`momo.db`, table `transactions`) and query with `pd.read_sql(query, conn)`.

```python
import sqlite3, pandas as pd

df = pd.read_csv("week12_transactions.csv")
conn = sqlite3.connect("momo.db")
df.to_sql("transactions", conn, if_exists="replace", index=False)
```

| Exercise | Task | Checkpoint |
|---|---|---|
| 1.1 | 5 largest `send` transactions above 100,000 RWF; count flagged | **11 flagged** |
| 1.2 | `COUNT`, `SUM`, `AVG` per `txn_type`; total per district | `send` ≈ **62.0M**; **Muhanga** leads ≈ **21.1M** |
| 1.3 | Transactions per hour in SQL, then in Pandas; confirm both agree | **Hour 20**, **185** transactions |

#### Part 2 — APIs & JSON (10 marks)

| Exercise | Task |
|---|---|
| 2.1 | Parse the provided JSON string with `json.loads`; print city, number of readings, average (2 decimals). |
| 2.2 | Fetch Kigali's forecast (falls back to `week12_api_sample.json` offline). Print hours count, max/min temperature, and the **time** of the hottest hour using `.index(max(...))`. |
| 2.3 | Build a DataFrame with `time` and `temp_c`; `describe()` it; count hours ≥ 25 °C with a mask. |

Expected results from the sample file (when running offline): 24 hours, max **27.9 °C**, min **15.9 °C**, hottest at **`2026-11-22T14:00`**, **7** hours ≥ 25 °C.

Offline fallback pattern:

```python
import json, requests

try:
    url = ("https://api.open-meteo.com/v1/forecast"
           "?latitude=-1.9441&longitude=30.0619"
           "&hourly=temperature_2m&forecast_days=1&timezone=Africa%2FKigali")
    data = requests.get(url, timeout=10).json()
except Exception:
    with open("week12_api_sample.json") as f:
        data = json.load(f)

times = data["hourly"]["time"]
temps = data["hourly"]["temperature_2m"]
```

#### Part 3 — PySpark (10 marks)

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("week12").master("local[*]").getOrCreate()
sdf = spark.read.csv("week12_transactions.csv", header=True, inferSchema=True)
```

| Exercise | Task | Checkpoint |
|---|---|---|
| 3.1 | Row count, schema, first 5 rows | 2,000 rows |
| 3.2 | Count transactions > 100,000 RWF; show 5 flagged | **451** above 100k |
| 3.3 | Total per `txn_type` (`groupBy().sum()`); per-district totals via `.toPandas()`; comment on whether Spark matches SQL | Must match Part 1 |
| Bonus (+3) | `createOrReplaceTempView("transactions")` and run your Part 1 `GROUP BY` unchanged with `spark.sql(...)`; comment on why the same SQL running on SQLite and a cluster is powerful | |

Startup warnings from Spark are normal.

#### Part 4 — Choose Your Weapon (3 marks, written)

For each of the 4 scenarios in the handout, name the **first** tool (Pandas / SQL / Spark / API) and justify in **one sentence**. Write your answers in a Markdown cell.

#### Part 5 — Final-Project Proposal (required)

Per **group**, one paragraph covering: the question, the dataset (source and rough size, minimum 500 rows), why it matters, and the planned model (regression or classification, and why). Save it as `Proposal_GroupName.md` in your branch (one person per group commits it to their branch, naming the group members inside).

### Step 5 — Add screenshots

Take screenshots of your key outputs (SQL results, API results, Spark results, checkpoints) and save them in a folder called `screenshots/` in the repo.

### Step 6 — Final checklist before you commit

- [ ] Notebook named `Week12_YourName.ipynb`
- [ ] Notebook runs top to bottom without errors (**Restart & Run All**)
- [ ] SQL and Spark answers agree
- [ ] API exercise ran (live or offline)
- [ ] Part 4 scenarios answered
- [ ] Project proposal written
- [ ] Screenshots saved in `screenshots/`
- [ ] You are on **your own branch**, not `main`

---

## 4. Submitting: commit and push to YOUR branch

### Step 7 — Check what changed

```bash
git status
```

### Step 8 — Stage your files

Stage only your own work (avoid adding `momo.db` or Spark junk):

```bash
git add Week12_YourName.ipynb screenshots/ Proposal_GroupName.md
```

### Step 9 — Commit

```bash
git commit -m "Week 12 lab submission - Firstname Lastname"
```

### Step 10 — Push to your branch

The first time, set the upstream:

```bash
git push -u origin firstname-lastname
```

Afterwards, `git push` is enough.

### Step 11 — Verify

Open the repository on GitHub, use the **branch dropdown** to switch to your branch, and confirm your notebook and screenshots are there.

> ⚠️ **Do not click "Compare & pull request"** on the GitHub banner. Pull Requests are not part of this submission.

---

## 5. Updating your submission

If you fix something before the deadline, repeat steps 7–10 on the same branch. Your latest push counts.

---

## 6. Troubleshooting

| Problem | Fix |
|---|---|
| `git push` is rejected for `main` | You are on `main`. Run `git checkout -b firstname-lastname`, then `git push -u origin firstname-lastname`. |
| Committed on `main` by mistake (not pushed) | `git branch firstname-lastname` then `git reset --hard origin/main` then `git checkout firstname-lastname`. |
| Authentication failed | GitHub no longer accepts account passwords. Use a [Personal Access Token](https://github.com/settings/tokens) as the password, or set up SSH keys. |
| `Permission denied` / `403` | Ask the instructor to add you as a collaborator on the repository. |
| `pyspark` not found | Run `!pip install pyspark` in the first Colab cell and wait for it to finish. |
| Spark `Java` errors locally | Install Java 11 or 17 (JDK), then restart the notebook. |
| `FileNotFoundError` for the CSV/JSON | The data files must be in the **same folder** as the notebook (in Colab, upload them via the Files panel). |
| Live API fails | Expected offline. The `try/except` falls back to `week12_api_sample.json`. |
| Unsure which branch you are on | Run `git branch`; the current one has a `*`. |

---

## 7. Marking scheme

| Part | Marks |
|---|---|
| Part 1 — SQL with Python | 12 |
| Part 2 — APIs & JSON | 10 |
| Part 3 — PySpark | 10 |
| Part 4 — Choose Your Weapon | 3 |
| **Total** | **35** (+3 bonus) |

Questions: Prince.ishimwe@auca.ac.rw
