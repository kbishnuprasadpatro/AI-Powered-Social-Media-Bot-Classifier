# AI-Powered Social Media Bot Classifier

A practical, resume-ready project that combines **machine learning for bot detection** with a **Twitter (X) data collection notebook**. 
It includes:
- `model-2.ipynb` — trains a baseline ML model (Logistic Regression) to classify accounts as **bot vs. human**.
- `tweet-bot.ipynb` — uses Selenium to **log in to X**, navigate to a profile, and scrape **tweet-level engagement signals** for feature building.

> This README is auto-generated from the structure and code found in the two uploaded notebooks.

---

## 🚀 Quick Start

### 1) Environment
- Python 3.9+
- Recommended: create and activate a virtual environment.

```bash
python -m venv .venv
# Windows
.venv\\Scripts\\activate
# macOS/Linux
source .venv/bin/activate
```

### 2) Install dependencies
Create a `requirements.txt` with the following (or use the list below directly):

```
pandas
scikit-learn
numpy
notebook
selenium
beautifulsoup4
webdriver-manager
```

Then install:

```bash
pip install -r requirements.txt
```

### 3) Start Jupyter
```bash
jupyter notebook
```
Open the notebooks:
- `model-2.ipynb`
- `tweet-bot.ipynb`

---

## 📁 Project Structure

```
.
├─ model-2.ipynb        # ML pipeline: data prep → baseline LogisticRegression model
├─ tweet-bot.ipynb      # Selenium-based X (Twitter) data collector
└─ README.md            # You are here
```

---

## 🧠 Bot Detection (model-2.ipynb)

This notebook builds a simple, clear ML baseline:

- **Libraries**: `pandas`, `scikit-learn`
- **Target**: `'Bot Label'` (binary classification)
- **Sample Features** (from the notebook code): categorical columns like `account_type` (one-hot encoded), and other numeric features; test-time `Username` is dropped.
- **Preprocessing**:
  - Handle missing values with column means for numeric fields.
  - One-hot encode categorical columns like `account_type`.
  - Align train/test columns (add missing dummy columns to test; order to match train).
- **Model**: `LogisticRegression` as a strong, explainable baseline.

### Expected input files
The notebook expects **two tabular datasets**:
- A training dataset (e.g., `train.csv`) including the target column `'Bot Label'`.
- A testing/holdout dataset (e.g., `test.csv`) without the target (or with it kept aside).

> If your files are named differently, just update the file paths in the first cell.

### Training flow (high level)
1. Load train and test data with `pandas`.
2. Clean and impute missing values.
3. One-hot encode categorical columns and align columns across train/test.
4. Split into `X_train`/`y_train` and `X_test` (target: `'Bot Label'`).
5. Fit `LogisticRegression`.
6. Generate predictions on the test set.

### Tips
- Try stronger models next (e.g., `RandomForestClassifier`, `XGBClassifier`) and compare with cross‑validation.
- Add feature importance or model coefficients to explain *why* an account was flagged as a bot.

---

## 🔎 Twitter Data Collector (tweet-bot.ipynb)

This notebook automates a lightweight crawl on X (Twitter) using **Selenium** and **BeautifulSoup** to fetch profile/tweet-level signals.

### What it does
- Opens `https://twitter.com/login` and performs a login.
- Searches for a subject keyword (e.g., `"virat kohali"` in the sample code) and clicks **People**.
- Opens the first profile result.
- Collects tweets from the profile feed and parses **reply**, **retweet**, and **like** counters (via `data-testid` attributes).

### Configure credentials
**Important:** Do not hardcode credentials. Use environment variables, a `.env` file, or prompt securely at runtime.

Example (environment variables):
```python
import os
TWITTER_USERNAME = os.getenv("TWITTER_USERNAME")
TWITTER_PASSWORD = os.getenv("TWITTER_PASSWORD")
```

### WebDriver setup
The notebook is compatible with `webdriver-manager`, which installs and manages your browser driver automatically:
```python
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager}
from selenium.webdriver.chrome.service import Service

service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service)
```

### Output
- A list of tweets and their engagement metrics (replies/retweets/likes) that you can export to CSV and later feed into your ML pipeline as features.

> Note: X's markup changes frequently. You may need to update XPaths and `data-testid` selectors over time.

### Legal & Ethical Notes
- Respect X (Twitter) **Terms of Service**.
- Scrape only what you’re allowed to access.
- Avoid aggressive crawling; add `sleep` and reasonable limits.
- Store credentials securely. Never commit them to Git.

---

## 🧪 Example: Export collected tweets to CSV

Add something like this near the end of `tweet-bot.ipynb` once you’ve built a list of items:
```python
import pandas as pd

rows = []
# ...populate rows as dicts: {"tweet_id": ..., "text": ..., "replies": ..., "retweets": ..., "likes": ...}
df = pd.DataFrame(rows)
df.to_csv("tweets_sample.csv", index=False)
```

You can then join or engineer features and run them through the model notebook.

---

## 📊 Evaluation (suggested)

In `model-2.ipynb`, add:
- Train/validation split.
- Metrics: accuracy, precision, recall, F1, ROC‑AUC.
- Confusion matrix and ROC curve.

This helps you present results convincingly on your resume and GitHub.

---

## ✅ How to Run (summary)

1. Prepare `train.csv` and `test.csv` (ensure `'Bot Label'` is present in train).
2. Open `model-2.ipynb`, run all cells to train and generate predictions.
3. Open `tweet-bot.ipynb`, log in with environment variables, set your **search subject**, run cells to collect tweets and export to CSV.
4. (Optional) Merge collected features into your training data and iterate on the model.

---

## 🧱 Roadmap

- [ ] Add cross‑validation and model comparison (LogReg vs. RF vs. XGBoost).
- [ ] Persist a trained model with `joblib` and add a `predict.py` script.
- [ ] Dockerfile for reproducible runs.
- [ ] CI checks (linting, tests).
- [ ] Replace brittle XPaths with more robust locators.
- [ ] Add `requirements.txt` and `environment.yml` to repo.

---

## 🔐 Security

- Use secrets managers or `.env` (git‑ignored) for credentials.
- Never push exported data that includes private information.

---

## 📜 License

Choose a license (MIT is common for academic/demo projects). Create a `LICENSE` file in the repo root.

---

## 🤝 Acknowledgements

- `scikit-learn` for modeling.
- `selenium` and `webdriver-manager` for browser automation.
- `beautifulsoup4` for HTML parsing.
