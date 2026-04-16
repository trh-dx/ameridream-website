# Adding a CNAME Record in GoDaddy to Point to Netlify

This guide walks you through connecting your GoDaddy domain to your Netlify site without transferring your DNS. Your existing GoDaddy services (email, borrower portal, etc.) will remain completely unaffected.

---

## Before You Start

You will need:
- Access to your GoDaddy account
- Your Netlify site's subdomain — found in Netlify under **Site settings → Domain management** (looks like `your-site-name.netlify.app`)

---

## Step 1 — Add Your Custom Domain in Netlify

1. Log in to [app.netlify.com](https://app.netlify.com)
2. Click on your site
3. Go to **Site settings → Domain management**
4. Click **Add a domain**
5. Enter your domain (e.g. `ameridreammtg.com`) and click **Verify**
6. Netlify will show you the DNS values you need — keep this tab open

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
| **Value** | `your-site-name.netlify.app` (your Netlify subdomain) |
| **TTL** | `1 Hour` (or default) |

3. Click **Save**

> **Note:** The CNAME record handles `www.yourdomain.com`. For the bare/apex domain (without www), see Step 4.

---

## Step 4 — Point the Apex Domain (Without www)

GoDaddy does not support CNAME records on the apex domain (e.g. `ameridreammtg.com` with no www). Instead you need to add **A records** pointing to Netlify's load balancer IPs.

1. Click **Add New Record**
2. Add the following A records one at a time:

| Field | Value |
|---|---|
| **Type** | `A` |
| **Name** | `@` |
| **Value** | `75.2.60.5` |
| **TTL** | `1 Hour` |

Netlify currently uses the IP above for apex domains. Double-check the latest value in your Netlify dashboard under **Domain management** — it will show the exact IP to use.

---

## Step 5 — Wait for DNS Propagation

- DNS changes typically take **10 minutes to 2 hours** to propagate
- In rare cases it can take up to **48 hours**
- You can check propagation status at [dnschecker.org](https://dnschecker.org)

---

## Step 6 — Enable HTTPS in Netlify

Once your domain is propagating:

1. Go back to Netlify → **Site settings → Domain management**
2. Scroll down to **HTTPS**
3. Click **Verify DNS configuration**
4. Once verified, click **Provision certificate**
5. Netlify will issue a free SSL certificate automatically via Let's Encrypt

---

## Troubleshooting

**Site not showing after 2 hours:**
- Double-check the CNAME value in GoDaddy matches your Netlify subdomain exactly
- Make sure there are no conflicting A records on the `www` name in GoDaddy

**HTTPS not provisioning:**
- DNS must be fully propagated before SSL will work
- Wait a bit longer and try **Verify DNS configuration** again in Netlify

**Existing email stopped working:**
- Your MX records should not have been touched — check that they are still present in GoDaddy DNS
- Adding a CNAME for `www` and an A record for `@` should not affect MX records

---

## Summary of Records to Add in GoDaddy

| Type | Name | Value |
|---|---|---|
| `CNAME` | `www` | `your-site-name.netlify.app` |
| `A` | `@` | `75.2.60.5` |
