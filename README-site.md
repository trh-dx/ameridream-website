# AmeriDream Mortgage Group — Website

Static HTML/CSS/JS website for AmeriDream Mortgage Group, LLC (NMLS #275209). Hosted on Vercel, deployed via GitHub.

---

## Pages

| File | URL | Description |
|---|---|---|
| `index.html` | `/` | Homepage — hero, services, rates, process, reviews, team, FAQ |
| `contact.html` | `/contact` | Contact form (Netlify Forms), office locations, hours |
| `learning-center.html` | `/learning-center` | Mortgage calculator, affordability calculator, refinance savings calculator, glossary |
| `loan-types.html` | `/loan-types` | Conventional, FHA, VA, USDA, Jumbo loan details |
| `meet-the-team.html` | `/meet-the-team` | Team profiles split into Management and Loan Originators sections, each with NMLS number, email, location, and personal site link |
| `checklist.html` | `/checklist` | Interactive first-time buyer checklist |

---

## Tech Stack

- Pure HTML, CSS, JavaScript — no frameworks, no build process
- Google Fonts (DM Serif Display, DM Sans)
- Netlify Forms for contact form submissions
- No database, no backend, no dependencies

---

## Deployment

The site is connected to GitHub and auto-deploys on every push to `main`.

### Workflow
1. Make changes to the relevant HTML file locally
2. Commit and push to `main` on GitHub
3. Vercel detects the push and deploys automatically — live in ~30 seconds

### First-time Vercel setup
1. Go to [vercel.com](https://vercel.com) and sign in
2. Click **Add New Project** → **Import Git Repository**
3. Select `trh-dx/ameridream-website`
4. Leave all settings as default (no build command needed for static HTML)
5. Click **Deploy**

---

## Navigation

The top nav bar is identical across all 6 pages and includes:
- Logo (links to homepage)
- Page links (bordered blue pills): Loan Types, Learning Center, Buyer Checklist, Our Team, Contact
- Facebook icon (links to AmeriDream Facebook page)
- Client Login button (bordered blue)
- Phone number (tap-to-call on mobile)
- Apply Now button (red)

On mobile (under 900px) the page links collapse. The Facebook icon, Client Login, phone, and Apply Now remain visible with compact sizing.

To update the Facebook link, search `facebook.com/450436438398721` across all 6 HTML files and replace with your page URL.

---

## Calculators

All three calculators live in `learning-center.html`. Logic is in a `<script>` tag at the bottom of that file.

| Calculator | ID | Description |
|---|---|---|
| Mortgage payment | `#calculator` | Estimates monthly P&I based on price, down payment, rate, and term |
| Affordability | `#affordability` | Estimates max home price based on income, debt, savings, and rate |
| Refinance savings | `#refinance` | Compares current loan vs new loan — shows monthly savings, break-even point, and lifetime interest saved |

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

A `_headers` file is included in the root of the project. It applies the following security headers to every page:

- `X-Frame-Options` — prevents clickjacking
- `X-Content-Type-Options` — prevents MIME sniffing
- `Referrer-Policy` — controls referrer data on external links
- `Permissions-Policy` — blocks camera, mic, and geolocation access
- `X-XSS-Protection` — cross-site scripting protection
- `Content-Security-Policy` — whitelists approved scripts and styles

---

## External Links

| Purpose | URL |
|---|---|
| Apply Now / Loan Application | `https://www.ameridream.mortgage/?loanapp&siteid=5691082585&workFlowId=67545` |
| Client / Borrower Portal Login | `https://www.ameridream.mortgage/?borrowerportal&siteid=5691082585` |
| Facebook Page | `https://www.facebook.com/450436438398721` |
| Privacy Policy | `https://ameridreammtg.com/privacy-policy` |
| File a Complaint | `https://ameridreammtg.com/file-a-complaint` |
| NMLS Consumer Access | `http://www.nmlsconsumeraccess.org/` |

---

## Custom Domain

DNS is managed through GoDaddy. See `README-godaddy-cname.md` for instructions on pointing your domain to Vercel.

---

## Team Page

`meet-the-team.html` is split into two sections:

**Management**
| Name | Title | NMLS |
|---|---|---|
| Jesse Woskowicz | President | #298738 |
| Tiffany Hand | Senior Vice President — Secondary | — |
| Name TBD | Senior Vice President — Operations | — |

**Loan Originators**
| Name | NMLS | Location |
|---|---|---|
| Kevin Shaw | #288362 | The Colony |
| Jacob Mejia | #365906 | The Colony |
| Brad Hunter | #1747584 | Bridgeport |
| Shea Armstrong | #37127 | The Colony |
| Leslie Riccitelli | #1987410 | The Colony |
| Shaley Tate | #2256129 | Bridgeport |
| Jake Martinez | #2453141 | Bridgeport |
| Menda Huddleston | #1822866 · ML027292 (OK) | The Colony |
| Diedra Freeman | #2404664 | Bridgeport |

To update the Operations SVP name, search `Name TBD` in `meet-the-team.html`.

---

## Notes

- All phone numbers are formatted as `tel:` links for mobile tap-to-call
- The checklist progress is stored in memory only — it resets on page refresh (no backend needed)
- NMLS numbers: Main #275209 · Decatur #1652928 · Bridgeport #2286544
- To update the Facebook link, search `facebook.com/450436438398721` across all 6 HTML files and replace with your page URL
