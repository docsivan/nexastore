# Changelog

All notable changes to NexaStore are documented in this file.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] — May 2026

**Status: Production Ready**

This is the first stable release of NexaStore. All core systems are built, integrated, and verified in production.

### Added

**Commerce Engine**
- Complete storefront with unlimited product catalogue
- Category tree with multi-level nesting
- Advanced filtering — category, brand, price range, stock status, discount
- Product variants and segment-based pricing tiers
- Flash deal and promotional pricing system with scheduled activation
- Original price crossed-out display alongside discounted pricing
- Configurable low-stock threshold alerts
- AI-automated product badge tagging — Best Seller, New Arrival, Low Stock, On Sale
- Image optimisation pipeline with WebP conversion via sharp
- SEO-optimised product URL generation

**Nexa AI Brain**
- 6 autonomous specialist agents — CFO, CMO, CRO, Inventory, Demand, Chat
- Central intelligence table — all agent outputs written to a shared database table
- Agent coordination — outputs of one agent used as inputs by others
- AI assistant name configurable via `AI_ASSISTANT_NAME` environment variable
- Full agent decision audit log stored to database on every run

**Mission Control Dashboard**
- 7-panel business intelligence dashboard
- Revenue panel with sparkline and category breakdown
- Margin panel with traffic-light threshold indicators
- Inventory health panel with drill-down by category
- Customer intelligence panel — registrations, logins, retention
- Conversion funnel panel — sessions through to completed orders
- SEO performance panel — schema coverage and search visibility
- GEO and AEO citation monitoring panel
- 5-minute auto-refresh
- Role-filtered views per admin tier

**Authentication and Security**
- JWT authentication on all protected routes with 8-hour expiry
- OTP customer login via phone number
- bcrypt password hashing at 12 salt rounds
- Account lockout after 5 failed attempts
- Rate limiting on all public API routes
- CRON_SECRET bearer token protection on all autonomous routes
- Emergency admin reset via secure token
- Tiered admin roles — `super_admin`, `operations`, `content`

**Customer Experience**
- Full bilingual support — English and Arabic with RTL-aware layout
- Customer registration and profile management
- Address manager with multiple saved addresses
- Order history with invoice download
- WhatsApp order support integration
- AI-powered semantic product search
- Personalised product recommendations
- Quick reorder from order history

**Autonomous Operations**
- 12 cron agents on configurable automated schedules
- Daily product badge tagging
- Weekly product translation
- Nightly SEO content generation
- Weekly content refresh for aging pages
- Daily inventory health analysis
- Weekly AI search citation monitoring
- Nightly demand trend analysis
- Morning business briefing via WhatsApp
- Full cron run logging to database

**Payments**
- PayTabs payment gateway integration with HMAC-signed webhook verification
- Payment session creation and callback handling
- Order status updates on successful and failed payments

**Infrastructure**
- 24 production-ready API routes
- 15-table database schema
- Next.js 14 App Router with TypeScript strict mode
- Server-side rendering for all product and category pages
- GitHub Actions CI/CD pipeline — type check, lint, build, deploy
- Docker support for any cloud deployment
- Vercel cron scheduling via `vercel.json`
- Full environment variable configuration — no hardcoded values

**Documentation**
- `FEATURES.md` — complete feature list
- `CHANGELOG.md` — this file
- `docs/setup.md` — setup guide for business owners and developers
- `docs/deployment.md` — deployment guide for Kuberns, Vercel, and Docker
- `docs/ai-training.md` — guide to configuring Nexa AI for your business
- `docs/white-label.md` — complete white-label customisation guide
- `docs/environment.md` — complete environment variable reference

---

## [Unreleased]

Features planned for upcoming releases will be documented here before release.

- Stripe payment gateway integration
- Razorpay payment gateway integration
- Multi-vendor marketplace mode
- Subscription and recurring order support
- Native mobile app (React Native)
- Advanced analytics export to CSV and Google Sheets
