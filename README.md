# Renewal Copilot

A free, open-source **renewal advisor** for anyone who manages subscription/contract renewals.
Enter a customer's contract value, adoption, and renewal date — get a clear recommendation
(renew, expand, or retain), the numbers behind it, talking points, and a ready-to-send email.
Load your whole book from a CSV or a Google Sheet and work it as a pipeline.

It's a **single HTML file**. No backend, no build step, no account. Everything runs in the
browser and your data never leaves it.

---

## Quick start

1. Download `renewal-copilot.html`.
2. Open it in your browser (double-click), or host it anywhere static (GitHub Pages, Netlify, S3…).
3. That's it. The **Advisor** tab works immediately. For the **Pipeline**, import a CSV.

> Tip: to serve locally, run `python3 -m http.server 8080` in the folder and open `http://localhost:8080/renewal-copilot.html`.

---

## What it does

**Advisor** — for one account:
- A recommendation based on adoption: **Expand** (high adoption), **Renew & grow** (healthy),
  **Renew flat** (soft), or **Retain** (churn risk). It leads with value and never volunteers a discount.
- Suggested renewal ACV with the uplift/expansion %, ACV delta, and a churn-risk read.
- A visual: adoption meter + current-vs-recommended ACV.
- Talking points and a one-click **email draft** (renewal heads-up, final reminder, or recommendation).
- A renewal-timing flag (outreach + final-reminder windows).

**Pipeline** — for your whole book:
- Import a **CSV** or connect a **Google Sheet** (see below).
- Filter by owner and health; sorted soonest-first.
- Overview metrics: accounts, total ACV, avg adoption, renewing ≤60 days, at-risk.
- Health + recommended ACV per row; click **Open** to load any account into the Advisor.
- Data persists in your browser (localStorage) so it's there when you come back.

---

## CSV format

Headers in row 1 (order doesn't matter — columns are matched by name). Use the in-app
**Download CSV template** button, or:

```
Customer,Owner,Renewal Date,Plan,ACV,Adoption %,Status,Notes
Acme Corp,Jordan,2026-08-15,Growth,48000,82,,High usage
Globex,Priya,2026-09-02,Starter,12000,28,,Low adoption
```

Recognized column names (any one works): Customer/Account/Company · Owner/CSM/Rep ·
Renewal Date · Plan/Tier · ACV/ARR/Value · Adoption/Usage/Utilization % · Status · Notes.

---

## Connect a Google Sheet (optional, ~5–10 min)

Live data from a Google Sheet you own or that's shared with you. Each user brings their own
Google OAuth client — nothing is shared or hard-coded.

1. **Google Cloud Console** → create/pick a project.
2. **APIs & Services → Library** → enable **Google Sheets API**.
3. **OAuth consent screen** → **External** → add your email under **Test users**
   (or **Internal** if you're on Google Workspace).
4. **Credentials → Create credentials → OAuth client ID → Web application**.
   Under **Authorized JavaScript origins** add wherever you open the app
   (e.g. `http://localhost:8080` and your hosting URL).
5. In the app's Pipeline tab, expand **Connect a Google Sheet**, paste your **Client ID** and
   **sheet link**, and click **Connect**. Put your renewals on the **first tab**, headers in row 1.

The app requests **read-only** access to Sheets, through your own Google account and permissions —
it only reads sheets you can already open.

---

## Customize (no code-diving)

Open the file and edit the `CONFIG` object near the top of the `<script>`:

| Setting | What it does |
|---|---|
| `currency` | ISO code, e.g. `"USD"`, `"EUR"`, `"GBP"` |
| `upliftPct` | Standard renewal uplift for healthy accounts (default 7%) |
| `expansionPct` | Suggested expansion for high-adoption accounts (default 20%) |
| `upsellThreshold` / `healthyThreshold` / `atRiskThreshold` | Adoption % cutoffs for the plays |
| `outreach1Days` / `outreach2Days` | Outreach and final-reminder windows |
| `signatureRole` / `githubUrl` | Email signature line and footer link |

---

## Privacy

100% client-side. CSV data is parsed in your browser and cached in `localStorage`. The Google
Sheets connection uses your own OAuth client and a read-only token held only in the page session.
No server, no tracking, nothing leaves your machine.

---

## License

MIT — do whatever you like. Attribution appreciated but not required.
