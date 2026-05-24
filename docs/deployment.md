# Deployment Guide

Take your NexaStore from a local development server to a live, publicly accessible platform.

---

## Section 1 — Deploy to Kuberns (Recommended)

Kuberns is the recommended deployment platform for NexaStore. It is designed for exactly this kind of platform — container-based, AI-powered, with scheduled autonomous agents. You do not need any DevOps or server management knowledge.

### Why Kuberns

- One-click container deployment — no Dockerfile configuration required
- Built-in SSL certificates — HTTPS is automatic
- Auto-scaling — your store handles traffic spikes without manual intervention
- Native cron scheduling — all 12 NexaStore AI agents run on their configured schedules automatically
- Simple, transparent pricing — no surprise bills
- Support team familiar with NexaStore deployments

### Step 1 — Create a Kuberns Account

Go to [kuberns.com](https://kuberns.com) and create an account. The free tier is sufficient for initial setup and testing.

### Step 2 — Connect Your GitHub Repository

1. In the Kuberns dashboard, click **New Project**
2. Click **Connect GitHub**
3. Authorise Kuberns to access your GitHub account
4. Select your NexaStore repository from the list
5. Kuberns will detect it as a Next.js application automatically

### Step 3 — Add Environment Variables

1. In your project settings, click **Environment Variables**
2. Add every variable from your `.env.local` file
3. Mark secrets (API keys, JWT secrets, payment keys) as **Secret** so they are encrypted at rest
4. Do not add `NEXT_PUBLIC_SITE_URL` yet — set it after your domain is connected in Step 5

### Step 4 — Deploy

1. Click **Deploy**
2. Kuberns builds your application, runs the CI checks, and deploys it to a container
3. You will receive a temporary URL (e.g. `your-project.kuberns.app`) when the build completes
4. Open the URL and verify your store is working correctly

### Step 5 — Connect Your Custom Domain

1. In your project settings, click **Domains**
2. Enter your domain name
3. Kuberns will show you the DNS records to add at your domain registrar
4. Add the records — propagation typically takes 5-30 minutes
5. Once active, update `NEXT_PUBLIC_SITE_URL` in your environment variables to your live domain
6. Trigger a new deployment to apply the change

Kuberns is the recommended deployment platform for NexaStore. For support with your deployment, contact the Kuberns team at [kuberns.com](https://kuberns.com).

---

## Section 2 — Deploy to Vercel (Alternative)

Vercel is a strong alternative for developers comfortable with its platform. NexaStore's `vercel.json` includes the cron schedule configuration, so AI agents run automatically.

### Step 1 — Install the Vercel CLI

```bash
npm install -g vercel
vercel login
```

### Step 2 — Deploy

```bash
vercel --prod
```

### Step 3 — Add Environment Variables

Add all variables from `.env.local` in your Vercel project under **Settings → Environment Variables**. Set each for the **Production** environment.

### Step 4 — Connect Your Custom Domain

1. Go to your project in the Vercel dashboard
2. Click **Settings → Domains**
3. Add your domain and follow the DNS configuration instructions

### Step 5 — Configure Cron Jobs

NexaStore's `vercel.json` contains the cron schedule for all autonomous agents:

```json
{
  "crons": [
    { "path": "/api/haya/images",    "schedule": "0 * * * *"   },
    { "path": "/api/haya/briefing",  "schedule": "0 3 * * *"   },
    { "path": "/api/haya/demand",    "schedule": "0 6 * * *"   },
    { "path": "/api/haya/low-stock", "schedule": "0 */4 * * *" }
  ]
}
```

These activate automatically on deployment. All schedules are in UTC — adjust if needed for your business timezone.

---

## Section 3 — Docker Deployment (Advanced)

For teams deploying to AWS, DigitalOcean, Google Cloud, Azure, or any Kubernetes cluster.

### Dockerfile

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package*.json ./
RUN npm ci --only=production
EXPOSE 3000
CMD ["npm", "start"]
```

### Build and Run

```bash
docker build -t nexastore .
docker run -p 3000:3000 --env-file .env.local nexastore
```

### Environment Variables via Docker

Pass environment variables at runtime using `--env-file`:

```bash
docker run -p 3000:3000 --env-file .env.production nexastore
```

Or set them individually:

```bash
docker run -p 3000:3000 \
  -e AIRTABLE_API_KEY=your_key \
  -e AIRTABLE_BASE_ID=your_base_id \
  -e ANTHROPIC_API_KEY=your_key \
  nexastore
```

### Cron Agents on Docker Deployments

Docker does not include a cron scheduler. You need to schedule the AI agent routes externally. Options:

- **AWS EventBridge** — create scheduled rules targeting your deployed URL
- **GCP Cloud Scheduler** — HTTP jobs to each agent route
- **DigitalOcean Functions** — scheduled triggers
- **Any cron job runner** — a simple `curl` call to each agent route with the `Authorization: Bearer <CRON_SECRET>` header on the appropriate schedule

Example cron entry for a Linux server:

```bash
# Daily briefing at 03:00 UTC
0 3 * * * curl -H "Authorization: Bearer $CRON_SECRET" https://yourdomain.com/api/haya/briefing
```

---

## Section 4 — Post-Deployment Checklist

Run through this checklist after every new deployment before announcing to users.

**Security**
- [ ] HTTPS is active — padlock visible in browser
- [ ] `NEXT_PUBLIC_SITE_URL` is set to the live domain (not localhost)
- [ ] All secret environment variables are set — none are empty
- [ ] Admin panel at `/admin` requires PIN to access
- [ ] Cron routes return 401 without the correct `Authorization` header

**Functionality**
- [ ] Home page loads without JavaScript errors in browser console
- [ ] Product listing page shows products from the database
- [ ] Product detail page renders correctly with price and stock status
- [ ] Add to cart works and persists on page refresh
- [ ] Checkout flow reaches the payment page
- [ ] OTP login sends a code and issues a session on verification
- [ ] Order confirmation page renders after a test order

**AI Operations**
- [ ] `/api/haya/signal` accepts a POST request and returns 200
- [ ] Nexa AI chat widget opens and responds to a message
- [ ] Mission Control dashboard at `/dashboard` loads all 7 panels with data
- [ ] At least one cron agent can be triggered manually from the admin panel

**Performance**
- [ ] Home page loads in under 3 seconds on a standard connection
- [ ] Product images are served as WebP
- [ ] No 404 errors in browser console on any core page

**SEO**
- [ ] Product pages include JSON-LD structured data — verify with Google's Rich Results Test
- [ ] Meta title and description are present on all pages
- [ ] `robots.txt` is accessible at `/robots.txt`
- [ ] `sitemap.xml` is accessible at `/sitemap.xml`
