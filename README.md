# Anomaly Detection on Internal Audit Transaction Logs

 **ML Engineer Track**

100% free and open-source tools only — no paid APIs, no credit card, runs on Google Colab's free tier.

---

## 📌 Overview

Forensic auditors need to surface the small number of genuinely unusual transactions hidden inside thousands of routine ones. Build an unsupervised Isolation Forest anomaly detector over engineered transaction features, with no labeled fraud data required -- entirely free, open-source scikit-learn.

**Domain:** Forensic Analytics / Internal Audit (Deloitte-style)

---

## 📊 Dataset

Any transaction log CSV (auto-generates sample data if file is missing)

**Source:** [https://www.kaggle.com/datasets/webdevbadger/journal-entry-testing-dataset](https://www.kaggle.com/datasets/webdevbadger/journal-entry-testing-dataset)

> This script also includes a **safe fallback**: if the real dataset file isn't found next to the
> notebook/script, it automatically generates a small realistic sample dataset with the same column
> names, so the whole pipeline still runs end-to-end even before you've downloaded the real data.

---

## 🛠️ Tech Stack

Python 3 | scikit-learn (Isolation Forest, PCA) | pandas | matplotlib

**Skills demonstrated:** Python, scikit-learn (Isolation Forest), pandas, Feature Engineering

---

## 🎯 What This Project Builds

- Feature engineering: transaction hour, amount z-score by user, days since user's last transaction
- An Isolation Forest model trained unsupervised (no labels needed) to score anomaly likelihood
- A contamination-rate tuning step to control what % of transactions get flagged
- A per-user behavioral baseline so 'unusual for this user' is caught, not just 'unusual overall'
- A ranked anomaly report combining the isolation score with human-readable flag reasons
- A visualization of anomalies in 2D using PCA for a quick visual audit review

---

## 🧭 Step-by-Step Approach

### Step 1: Load Transaction Log (with a Safe Fallback)

**What:** Load transaction_log.csv, or auto-build a realistic sample log if the file isn't found

**Why:** A missing dataset file shouldn't stop the anomaly detector from being testable end-to-end

**How:** if os.path.exists(path): pd.read_csv(path) else: generate synthetic transactions with numpy


### Step 2: Engineer Behavioral Features

**What:** Compute transaction hour, amount z-score per user, and days-since-last-transaction per user

**Why:** Isolation Forest performs far better on engineered behavioral features than on raw transaction columns

**How:** df.groupby('user')['amount'].transform(lambda x:(x-x.mean())/x.std())


### Step 3: Train Isolation Forest

**What:** Fit an unsupervised Isolation Forest on the engineered feature matrix

**Why:** Isolation Forest needs no labels, which matches the reality that most audit logs have no fraud labels

**How:** IsolationForest(contamination=0.02, random_state=42).fit(X)


### Step 4: Rank & Explain Anomalies

**What:** Combine the isolation score with the specific feature(s) that drove the flag into a report

**Why:** A raw anomaly score alone doesn't tell the auditor WHY a transaction was flagged

**How:** Compare each flagged transaction's feature values to the user's own baseline to write the reason


---

## 📈 Dashboard / Reporting Ideas

- Scatter plot: PCA 2D projection with anomalies highlighted in red
- Table: ranked flagged transactions with anomaly_score and flag_reason
- KPI card: total transactions reviewed, % flagged, this period's flag count vs last period
- Bar chart: flagged transaction count by user, to spot any single account driving most flags
- Slider (in Streamlit): adjust contamination rate live and see the flagged count update

---

## 💡 Key Insights

- Isolation Forest's key advantage here is needing zero fraud labels -- most real audit logs have none
- Per-user z-scores catch 'unusual for this specific person' far better than a single global amount threshold
- Contamination rate should be tuned to match the audit team's actual review bandwidth, not left at a default
- Combining the raw anomaly score with a plain-English reason is what makes the output usable by a human auditor
- This is the standard forensic-analytics pattern used across Big-4 style internal audit anomaly detection engagements

---

## 🚀 How to Run

1. Open the `.py` file in **Google Colab** (free tier — no GPU or paid compute needed) or run it locally with Python 3.
2. Install dependencies with the `pip install ...` line at the top of the script (all free, open-source packages).
3. (Optional) Download the real dataset from the Kaggle link above and place it in the same folder — the filename the script expects is noted in the code's data-loading step. If you skip this, the script auto-generates sample data so you can still see it run.
4. Run the script top to bottom. Outputs (charts, CSVs, model files) are saved in the working directory.

```bash
pip install -r requirements.txt   # or the pip install line at the top of the script
python MLEng_08_Audit_Anomaly_Detection_IsolationForest.py
```

---

## 📂 Repo Structure

```
anomaly-detection-on-internal-audit-transaction/
├── MLEng_08_Audit_Anomaly_Detection_IsolationForest.py       # complete, runnable, free-only solution code
├── README.md              # this file
└── outputs/                # charts, CSVs, and model files generated on run
```

---

## ⚠️ Disclaimer

This project is built for educational and portfolio purposes to demonstrate applied ML/quant-risk
skills. It is not financial, credit, or investment advice, and should not be used for real lending,
trading, or compliance decisions without proper review by a licensed professional.

---

*Part of a 20-project AI Engineer + ML Engineer portfolio focused on finance and consulting use cases —
built entirely with free, open-source tools.*
