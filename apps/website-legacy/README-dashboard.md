# AmeriDream Mortgage — Operations Dashboard

Internal read-only pipeline and reporting dashboard for AmeriDream Mortgage Group, LLC. Pulls live data from Encompass (ICE Mortgage Technology) via the Developer Connect API and displays it in a secure, role-based web interface.

---

## Overview

- **Type:** Read-only internal dashboard — no data is written back to Encompass
- **Purpose:** Give loan officers and managers a unified view of the loan pipeline, lead activity, and performance metrics
- **Data source:** Encompass LOS via ICE Developer Connect REST API
- **Access:** Login required — not publicly accessible

---

## Features

### Overview Page
- Total loans in pipeline
- Loans by stage (funnel chart)
- Closings this month vs last month
- Pipeline breakdown by loan officer

### Pipeline Page
- Full table of active loans — borrower name, loan amount, type, stage, assigned officer, days in current stage
- Filterable by loan officer, stage, and loan type

### Leads Page
- New inquiries this week
- Follow-up overdue alerts
- Lead to application conversion rate

### Loan Officer View
- Individual pipeline per officer
- Close rate and average days to close

---

## Architecture

```
Browser (Dashboard UI)
    │
    │  Login via Auth0 (MFA required)
    │
    ▼
Node.js Backend (Railway)
    │
    │  OAuth 2.0 token exchange
    │  Proxied API requests
    │
    ▼
Encompass Developer Connect API
(ICE Mortgage Technology)
```

The browser never communicates directly with Encompass. All API calls are proxied through the backend, which holds the credentials securely.

---

## Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend | HTML / CSS / JavaScript | Dashboard UI |
| Charts | Chart.js | Pipeline funnel, bar charts |
| Backend | Node.js (Express) | API proxy, token management |
| Hosting | Railway | Backend server + HTTPS |
| Authentication | Auth0 | Login, MFA, session management |
| LOS API | Encompass Developer Connect | Live loan data |

---

## What is Railway and Why Use It

Railway is a cloud hosting platform that runs backend code — it's the place where your server lives on the internet 24/7. Similar to Heroku or a small AWS instance but significantly simpler to set up and manage. You give it your code, it handles everything else — servers, infrastructure, scaling, HTTPS certificates, and uptime.

### Why Netlify Alone Is Not Enough

Netlify is built for static files — HTML, CSS, and JS that doesn't change. The marketing site is perfect for it. But the dashboard needs a persistent backend server that:

- Stores Encompass API credentials securely
- Exchanges OAuth tokens with ICE Mortgage
- Proxies every API request so credentials never reach the browser
- Runs constantly waiting for requests

That requires a real server. Railway provides one.

### How the Two Platforms Work Together

```
User's browser
    ↓
Netlify  (dashboard HTML/CSS/JS — what the user sees)
    ↓
Railway  (Node.js backend — holds API keys, talks to Encompass)
    ↓
Encompass API (ICE Mortgage Technology)
```

Netlify handles what the user sees. Railway handles secure communication with Encompass behind the scenes. Neither can do the other's job.

### Railway vs Other Backend Hosting Options

| Platform | Ease of Setup | Cost | Notes |
|---|---|---|---|
| **Railway** | Very easy | ~$5/mo | Recommended — deploys from GitHub like Netlify |
| Render | Easy | Free–$7/mo | Good alternative to Railway |
| Heroku | Easy | $5–$25/mo | Was the standard, now pricier |
| AWS / Azure | Complex | Variable | Overkill for this project |
| DigitalOcean | Medium | $6+/mo | More control but more setup |

Railway is the right choice here because:
- **Deploys from GitHub** — push code and it auto-deploys, same workflow as Netlify
- **Environment variables built in** — store Encompass credentials safely in their dashboard, never in code
- **HTTPS automatic** — no SSL configuration needed
- **Logs and monitoring included** — see what's happening in real time
- **No DevOps knowledge required** — no servers, patches, or infrastructure to manage
- **~$5/month** — cost-effective for an internal tool

---

## Prerequisites

Before starting the build, the following must be in place:

- [ ] Encompass admin access to authorize API connection
- [ ] ICE Developer Connect account — register at developer.icemortgage.com
- [ ] Client ID and Client Secret from ICE Developer Connect
- [ ] Auth0 account (free tier) — register at auth0.com
- [ ] Railway account (free tier to start) — register at railway.app
- [ ] Node.js installed locally for development

---

## Encompass API Setup

### Step 1 — Register on ICE Developer Connect

