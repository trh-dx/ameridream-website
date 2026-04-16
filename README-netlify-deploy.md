# Deploying AmeriDream Mortgage Group
## GitHub + Vercel (Current Workflow)

The site is now deployed via GitHub and Vercel. Every push to the `main` branch automatically triggers a new deployment — no manual uploads needed.

---

## How It Works

```
Edit files locally
    ↓
git add + git commit + git push
    ↓
GitHub (trh-dx/ameridream-website)
    ↓
Vercel detects push → builds and deploys automatically
    ↓
Live site updated in ~30 seconds
```

---

## Daily Update Workflow

### Step 1 — Edit the file
Open the HTML file you want to change and make your edits.

### Step 2 — Push to GitHub
```bash
git add <filename>
git commit -m "describe what you changed"
git push origin main
```

### Step 3 — Vercel deploys automatically
Go to [vercel.com/dashboard](https://vercel.com/dashboard), click your project, and watch the deployment complete under the **Deployments** tab.

---

## First-Time Vercel Setup (Already Done)

For reference — this was completed during initial setup:

1. Signed in to [vercel.com](https://vercel.com)
2. Clicked **Add New Project** → **Import Git Repository**
3. Selected `trh-dx/ameridream-website` from GitHub
4. Left all settings as default (no build command needed for static HTML)
5. Clicked **Deploy**

Vercel auto-deploys on every push to `main` from that point on.

---

## Checking Your Live Site

- Vercel URL: shown in your Vercel project dashboard
- Custom domain: `ameridreammtg.com` (once DNS is configured — see `README-godaddy-cname.md`)

---

## Pages to Verify After Each Deploy

| Page | Check |
|---|---|
| Homepage | Loads, nav works, Facebook icon visible |
| Learning Center | All 3 calculators work (mortgage, affordability, refinance) |
| Contact | Form submits successfully |
| Loan Types | All sections load |
| Meet the Team | All profiles visible |
| Buyer Checklist | Checkboxes toggle correctly |

---

## Rollback a Deployment

If something breaks after a push:

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Click your project
3. Click **Deployments**
4. Find the last working deployment
5. Click the three-dot menu → **Promote to Production**

The previous version goes live instantly.

---

## Troubleshooting

**Changes not showing after push:**
- Check the Deployments tab in Vercel — look for any build errors
- Hard refresh your browser (Ctrl+Shift+R or Cmd+Shift+R)

**Vercel not detecting pushes:**
- Go to Vercel → Settings → Git — confirm the repo and branch are connected
- Re-authorize GitHub if needed

**Custom domain not working:**
- See `README-godaddy-cname.md` for DNS setup instructions
