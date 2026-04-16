# Deploying AmeriDream Mortgage Group to Netlify
## Step-by-Step Guide

---

## What You Need Before Starting

- A computer with all your site files downloaded
- A Netlify account — free at [netlify.com](https://netlify.com)
- Your domain login credentials at GoDaddy (for the custom domain step)

---

## Part 1 — Prepare Your Files

### Step 1 — Gather All 7 Files Into One Folder

Create a folder on your computer called `ameridream-website` and make sure it contains exactly these files:

```
ameridream-website/
  ├── index.html
  ├── contact.html              ← use the fixed version (Netlify Forms + honeypot)
  ├── learning-center.html      ← use the fixed version (calculator fix)
  ├── loan-types.html
  ├── meet-the-team.html
  ├── checklist.html
  └── _headers
```

> ⚠️ **Important:** Use the fixed versions of `contact.html` and `learning-center.html`
> that were provided during this project — not the originals.
> The `_headers` file has no extension — that is correct, do not add `.txt` or anything else.

### Step 2 — Verify the Folder

Open the folder and confirm:
- All 7 files are present
- No subfolders inside (all files sit at the root level)
- `_headers` has no file extension

---

## Part 2 — Create a Netlify Account

### Step 3 — Sign Up

1. Go to [app.netlify.com/signup](https://app.netlify.com/signup)
2. Click **Sign up with Email**
3. Enter your email address and create a password
4. Check your email and click the confirmation link
5. Log in

---

## Part 3 — Deploy the Site

### Step 4 — Deploy via Drag and Drop

1. Once logged in you will see the Netlify dashboard
2. Look for the section that says **"Drag and drop your site folder here"** at the bottom of the screen
3. Open your `ameridream-website` folder on your computer
4. Drag the entire folder and drop it onto that area in Netlify
5. Netlify will upload and process your files — this takes about 10–30 seconds
6. When complete you will see a green banner and a URL like `https://random-name.netlify.app`

### Step 5 — Verify the Site Is Working

1. Click the URL Netlify gives you (e.g. `https://random-name.netlify.app`)
2. Check each page loads correctly:
   - Home page
   - Contact page
   - Learning Center (test the calculator sliders)
   - Loan Types
   - Meet the Team
   - Buyer Checklist
3. Test the contact form — fill it out and submit. You should see the success message.

---

## Part 4 — Set Up Contact Form Notifications

### Step 6 — Enable Email Alerts for Form Submissions

1. In your Netlify dashboard click on your site
2. Click **Forms** in the top navigation
3. You should see a form called **contact** listed — if you don't see it yet, wait a few minutes and refresh
4. Click on **contact**
5. Click **Form notifications**
6. Click **Add notification**
7. Select **Email notification**
8. Enter the email address where you want to receive contact form submissions
9. Click **Save**

> From now on every time someone submits the contact form you will receive an email with their details.

---

## Part 5 — Rename Your Site (Optional but Recommended)

### Step 7 — Give Your Site a Clean Netlify URL

Before connecting your custom domain, rename the site from the random URL to something cleaner:

1. In your Netlify dashboard go to **Site settings**
2. Click **Change site name**
3. Enter `ameridream-mortgage` (or similar)
4. Click **Save**
5. Your site is now at `https://ameridream-mortgage.netlify.app`

---

## Part 6 — Connect Your Custom Domain

### Step 8 — Add Your Domain in Netlify

1. In your Netlify dashboard click **Domain management**
2. Click **Add a domain**
3. Type your domain — for example `ameridreammtg.com`
4. Click **Verify**
5. Click **Add domain**
6. Netlify will show you a screen with DNS instructions — **keep this tab open**

### Step 9 — Log In to GoDaddy

1. Open a new tab and go to [godaddy.com](https://godaddy.com)
2. Sign in to your account
3. Click your name in the top right corner
4. Click **My Products**
5. Find your domain (e.g. `ameridreammtg.com`)
6. Click **DNS** next to it

### Step 10 — Add a CNAME Record in GoDaddy

This connects `www.ameridreammtg.com` to Netlify:

1. In the GoDaddy DNS Management screen click **Add New Record**
2. Fill in the fields exactly as follows:

| Field | Value |
|---|---|
| Type | `CNAME` |
| Name | `www` |
| Value | `ameridream-mortgage.netlify.app` |
| TTL | `1 Hour` |

3. Click **Save**

### Step 11 — Add an A Record in GoDaddy

This connects the bare domain (`ameridreammtg.com` without www) to Netlify:

1. Click **Add New Record** again
2. Fill in the fields exactly as follows:

| Field | Value |
|---|---|
| Type | `A` |
| Name | `@` |
| Value | `75.2.60.5` |
| TTL | `1 Hour` |

3. Click **Save**

> ✅ You should now have both records saved in GoDaddy.
> Your existing MX records (email) and other records are untouched.

### Step 12 — Wait for DNS to Propagate

- DNS changes take between **10 minutes and 2 hours** to take effect worldwide
- In rare cases it can take up to 48 hours
- You can check progress at [dnschecker.org](https://dnschecker.org) — type in your domain and look for green checkmarks

---

## Part 7 — Enable HTTPS (SSL)

### Step 13 — Provision Your SSL Certificate

Once DNS has propagated:

1. Go back to your Netlify dashboard
2. Click **Domain management**
3. Scroll down to the **HTTPS** section
4. Click **Verify DNS configuration**
5. If DNS has propagated you will see a green checkmark
6. Click **Provision certificate**
7. Netlify will issue a free SSL certificate automatically — takes 1–2 minutes
8. Once complete your site is live at `https://ameridreammtg.com` with the padlock icon

---

## Part 8 — Test Everything Live

### Step 14 — Final Checklist

Go to your live domain and verify each of the following:

- [ ] `https://ameridreammtg.com` loads the homepage
- [ ] `https://www.ameridreammtg.com` also loads (redirects to main domain)
- [ ] Padlock / HTTPS shows in the browser address bar
- [ ] All 6 pages load without errors
- [ ] Navigation links between pages work
- [ ] Calculator sliders update numbers on the Learning Center page
- [ ] Contact form submits and shows success message
- [ ] You receive the test email notification in your inbox
- [ ] Client Login button takes you to the borrower portal
- [ ] Apply Now button takes you to the loan application
- [ ] Phone numbers are clickable on mobile

---

## How to Update the Site in the Future

Whenever you need to make a change to any page:

1. Edit the HTML file on your computer
2. Go to [app.netlify.com](https://app.netlify.com)
3. Click on your site
4. Click **Deploys**
5. Drag and drop the entire `ameridream-website` folder again onto the deploy area
6. Netlify will update the live site in seconds

> Always drag the full folder — not just the changed file.

---

## Troubleshooting

**Site shows "Page Not Found" after deploy:**
- Make sure `index.html` is at the root of the folder, not inside a subfolder

**Contact form not appearing in Netlify Forms tab:**
- The form is detected on first submission, not on deploy
- Submit a test entry and then check the Forms tab

**Custom domain not working after 2 hours:**
- Double check the CNAME value in GoDaddy matches your Netlify site name exactly
- Make sure there are no duplicate or conflicting A records on `@` in GoDaddy

**HTTPS not provisioning:**
- DNS must be fully propagated first
- Wait a little longer and click Verify DNS configuration again

**Email notifications not arriving:**
- Check your spam folder
- Verify the email address was saved correctly in Netlify Forms notifications

---

## Summary

| Step | Action | Where |
|---|---|---|
| 1–2 | Prepare your 7 files in one folder | Your computer |
| 3 | Create Netlify account | netlify.com |
| 4–5 | Drag and drop folder, verify site | Netlify |
| 6 | Set up contact form email alerts | Netlify → Forms |
| 7 | Rename site URL | Netlify → Site settings |
| 8 | Add custom domain | Netlify → Domain management |
| 9–11 | Add CNAME and A records | GoDaddy → DNS |
| 12 | Wait for DNS propagation | dnschecker.org |
| 13 | Enable HTTPS | Netlify → Domain management |
| 14 | Final test of everything | Your live domain |
