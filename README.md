# Customer Pulse

A Flask-based financial customer happiness POC with admin login, VADER sentiment analysis, SQLite storage, responsive dashboard, review search and filters, CSV upload/export, and category insights.

## Run locally (Windows)

Use Python 3.10 or newer. In PowerShell from the project folder:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
$env:SECRET_KEY = "replace-with-a-long-random-secret"
.\.venv\Scripts\python.exe app.py
```

Open `http://127.0.0.1:5000`. First choose **Create an account**, register a username and password, then return to **Sign in**. New accounts are Analysts; the demo admin account is `admin` / `admin123` and can manage sample data. Passwords for registered accounts are stored as hashes. SQLite is stored at `instance/customer_happiness.sqlite3`; the 46 sample reviews load automatically when the database is empty.

Run tests:

```powershell
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

## Features

- Dashboard: CHI, total reviews, positive/neutral/negative counts, sentiment doughnut, category bars, and 7-day trend.
- Customer reviews: SQLite-backed table, text search, date/source/category/sentiment filters, filtered CSV export.
- Analyze feedback: manual review entry and CSV bulk import with VADER sentiment and keyword categories.
- Insights & reports: top issues, positive drivers, category-wise CHI, trend chart, report export, sample reload, and delete-all controls.
- VADER compound score is displayed on its original −1 to +1 scale. Thresholds: positive `>= 0.05`, neutral between thresholds, negative `<= -0.05`.
- CHI: `(positive + 0.5 * neutral) / total * 100`, where positive reviews earn 100 points, neutral 50, and negative 0.

## GitHub, Render, and Netlify

Push this project to GitHub. Render runs the Flask server using the included `render.yaml`; Netlify cannot run Flask directly. To provide the teacher with a Netlify URL, `netlify.toml` proxies the Netlify site to the Render service. People can create Analyst accounts, while the configured demo admin retains the sample-data controls.

1. Push the repository to GitHub.
2. In Render, create a Blueprint from the repository. Set a strong `ADMIN_PASSWORD`; Render generates `SECRET_KEY` from the blueprint settings.
3. Copy the live Render service URL. Replace `https://YOUR-RENDER-SERVICE.onrender.com` in `netlify.toml` with that URL, then push the change to GitHub.
4. In Netlify, import the same GitHub repository. Netlify reads `netlify.toml`, deploys the proxy, and gives you the public `*.netlify.app` link to share.

The included Render configuration attaches a 1 GB persistent disk to the SQLite `instance` directory, so accounts and reviews survive restarts and redeploys. Persistent disks require Render's paid `0.5c-512mb` plan and disable zero-downtime deploys; review Render's current pricing before creating the service. The disk is persistence, not a backup, so export important reports regularly. GitHub stores the code; it does not host the running Flask app. Netlify is the public link/proxy, while Render runs Python and SQLite.

## Demo limitations

Authentication is POC-level, without 2FA or banking-grade protections. VADER is for English and may misread sarcasm, Urdu, or Roman Urdu. Categories are keyword-based (App, Loans, Fees, Cards, Payments, Branch, Service, Accounts); unmatched reviews become General. Use synthetic or anonymized feedback, not real customer financial information.