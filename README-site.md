# AmeriDream Mortgage Group — Website

Static HTML/CSS/JS website for AmeriDream Mortgage Group, LLC (NMLS #275209). Hosted on Netlify.

---

## Pages

| File | URL | Description |
|---|---|---|
| `index.html` | `/` | Homepage — hero, services, rates, process, reviews, team, FAQ |
| `contact.html` | `/contact` | Contact form (Netlify Forms), office locations, hours |
| `learning-center.html` | `/learning-center` | Mortgage calculator, affordability calculator, glossary |
| `loan-types.html` | `/loan-types` | Conventional, FHA, VA, USDA, Jumbo loan details |
| `meet-the-team.html` | `/meet-the-team` | Loan originator profiles |
| `checklist.html` | `/checklist` | Interactive first-time buyer checklist |

---

## Tech Stack

- Pure HTML, CSS, JavaScript — no frameworks, no build process
- Google Fonts (DM Serif Display, DM Sans)
- Netlify Forms for contact form submissions
- No database, no backend, no dependencies

---

## Deployment

### First deploy
1. Make sure all 7 files are in one folder:
   - `index.html`
   - `contact.html`
   - `learning-center.html`
   - `loan-types.html`
   - `meet-the-team.html`
   - `checklist.html`
   - `_headers`
2. Go to [app.netlify.com](https://app.netlify.com)
3. Drag and drop the folder onto the deploy zone
4. Site is live instantly at a Netlify subdomain (e.g. `random-name.netlify.app`)

### Updating the site
- Make changes to the relevant HTML file
- Drag and drop the full folder again onto Netlify
- Or connect a GitHub repo for automatic deploys on every push

---

## Contact Form

The contact form on `contact.html` uses **Netlify Forms**.

- Submissions appear in your Netlify dashboard under **Forms → contact**
- A honeypot field (`bot-field`) is included for spam protection
- To receive email notifications for new submissions:
  1. Go to Netlify dashboard → **Forms**
  2. Click on the **contact** form
  3. Go to **Form notifications** → **Add notification** → **Email notification**
  4. Enter the email address you want alerts sent to

---

## Security

A `_headers` file is included in the root of the project. Netlify reads this automatically and applies the following security headers to every page:

- `X-Frame-Options` — prevents clickjacking
- `X-Content-Type-Options` — prevents MIME sniffing
- `Referrer-Policy` — controls referrer data on external links
- `Permissions-Policy` — blocks camera, mic, and geolocation access
- `X-XSS-Protection` — cross-site scripting protection
- `Content-Security-Policy` — whitelists approved scripts and styles

---

## External Links

These are third-party URLs used throughout the site. They are not hosted on Netlify and require no changes:

| Purpose | URL |
|---|---|
| Apply Now / Loan Application | `https://www.ameridream.mortgage/?loanapp&siteid=5691082585&workFlowId=67545` |
| Client / Borrower Portal Login | `https://www.ameridream.mortgage/?borrowerportal&siteid=5691082585` |
| Privacy Policy | `https://ameridreammtg.com/privacy-policy` |
| File a Complaint | `https://ameridreammtg.com/file-a-complaint` |
| NMLS Consumer Access | `http://www.nmlsconsumeraccess.org/` |

---

## Custom Domain

DNS is managed through GoDaddy. See `README-godaddy-cname.md` for instructions on pointing your domain to Netlify.

---

## Notes

- All phone numbers are formatted as `tel:` links for mobile tap-to-call
- Calculator logic is in `learning-center.html` inside a `<script>` tag at the bottom of the file
- The checklist progress is stored in memory only — it resets on page refresh (no backend needed)
- NMLS numbers: Main #275209 · Decatur #1652928 · Bridgeport #2286544
