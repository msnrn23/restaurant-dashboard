# Deploy to GitHub Pages — Step by Step

## What you need
- A GitHub account (free) → https://github.com
- Git installed on your computer → https://git-scm.com/downloads
- The 3 files from this zip: `index.html`, `data.js`, `.github/workflows/deploy.yml`

---

## Step 1 — Create a new GitHub repository

1. Go to https://github.com/new
2. Name it: `restaurant-dashboard`  (or anything you like)
3. Set it to **Public**
4. Do NOT tick "Add README" — leave it empty
5. Click **Create repository**

---

## Step 2 — Push the files from your computer

Open Terminal (Mac/Linux) or Command Prompt (Windows) and run:

```bash
# Navigate to where you unzipped the files
cd restaurant-dashboard

# Initialise git
git init
git add .
git commit -m "Initial dashboard"

# Connect to your new GitHub repo (replace YOUR_USERNAME)
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/restaurant-dashboard.git
git push -u origin main
```

---

## Step 3 — Enable GitHub Pages

1. Go to your repo on GitHub
2. Click **Settings** (top tab bar)
3. Click **Pages** (left sidebar, under "Code and automation")
4. Under **Source**, choose **GitHub Actions**
5. That's it — the workflow file handles everything automatically

---

## Step 4 — Wait ~60 seconds, then visit your live URL

```
https://YOUR_USERNAME.github.io/restaurant-dashboard/
```

Every time you `git push`, the site redeploys automatically in about 60 seconds.

---

## Updating your data

To load a new Excel file:

1. Export it to JSON with Python (see below)
2. Replace `data.js`
3. `git add data.js && git commit -m "Update data" && git push`

```python
import pandas as pd, json

df = pd.read_excel('your_new_file.xlsx')
df = df.round(2)
records = df.to_dict(orient='records')
with open('data.js', 'w') as f:
    f.write('const RAW_DATA=' + json.dumps(records, separators=(',',':')) + ';')
print('Done —', len(records), 'records')
```

The column names the dashboard expects:
- `Number_of_Customers`
- `Menu_Price`
- `Marketing_Spend`
- `Cuisine_Type`
- `Average_Customer_Spending`
- `Promotions`  (0 or 1)
- `Reviews`
- `Monthly_Revenue`

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Page shows 404 | Wait 2 minutes and hard-refresh (Ctrl+Shift+R) |
| Charts blank | Check browser console for errors; make sure `data.js` is in the same folder as `index.html` |
| Workflow fails | Go to **Actions** tab in your repo → click the failed run → read the error |
| Want a custom domain | Settings → Pages → Custom domain |
