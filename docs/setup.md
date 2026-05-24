# Setup Guide

Get NexaStore running locally in under 30 minutes.

---

## Part A — For Business Owners

No technical knowledge required. Follow these steps in order.

---

### What You Need Before Starting

Create accounts on each of these services before you begin. All have free tiers that are sufficient to get started.

| Service | What it does | Where to sign up |
|---|---|---|
| **GitHub** | Hosts your code | github.com |
| **Airtable** | Your store database | airtable.com |
| **Google AI Studio** | Powers AI search and content | aistudio.google.com |
| **Anthropic Console** | Powers your AI assistant | console.anthropic.com |
| **Make.com** | Sends OTP messages and notifications | make.com |
| **Kuberns** | Hosts your live store | kuberns.com |

Keep all the API keys and credentials you generate — you will need them in Step 5.

---

### Step 1 — Get the Code

1. Go to [github.com/docsivan/nexastore](https://github.com/docsivan/nexastore)
2. Click the green **Code** button
3. Click **Download ZIP**
4. Unzip the file to a folder on your computer — for example, your Desktop

No git knowledge required.

---

### Step 2 — Install Node.js

Node.js is the software that runs NexaStore on your computer.

1. Go to [nodejs.org](https://nodejs.org)
2. Download the **LTS** version (the one labelled "Recommended For Most Users")
3. Run the installer and follow the prompts
4. When the installation is complete, continue to Step 3

---

### Step 3 — Open Terminal or Command Prompt

**On Mac:** Press `Command + Space`, type `Terminal`, press Enter.

**On Windows:** Press `Windows key`, type `cmd`, press Enter.

Then navigate to your NexaStore folder:

```
cd Desktop/nexastore-main
```

Adjust the path if you unzipped it to a different location.

---

### Step 4 — Install Dependencies

In your Terminal or Command Prompt, type the following and press Enter:

```
npm install
```

This downloads everything NexaStore needs to run. It may take 1-2 minutes. Wait until you see the cursor return.

---

### Step 5 — Create Your Configuration File

NexaStore reads your API keys and settings from a file called `.env.local`.

1. In your NexaStore folder, find the file named `.env.example`
2. Make a copy of it and rename the copy to `.env.local`
3. Open `.env.local` in any text editor (Notepad on Windows, TextEdit on Mac)
4. Replace each placeholder value with your real credentials from the accounts you created in Step 1
5. Save the file

See [docs/environment.md](./environment.md) for a description of every variable.

---

### Step 6 — Start Your Store

In Terminal or Command Prompt, type the following and press Enter:

```
npm run dev
```

After a few seconds you will see a message saying the server is ready. Open your browser and go to:

```
http://localhost:3000
```

Your store is now running on your computer.

---

### Step 7 — Check Everything Works

Before going live, verify the following:

- [ ] Home page loads without errors
- [ ] At least one product appears on the products page
- [ ] Product detail page opens and shows correct information
- [ ] Add to cart button works and cart updates
- [ ] Search bar returns results when you type a product name
- [ ] Language toggle switches between English and another language
- [ ] Login page loads and OTP form is visible
- [ ] Admin panel is accessible at `/admin` and PIN login works
- [ ] Mission Control dashboard is accessible at `/dashboard` after logging in
- [ ] WhatsApp support button is visible and links to the correct number

If anything does not work, see the troubleshooting section in [docs/setup.md](./setup.md) or open an issue on GitHub.

---

### Step 8 — Deploy Live

When you are ready to make your store publicly accessible, follow the deployment guide:

[docs/deployment.md](./deployment.md)

The recommended path for business owners with no technical background is **Kuberns** — it handles everything automatically.

---

## Part B — For Developers

---

### Prerequisites

- Node.js 18 or later
- npm 9 or later
- git
- An Airtable account with a base configured using the 15-table schema
- API keys for Anthropic Claude and Google Gemini

---

### Clone the Repository

```bash
git clone https://github.com/docsivan/nexastore.git
cd nexastore
```

---

### Install Dependencies

```bash
npm install
```

---

### Environment Configuration

Copy the example environment file and populate all values:

```bash
cp .env.example .env.local
```

See [docs/environment.md](./environment.md) for the complete variable reference including required scopes and generation instructions for secrets.

---

### Database Setup

NexaStore uses Airtable as its database. You need to create a base with 15 tables matching the documented schema.

1. Create a new Airtable base
2. Create each of the following tables with the specified fields:

| Table | Purpose |
|---|---|
| `Products` | Product catalogue |
| `Categories` | Category hierarchy |
| `Orders` | Order records |
| `Order_Items` | Line items per order |
| `Customers` | Registered customer accounts |
| `Addresses` | Customer delivery addresses |
| `Sessions` | JWT session tracking |
| `Cart_Validates` | Server-side cart snapshots |
| `Haya_Log` | AI behaviour signals |
| `Haya_Signals` | Aggregated AI intelligence |
| `Briefings` | Daily briefing records |
| `Low_Stock_Alerts` | Inventory alert records |
| `AEO_Citations` | AI search citation content |
| `Schema_Cache` | JSON-LD structured data cache |
| `Admin_Sessions` | Admin authentication audit log |

Full field-level schema documentation is in `docs/schema.md`.

3. Copy the Base ID from the Airtable URL — it starts with `app`
4. Generate a Personal Access Token at `airtable.com/create/tokens` with `data.records:read` and `data.records:write` scopes
5. Set both values in `.env.local`

---

### Run Development Server

```bash
npm run dev
```

Available at [http://localhost:3000](http://localhost:3000).

---

### Run Production Build

```bash
npm run build
npm start
```

---

### Type Check and Lint

```bash
npm run type-check    # TypeScript strict validation
npm run lint          # ESLint
```

---

### Seed Staging Data

```bash
npm run seed
```

Provisions products, orders, and customers across all 15 tables for a realistic demo environment.

---

### CI/CD — GitHub Actions

The `.github/workflows/ci.yml` pipeline runs on every push to `main`:

1. TypeScript type check
2. ESLint lint pass
3. Next.js production build
4. Automatic deployment to Vercel or Kuberns on success

Set `VERCEL_TOKEN`, `VERCEL_ORG_ID`, and `VERCEL_PROJECT_ID` as GitHub repository secrets to enable automatic deployment.
