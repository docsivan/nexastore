<div align="center">

# NexaStore

### The open platform for healthcare commerce — deployable anywhere, for any market.

[![Next.js](https://img.shields.io/badge/Next.js-14-black?logo=next.js&logoColor=white)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://typescriptlang.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![AI Powered](https://img.shields.io/badge/AI-Powered-8B5CF6?logo=anthropic&logoColor=white)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-22C55E)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-F59E0B)](./CONTRIBUTING.md)

<br/>

**[Live Demo](https://demo.nexastore.io) · [Docs](#) · [Deploy on Kuberns](#deploy-on-kuberns) · [Report a Bug](#)**

</div>

---

## What is NexaStore?

NexaStore is a fully open, white-label AI-Powered Healthcare Commerce Platform built for hospitals, clinics, distributors, and medical suppliers worldwide. It ships with a production-ready storefront, autonomous AI agents that run your operations around the clock, and a one-command deployment path — so you go from zero to a live healthcare commerce platform in minutes, not months. Fork it, brand it, and deploy it for any healthcare market on the planet.

---

## Key Features

- **Haya AI** — 6 autonomous specialist agents (CFO, CMO, CRO, Inventory, Demand, Chat) that manage operations, forecast demand, and engage customers without human intervention
- **Semantic AI search** — buyers find products by clinical intent, not just keywords, powered by Google Gemini Flash 2.0
- **24 production-ready API routes** — auth, payments, orders, AI enrichment, and autonomous agent endpoints out of the box
- **Fully bilingual** — English and Arabic built in, RTL-aware layout, extensible to any language
- **Deploy anywhere** — Vercel, Docker, Kubernetes — one codebase, any infrastructure, any market

---

## Who Is It For?

| Audience | Use Case |
|---|---|
| **Healthcare Distributors** | Launch a branded digital sales channel with AI-powered operations |
| **Hospital & Clinic Groups** | Centralise supply purchasing across facilities with real-time inventory intelligence |
| **Medical Suppliers** | Sell direct to buyers with AI-generated product content and semantic search |
| **Health Tech Companies** | Embed or licence a battle-tested commerce engine into your platform |
| **Developers & Agencies** | Build and deploy custom healthcare commerce storefronts for clients globally |

---

## Tech Stack

[![Next.js](https://img.shields.io/badge/Next.js_14-App_Router-black?logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-Strict-3178C6?logo=typescript)](https://typescriptlang.org)
[![Tailwind](https://img.shields.io/badge/Tailwind_CSS-3-06B6D4?logo=tailwindcss)](https://tailwindcss.com)
[![Airtable](https://img.shields.io/badge/Airtable-REST_API-18BFFF?logo=airtable)](https://airtable.com)
[![Gemini](https://img.shields.io/badge/Google-Gemini_Flash_2.0-4285F4?logo=google)](https://ai.google.dev)
[![Claude](https://img.shields.io/badge/Anthropic-Claude_Haiku_%2B_Sonnet-8B5CF6)](https://anthropic.com)
[![Vercel](https://img.shields.io/badge/Vercel-Deployment-black?logo=vercel)](https://vercel.com)
[![PayTabs](https://img.shields.io/badge/PayTabs-Payments-0EA5E9)](https://paytabs.com)

---

## Quick Start

```bash
git clone https://github.com/docsivan/nexastore.git
cd nexastore
cp .env.example .env.local   # add your API keys
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) — your NexaStore is running.

> See [Environment Variables](#) in the docs for the full `.env.local` reference.

---

## Deploy

### Deploy on Kuberns

NexaStore's recommended hosting partner. One-click container deployment with auto-scaling, built-in SSL, and managed cron scheduling for all 12 AI agents.

[![Deploy on Kuberns](https://img.shields.io/badge/Deploy_on-Kuberns-6366F1?style=for-the-badge&logo=kubernetes&logoColor=white)](#)

### Deploy on Vercel

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/docsivan/nexastore)

---

## White Label

NexaStore is designed to become your brand, not ours. Swap the logo, update the colour tokens in `tailwind.config.ts`, point the environment variables at your Airtable base and payment gateway, and your white-label healthcare commerce platform is live — no forks to maintain, no vendor lock-in, no licensing fees. Every market, every currency, every language: the same clean codebase.

---

## Contributing

NexaStore is open to contributions from developers, healthcare professionals, and businesses worldwide.

- **Bug reports** — open an issue with reproduction steps
- **Feature requests** — open a discussion before building
- **Pull requests** — all PRs welcome; please read [CONTRIBUTING.md](#) first

```bash
# Fork the repo, then:
git checkout -b feature/your-feature
git commit -m "feat: your feature"
git push origin feature/your-feature
# Open a pull request
```

---

## License

MIT — free to use, modify, and deploy commercially.

See [LICENSE](./LICENSE) for the full text.

---

<div align="center">

**NexaStore** — Open healthcare commerce infrastructure for the world.

*Fork it. Brand it. Ship it.*

</div>
