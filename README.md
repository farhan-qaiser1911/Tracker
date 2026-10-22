# 💸 SpendWise – AI-Powered Personal Expense Tracker

A simple, modern expense tracker built with **Python and Streamlit**. Track income and expenses, set category budgets, and get savings advice from **Google Gemini**.

> ⚠️ **No database, no saved data.** Transactions and budgets live only in your browser session's memory. When the session resets, the page is refreshed, the tab is closed, or the app restarts, **everything is lost** unless you exported a CSV first. See [Data-storage limitation](#data-storage-limitation).

## Features

- **Dashboard** – total income, total expenses, balance, spending for any month, category and monthly charts, recent transactions.
- **Transactions** – add, edit, delete, search, and filter (type, category, date range).
- **Budgets** – monthly limit per expense category with colour-coded progress bars (green → amber → red).
- **AI assistant** – Gemini reviews a summary of your data and suggests ways to save and budget. Suggested budgets can be applied with one click.
- **CSV export / import** – download transactions (and budgets) and restore them later.
- Works **fully without a Gemini key** for basic tracking.

## Tech stack

| Purpose | Tool |
|---|---|
| UI | Streamlit |
| Data handling | Pandas |
| Charts | Plotly |
| AI insights | Google Gemini API (`google-genai` SDK) |
| Storage | `st.session_state` only (no database) |

## Installation

Requires **Python 3.10+**.

```bash
# 1. Put app.py, readme.md and requirements.txt in a folder, then:
cd your-project-folder

# 2. (Recommended) create a virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Run the app
streamlit run app.py
```

The app opens at http://localhost:8501.

## Gemini API configuration (optional)

The AI assistant needs a Gemini API key. Everything else works without one.

1. Create a free key in [Google AI Studio](https://aistudio.google.com/apikey).
2. Provide it **either** way (never put the key in `app.py`):

**Option A – environment variable**

```bash
export GEMINI_API_KEY="your-key-here"      # macOS / Linux
setx GEMINI_API_KEY "your-key-here"        # Windows (open a new terminal afterwards)
```

**Option B – Streamlit secrets**

Create `.streamlit/secrets.toml` in your project folder:

```toml
GEMINI_API_KEY = "your-key-here"
```

Add `.streamlit/secrets.toml` to your `.gitignore` so the key is never committed. On Streamlit Community Cloud, paste the same line into the app's **Secrets** settings instead.

`GOOGLE_API_KEY` is also accepted as the variable name.

### Choosing the model

The app defaults to **`gemini-3.8-flash`**, which Google's model list (updated 2026-10-21) shows as a stable Flash model with a free tier. Google changes models regularly, so check the [current model list](https://ai.google.dev/gemini-api/docs/models) if you see a "model not found" error. To use a different model, set `GEMINI_MODEL` (as an environment variable or secret), for example:

```bash
export GEMINI_MODEL="gemini-3.5-flash-lite"
```

### Privacy note

When you click **Analyze**, the app sends a summary of your data to Google: totals, monthly and category figures, your budgets, and your 25 most recent transactions including descriptions. The app shows you this exact payload before you send it. On Google's free tier, content may be used to improve Google's products (see their [pricing/terms page](https://ai.google.dev/gemini-api/docs/pricing)). Don't analyse data you consider sensitive on the free tier.

## Using the app

1. **Transactions → Add transaction**: choose Income or Expense, then fill in date, amount, category and description.
2. **Transactions → All transactions**: search, filter, then open *Edit or delete a transaction* to change or remove a record.
3. **Budgets**: set a monthly limit for a category, then pick a month to see progress.
4. **AI Assistant**: click *Analyze my finances* (optionally ask a question).
5. **Import / Export**: download your CSV before you leave; upload it next time to restore your data.

### CSV format

Transactions CSV:

```csv
date,type,category,description,amount
2026-10-01,Income,Salary,October salary,3000.00
2026-10-03,Expense,Groceries,Weekly shop,84.50
```

- `date` must be `YYYY-MM-DD`.
- `type` must be `Income` or `Expense` (case-insensitive).
- `amount` must be a number greater than 0.
- Rows that fail validation are skipped and reported; valid rows are still imported.

Budgets CSV has the columns `category,monthly_budget`.

If you edit a CSV in a spreadsheet app, keep dates in `YYYY-MM-DD` format when saving. Text that begins with `=`, `+`, `-` or `@` is prefixed with an apostrophe on export (to block spreadsheet formula injection) and cleaned automatically on import.

## Data-storage limitation

**This app intentionally has no database or server-side storage.**

- Transactions and budgets are stored in Streamlit session state (server memory for your session).
- They are **lost** when: you refresh or close the browser tab, the session times out, the app restarts or redeploys, or (on hosted platforms) the app goes to sleep.
- **Always use Import / Export** to download a CSV before leaving, and upload it to continue later.
- The last AI analysis is also session-only.
- Each browser session has its own separate data; sessions do not share data.
- On a shared or hosted deployment, anyone with the link gets their own empty session. The app has no user accounts.

## Troubleshooting

| Problem | Fix |
|---|---|
| "No Gemini API key found" / AI button disabled | Set `GEMINI_API_KEY` (see above) and restart the app. |
| "Gemini rejected the API key" | Re-copy the key from Google AI Studio; make sure it has no extra spaces. |
| "Could not find that model" | Check `GEMINI_MODEL` against Google's current model list. |
| "Rate limit or quota reached" | Free tiers have limits; wait a minute and retry. |
| CSV import rejects rows | Check dates are `YYYY-MM-DD` and amounts are positive numbers. |
| Data disappeared | Expected – see [Data-storage limitation](#data-storage-limitation). |

## Testing status

This code was written and reviewed but **has not been run or tested** by its author. Please try each page (add/edit/delete a transaction, set a budget, export then import a CSV, run an AI analysis with your own key) and report anything that does not behave as described.

## Disclaimer

AI-generated suggestions are informational only, can be wrong, and are not financial advice.# Tracker