1. Go to [developer.icemortgage.com](https://developer.icemortgage.com)
2. Create a developer account using your company email
3. Click **Create Application**
4. Fill in:
   - **Application name:** AmeriDream Ops Dashboard
   - **Application type:** Internal
   - **Scope:** Read-only (request only read permissions)
5. Submit and wait for approval — ICE will email your **Client ID** and **Client Secret**

> **Note:** For internal read-only dashboards, approval is typically fast. Contact your ICE account representative to expedite if needed.

### Step 2 — Authorize the API in Encompass

Your Encompass admin needs to complete this step:

1. Log in to Encompass as an admin
2. Go to **Encompass Settings → Company/User Setup → API Access**
3. Add your new application's Client ID to the approved applications list
4. Set permissions to **Read Only**
5. Save

### Step 3 — Store Your Credentials

Never put credentials in your code. Store them as environment variables:

```
ENCOMPASS_CLIENT_ID=your_client_id_here
ENCOMPASS_CLIENT_SECRET=your_client_secret_here
ENCOMPASS_BASE_URL=https://api.elliemae.com
```

On Railway, add these under **Project → Variables**.

---

## Encompass OAuth 2.0 Authentication

Encompass uses OAuth 2.0 Client Credentials flow for server-to-server authentication.

### Token Request

```javascript
// backend/auth/encompass.js

const getEncompassToken = async () => {
  const response = await fetch(`${process.env.ENCOMPASS_BASE_URL}/oauth2/v1/token`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded'
    },
    body: new URLSearchParams({
      grant_type: 'client_credentials',
      client_id: process.env.ENCOMPASS_CLIENT_ID,
      client_secret: process.env.ENCOMPASS_CLIENT_SECRET,
      scope: 'lp'
    })
  });

  const data = await response.json();
  return data.access_token;
};
```

Tokens expire — cache them and refresh before expiry. Do not request a new token on every API call.

---

## Key Encompass API Endpoints

All endpoints are prefixed with `https://api.elliemae.com/encompass/v3`

| Data | Endpoint | Method |
|---|---|---|
| Loan pipeline | `/loanPipeline` | POST |
| Single loan detail | `/loans/{loanId}` | GET |
| Loan milestones | `/loans/{loanId}/milestones` | GET |
| Loan associates (officer) | `/loans/{loanId}/associates` | GET |
| Pipeline fields | `/loanPipeline` with field list | POST |

### Example — Fetch Pipeline

```javascript
// backend/routes/pipeline.js

const fetchPipeline = async (token) => {
  const response = await fetch(
    `${process.env.ENCOMPASS_BASE_URL}/encompass/v3/loanPipeline`,
    {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        fields: [
          'Loan.LoanNumber',
          'Loan.BorrowerName',
          'Loan.LoanAmount',
          'Loan.LoanType',
          'Loan.MilestoneName',
          'Loan.LoanOfficerName',
          'Loan.ExpectedClosingDate',
          'Loan.ApplicationDate'
        ],
        filter: {
          operator: 'and',
          terms: [
            {
              canonicalName: 'Loan.MilestoneName',
              matchType: 'exact',
              value: 'Active',
              include: true
            }
          ]
        },
        sortOrder: [
          {
            canonicalName: 'Loan.ApplicationDate',
            order: 'desc'
          }
        ]
      })
    }
  );

  return response.json();
};
```

---

## Backend Structure

```
/backend
  /auth
    encompass.js      ← token management
  /routes
    pipeline.js       ← loan pipeline endpoint
    officers.js       ← loan officer breakdown
    metrics.js        ← summary stats
  /middleware
    auth.js           ← verify Auth0 JWT on every request
    rateLimit.js      ← rate limiting
  server.js           ← Express app entry point
  .env                ← environment variables (never commit this)
```

### server.js (simplified)

```javascript
const express = require('express');
const cors = require('cors');
const rateLimit = require('express-rate-limit');
const { checkJwt } = require('./middleware/auth');

const app = express();

app.use(express.json());
app.use(cors({ origin: process.env.FRONTEND_URL }));

// Rate limiting — max 100 requests per 15 minutes per IP
const limiter = rateLimit({ windowMs: 15 * 60 * 1000, max: 100 });
app.use(limiter);

// All routes require a valid Auth0 JWT
app.use(checkJwt);

app.use('/api/pipeline', require('./routes/pipeline'));
app.use('/api/officers', require('./routes/officers'));
app.use('/api/metrics', require('./routes/metrics'));

app.listen(process.env.PORT || 3000);
```

---

## Auth0 Setup

### Step 1 — Create an Auth0 Application

1. Go to [manage.auth0.com](https://manage.auth0.com)
2. Click **Applications → Create Application**
3. Name it **AmeriDream Dashboard**
4. Select **Single Page Application**
5. Under **Allowed Callback URLs** add your dashboard URL
6. Under **Allowed Logout URLs** add your dashboard URL
7. Save

### Step 2 — Create an Auth0 API

1. Go to **APIs → Create API**
2. Name it **AmeriDream Dashboard API**
3. Set identifier to your backend URL (e.g. `https://your-backend.railway.app`)
4. Save

### Step 3 — Enable MFA

1. Go to **Security → Multi-factor Auth**
2. Enable **One-time Password (OTP)**
3. Set policy to **Always** — every login requires MFA
4. Save

### Step 4 — Create Users

1. Go to **User Management → Users → Create User**
2. Add each team member with their company email
3. They will receive an email to set their password and set up MFA on first login

### Step 5 — Role-Based Access

1. Go to **User Management → Roles**
2. Create two roles: **loan_officer** and **manager**
3. Assign roles to users
4. In your backend, read the role from the JWT and filter data accordingly:
   - `loan_officer` — sees only their own loans
   - `manager` — sees all loans

---

## Environment Variables

### Backend (.env — never commit to git)

```
ENCOMPASS_CLIENT_ID=
ENCOMPASS_CLIENT_SECRET=
ENCOMPASS_BASE_URL=https://api.elliemae.com
AUTH0_DOMAIN=your-tenant.auth0.com
AUTH0_AUDIENCE=https://your-backend.railway.app
FRONTEND_URL=https://your-dashboard-url.com
PORT=3000
```

### Frontend

```
AUTH0_DOMAIN=your-tenant.auth0.com
AUTH0_CLIENT_ID=your_auth0_client_id
API_BASE_URL=https://your-backend.railway.app
```

---

## Deployment

### Backend — Railway

1. Push backend code to a GitHub repo
2. Go to [railway.app](https://railway.app) → **New Project → Deploy from GitHub**
3. Select your repo
4. Add all environment variables under **Project → Variables**
5. Railway will build and deploy automatically
6. HTTPS is provided automatically — no configuration needed

### Frontend

For a plain HTML/CSS/JS frontend:
- Can be hosted on Netlify (separate from the marketing site, or a subdomain like `dashboard.ameridreammtg.com`)
- Add Auth0 login before the dashboard loads

---

## Security Checklist

- [ ] OAuth 2.0 with Encompass — no passwords stored anywhere
- [ ] API credentials stored as environment variables on Railway only
- [ ] Browser never calls Encompass directly — all calls proxied through backend
- [ ] Auth0 MFA enabled and set to Always
- [ ] Read-only API scope requested from ICE — no write permissions
- [ ] HTTPS enforced on both frontend and backend
- [ ] CORS locked to frontend URL only — no open CORS
- [ ] Rate limiting on all backend endpoints
- [ ] Session timeout configured in Auth0 (recommend 8 hours)
- [ ] `.env` file in `.gitignore` — never committed to version control
- [ ] Separate user accounts per team member — no shared logins
- [ ] Role-based data filtering — officers see only their own loans

---

## Monthly Cost

| Service | Cost |
|---|---|
| Railway (backend) | ~$5 / month |
| Auth0 | Free (up to 7,500 users) |
| Netlify (frontend) | Free |
| Encompass API access | Confirm with ICE rep — likely included in existing subscription |
| **Total** | **~$5 / month** |

---

## Build Timeline

| Week | Milestone |
|---|---|
| Week 1 | API credentials approved, backend scaffolded, Auth0 login working, first live data from Encompass |
| Week 2 | Pipeline page, metrics page, loan officer view, Chart.js visualizations |
| Week 3 | Role-based filtering, rate limiting, testing with real data, deploy to Railway + Netlify |

---

## Important Notes

- This dashboard is **read-only** — it cannot modify, delete, or create any data in Encompass
- All borrower data displayed is pulled live from Encompass — nothing is stored in a separate database
- Access is restricted to authenticated AmeriDream team members only
- MFA is mandatory — do not disable this setting
- If Encompass API credentials are ever compromised, revoke them immediately in the ICE Developer Connect portal and generate new ones
- Keep your Auth0 and Railway accounts secured with strong passwords and MFA

---

## Support & References

- Encompass Developer Docs: [developer.icemortgage.com](https://developer.icemortgage.com)
- Auth0 Docs: [auth0.com/docs](https://auth0.com/docs)
- Railway Docs: [docs.railway.app](https://docs.railway.app)
- Chart.js Docs: [chartjs.org/docs](https://www.chartjs.org/docs)
- ICE Mortgage Support: Contact your ICE account representative for API access questions
