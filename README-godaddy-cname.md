# Adding a CNAME Record in GoDaddy to Point to Vercel

This guide walks you through connecting your GoDaddy domain to your Vercel site. Your existing GoDaddy services (email, borrower portal, etc.) will remain completely unaffected.

---

## Before You Start

You will need:
- Access to your GoDaddy account
- Your Vercel project open — found at [vercel.com/dashboard](https://vercel.com/dashboard)

---

## Step 1 — Add Your Custom Domain in Vercel

1. Log in to [vercel.com](https://vercel.com)
2. Click on your project (`ameridream-website`)
3. Go to **Settings → Domains**
4. Enter your domain (e.g. `ameridreammtg.com`) and click **Add**
5. Vercel will show you the DNS values you need — keep this tab open

---

## Step 2 — Log In to GoDaddy

1. Go to [godaddy.com](https://godaddy.com) and sign in
2. Click your name in the top right → **My Products**
3. Find your domain and click **DNS** (or **Manage DNS**)

---

## Step 3 — Add the CNAME Record

In the DNS Management screen:

1. Click **Add New Record**
2. Set the fields as follows:

| Field | Value |
|---|---|
| **Type** | `CNAME` |
| **Name** | `www` |
| **Value** | `cname.vercel-dns.com` |
| **TTL** | `1 Hour` (or default) |

3. Click **Save**

> **Note:** The CNAME record handles `www.yourdomain.com`. For the bare/apex domain (without www), see Step 4.

---

## Step 4 — Point the Apex Domain (Without www)

GoDaddy does not support CNAME records on the apex domain (e.g. `ameridreammtg.com` with no www). Instead add an **A record** pointing to Vercel's IP.

1. Click **Add New Record**
2. Add the following A record:

| Field | Value |
|---|---|
| **Type** | `A` |
| **Name** | `@` |
| **Value** | `76.76.21.21` |
| **TTL** | `1 Hour` |

3. Click **Save**

> Double-check the latest IP in your Vercel dashboard under **Settings → Domains** — it will show the exact value to use.

---

## Step 5 — Wait for DNS Propagation

- DNS changes typically take **10 minutes to 2 hours** to propagate
- In rare cases it can take up to **48 hours**
- You can check propagation status at [dnschecker.org](https://dnschecker.org)

---

## Step 6 — Verify in Vercel

Once DNS has propagated:

1. Go back to Vercel → **Settings → Domains**
2. Your domain should show a green **Valid Configuration** status
3. Vercel issues HTTPS certificates automatically — no action needed

---

## Troubleshooting

**Site not showing after 2 hours:**
- Double-check the CNAME value in GoDaddy matches `cname.vercel-dns.com` exactly
- Make sure there are no conflicting A records on the `www` name in GoDaddy

**Domain shows "Invalid Configuration" in Vercel:**
- DNS must be fully propagated — wait longer and refresh the Vercel dashboard
- Confirm the A record IP matches what Vercel shows under Settings → Domains

**Existing email stopped working:**
- Your MX records should not have been touched — check that they are still present in GoDaddy DNS
- Adding a CNAME for `www` and an A record for `@` should not affect MX records

---

## Summary of Records to Add in GoDaddy

| Type | Name | Value |
|---|---|---|
| `CNAME` | `www` | `cname.vercel-dns.com` |
| `A` | `@` | `76.76.21.21` |
