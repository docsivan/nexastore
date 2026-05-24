# Environment Variables Reference

Complete guide to every configuration variable in NexaStore.

---

## Setup

Create a `.env.local` file in the root of your project:

```bash
cp .env.example .env.local
```

Open `.env.local` in any text editor and fill in the values for your deployment. Never commit `.env.local` to GitHub — it is already listed in `.gitignore`.

All variables without `NEXT_PUBLIC_` prefix are server-side only and never exposed to the browser. Variables prefixed with `NEXT_PUBLIC_` are available in both server and client code — do not put secrets in these variables.

---

## Group 1 — Database

### `AIRTABLE_API_KEY`

Your Airtable personal access token.

- Get it from: [airtable.com/create/tokens](https://airtable.com/create/tokens)
- Required scopes: `data.records:read`, `data.records:write`
- Format: starts with `pat_`
- Required: yes

```
AIRTABLE_API_KEY=pat_xxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### `AIRTABLE_BASE_ID`

The ID of your Airtable base where the 15 NexaStore tables are created.

- Found in your Airtable base URL: `airtable.com/[BASE_ID]/...`
- Format: starts with `app`
- Required: yes

```
AIRTABLE_BASE_ID=appXXXXXXXXXXXXXX
```

---

## Group 2 — AI Services

### `ANTHROPIC_API_KEY`

API key for Anthropic Claude — powers the Nexa AI chat agent, product description generation, and CMO Agent content tasks.

- Get it from: [console.anthropic.com](https://console.anthropic.com)
- Format: starts with `sk-ant-`
- Required: yes

```
ANTHROPIC_API_KEY=sk-ant-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

### `GEMINI_API_KEY`

API key for Google Gemini Flash 2.0 — powers AI search, demand forecasting, daily briefings, and auto-translation.

- Get it from: [aistudio.google.com](https://aistudio.google.com)
- Format: starts with `AIzaSy`
- Required: yes

```
GEMINI_API_KEY=AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

---

## Group 3 — Authentication

### `ADMIN_JWT_SECRET`

Secret key used to sign and verify admin JWT sessions. Must be at least 32 characters.

- Generate with: `openssl rand -base64 32`
- Never reuse this value across deployments
- Required: yes

```
ADMIN_JWT_SECRET=your_minimum_32_character_random_string_here
```

### `CRON_SECRET`

Bearer token that protects all 12 autonomous cron agent routes. Any request to a cron route without this token in the `Authorization` header receives a 401 response.

- Generate with: `openssl rand -base64 32`
- Set the same value in your Vercel or Kuberns cron configuration
- Required: yes

```
CRON_SECRET=your_minimum_32_character_random_string_here
```

### `EMERGENCY_TOKEN`

Secure token for the emergency admin reset route. Used when a super_admin account is locked out and cannot be recovered through normal means.

- Generate with: `openssl rand -base64 32`
- Store this somewhere safe outside the codebase — if lost, generate a new one and redeploy
- Required: yes

```
EMERGENCY_TOKEN=your_minimum_32_character_random_string_here
```

---

## Group 4 — Email

### `SMTP_HOST`

Your SMTP server hostname for outgoing email (OTP delivery, order confirmations).

```
SMTP_HOST=smtp.gmail.com
```

### `SMTP_PORT`

SMTP port. Use `587` for TLS (recommended) or `465` for SSL.

```
SMTP_PORT=587
```

### `SMTP_USER`

The email address used to authenticate with your SMTP server.

```
SMTP_USER=noreply@yourdomain.com
```

### `SMTP_PASS`

The password or app password for your SMTP account.

For Gmail, generate an App Password at `myaccount.google.com/apppasswords` — do not use your main Gmail password.

```
SMTP_PASS=your_smtp_password_or_app_password
```

### `SMTP_FROM`

The display name and email address shown to recipients of outgoing emails.

```
SMTP_FROM="Your Store Name <noreply@yourdomain.com>"
```

---

## Group 5 — Payments

### `PAYTABS_PROFILE_ID`

Your PayTabs merchant profile ID, found in the PayTabs merchant dashboard.

```
PAYTABS_PROFILE_ID=12345
```

### `PAYTABS_SERVER_KEY`

Your PayTabs server key, used for payment session creation and HMAC webhook signature verification.

```
PAYTABS_SERVER_KEY=SXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

---

## Group 6 — Platform

### `NEXT_PUBLIC_SITE_URL`

The full public URL of your deployed platform, including the protocol. Used for generating absolute URLs in structured data, payment callback URLs, and email links.

- Do not include a trailing slash
- Use `http://localhost:3000` for local development
- Required: yes

```
NEXT_PUBLIC_SITE_URL=https://yourdomain.com
```

### `NEXT_PUBLIC_PLATFORM_NAME`

Your platform's display name. Appears in the browser tab title, email communications, and AI-generated content.

```
NEXT_PUBLIC_PLATFORM_NAME=MyStore
```

### `NEXT_PUBLIC_CURRENCY`

The currency symbol displayed in the storefront.

```
NEXT_PUBLIC_CURRENCY=$
```

### `NEXT_PUBLIC_CURRENCY_CODE`

The ISO 4217 currency code. Used in payment processing and structured data.

```
NEXT_PUBLIC_CURRENCY_CODE=USD
```

### `NEXT_PUBLIC_CURRENCY_DECIMALS`

The number of decimal places used in price display.

- Use `2` for most currencies (USD, EUR, GBP)
- Use `0` for zero-decimal currencies (JPY, KRW)
- Use `3` for currencies with three decimal places

```
NEXT_PUBLIC_CURRENCY_DECIMALS=2
```

### `NEXT_PUBLIC_TAX_RATE`

The tax rate applied at checkout, expressed as a decimal.

- `0.20` = 20%
- `0.05` = 5%
- `0` = no tax

```
NEXT_PUBLIC_TAX_RATE=0.20
```

### `NEXT_PUBLIC_TAX_LABEL`

The label shown next to the tax line at checkout.

```
NEXT_PUBLIC_TAX_LABEL=VAT
```

### `NEXT_PUBLIC_DEFAULT_LANGUAGE`

The default language for the storefront. Supported values in v1.0: `en` and `ar`.

```
NEXT_PUBLIC_DEFAULT_LANGUAGE=en
```

### `AI_ASSISTANT_NAME`

The customer-facing name of your AI assistant. Appears in the chat widget, automated messages, and AI-generated content.

```
AI_ASSISTANT_NAME=Nexa AI
```

---

## Group 7 — Integrations

### `MAKE_WEBHOOK_URL`

Make.com webhook URL used to dispatch OTP messages and notifications (including the morning briefing and low-stock alerts).

Create a Make.com scenario with a Custom Webhook trigger and connect it to your preferred messaging provider (WhatsApp Business, SMS, email).

```
MAKE_WEBHOOK_URL=https://hook.make.com/xxxxxxxxxxxxxxxxxxxx
```

### `WHATSAPP_NUMBER`

The WhatsApp Business number displayed on the order confirmation page and accessible via the floating support button.

- Include the country code without a leading `+`
- Example: `441234567890` for a number beginning with `44`

```
WHATSAPP_NUMBER=441234567890
```

### `GOOGLE_CSE_KEY`

Google Custom Search API key. Used by the SEO content agent to research search trends and competitor content.

- Get it from: [console.cloud.google.com](https://console.cloud.google.com) — enable the Custom Search JSON API

```
GOOGLE_CSE_KEY=AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

### `GOOGLE_CSE_ID`

Google Programmable Search Engine ID. Used alongside `GOOGLE_CSE_KEY` for content research.

- Create a search engine at: [programmablesearchengine.google.com](https://programmablesearchengine.google.com)

```
GOOGLE_CSE_ID=XXXXXXXXXXXXXXXXX
```

### `TRENDS_SERVER_URL`

URL of the Google Trends Flask server used by the Demand Agent for trend signal data.

- Runs as a separate lightweight service — setup instructions in `scripts/trends-server/README.md`
- Optional — the Demand Agent operates without it but trend signals will not be included

```
TRENDS_SERVER_URL=https://trends.yourdomain.com
```

---

## Complete `.env.local` Template

Copy this template to `.env.local` and fill in your values:

```bash
# ── Database ──────────────────────────────────────────────────
AIRTABLE_API_KEY=
AIRTABLE_BASE_ID=

# ── AI Services ───────────────────────────────────────────────
ANTHROPIC_API_KEY=
GEMINI_API_KEY=

# ── Authentication ────────────────────────────────────────────
ADMIN_JWT_SECRET=
CRON_SECRET=
EMERGENCY_TOKEN=

# ── Email ─────────────────────────────────────────────────────
SMTP_HOST=
SMTP_PORT=587
SMTP_USER=
SMTP_PASS=
SMTP_FROM=

# ── Payments ──────────────────────────────────────────────────
PAYTABS_PROFILE_ID=
PAYTABS_SERVER_KEY=

# ── Platform ──────────────────────────────────────────────────
NEXT_PUBLIC_SITE_URL=http://localhost:3000
NEXT_PUBLIC_PLATFORM_NAME=MyStore
NEXT_PUBLIC_CURRENCY=$
NEXT_PUBLIC_CURRENCY_CODE=USD
NEXT_PUBLIC_CURRENCY_DECIMALS=2
NEXT_PUBLIC_TAX_RATE=0.00
NEXT_PUBLIC_TAX_LABEL=Tax
NEXT_PUBLIC_DEFAULT_LANGUAGE=en
AI_ASSISTANT_NAME=Nexa AI

# ── Integrations ──────────────────────────────────────────────
MAKE_WEBHOOK_URL=
WHATSAPP_NUMBER=
GOOGLE_CSE_KEY=
GOOGLE_CSE_ID=
TRENDS_SERVER_URL=
```
