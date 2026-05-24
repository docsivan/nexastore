# NexaStore — Complete Feature List

Every capability included in v1.0.

---

## 1. Commerce Engine

- Product catalogue with unlimited SKUs
- Category tree with infinite nesting
- Advanced filtering and sorting across all catalogue dimensions
- Product variants and pricing tiers per customer segment
- Flash deals and promotional pricing with scheduled start and end times
- Crossed-out original price display alongside discounted price
- Stock level tracking with configurable low-stock thresholds
- Product badges — automatically assigned by AI based on demand and performance signals
- Image optimisation pipeline with WebP conversion
- SEO-optimised product URLs generated from product name and category

---

## 2. Nexa AI Brain

- 6 autonomous AI agents running independently on configurable schedules
- **CFO Agent** — revenue analysis, margin tracking, and P&L insights delivered daily
- **CMO Agent** — content creation, SEO writing, and product description generation
- **CRO Agent** — conversion optimisation, funnel analysis, and zero-result search gap detection
- **Inventory Agent** — stock intelligence, dead-stock detection, and reorder recommendations
- **Demand Agent** — trend detection, velocity analysis, and demand forecasting
- **Chat Agent** — customer conversations, product recommendations, and order support
- All agents write structured insights to a central intelligence table in the database
- Agents coordinate — one agent's output feeds the inputs of other agents
- Nexa AI can be renamed to any name via a single environment variable — see [docs/ai-training.md](./docs/ai-training.md)

---

## 3. Mission Control Dashboard

- 7-panel business intelligence dashboard for operations and management teams
- Revenue panel with sparkline chart and breakdown by category
- Margin panel with traffic-light indicators for products below threshold
- Inventory health panel showing stock status across all categories
- Customer intelligence panel — registrations, logins, and retention signals
- Conversion funnel panel — sessions, product views, cart additions, and completed orders
- SEO performance panel — structured data coverage and search visibility
- GEO and AEO citation monitoring panel for AI search engine visibility
- Dashboard auto-refreshes every 5 minutes
- Role-filtered views — each admin tier sees only the panels relevant to their role

---

## 4. Customer Experience

- Full bilingual support — English and Arabic included, extensible to any language without code changes
- OTP login via phone number — no password required, no phishing risk
- Customer registration with profile management
- Address manager with multiple saved delivery addresses and default selection
- Order history with invoice download
- WhatsApp order support integration for post-purchase queries
- AI-powered product search — finds products by intent, not just keyword match
- Personalised product recommendations driven by browse and purchase history
- Quick reorder from previous orders with one click

---

## 5. Administration

- Tiered admin roles — `super_admin`, `operations`, and `content` with distinct permissions
- bcrypt password hashing with 12 salt rounds on all stored credentials
- JWT sessions with 8-hour expiry and automatic renewal
- Account lockout after 5 consecutive failed attempts with configurable cooldown
- Order management panel with live status updates and fulfilment tracking
- Stock management panel with bulk quantity editing
- Customer management panel — view, search, and manage registered accounts
- Flash sale management — create, schedule, and end promotional events
- Banner and promotion management — update homepage banners without a developer
- Admin users management — add new admin users, reset credentials, and deactivate accounts

---

## 6. Autonomous Operations

- 12 cron agents running on automated schedules without human intervention
- Daily product badge tagging — Best Seller, New Arrival, Low Stock, On Sale
- Weekly automatic translation of new products into configured languages
- Nightly SEO content generation for new and updated product pages
- Weekly content refresh for product pages older than 90 days
- Daily inventory health analysis — low stock, dead stock, and reorder flagging
- Weekly citation monitoring for AI search engine visibility
- Nightly demand trend analysis updating the forecast signals table
- Morning business briefing dispatched via WhatsApp every day before business hours
- Every cron run is logged to the database with timestamp, agent identity, and outcome

---

## 7. Security

- JWT authentication required on all protected routes
- bcrypt password hashing — plaintext credentials never stored
- Rate limiting on all public-facing API routes with configurable thresholds
- CRON_SECRET bearer token protection on all autonomous agent routes
- Account lockout enforced after configurable number of failed authentication attempts
- OTP expiry enforcement — codes expire after 10 minutes
- Emergency admin reset via secure token for locked-out super administrators
- HTTPS enforced via deployment platform — all traffic encrypted in transit

---

## 8. Technical

- 24 production-ready API routes covering all commerce, auth, AI, and admin operations
- 15 database tables with a documented schema
- Next.js 14 App Router with full TypeScript strict mode
- Server-side rendering for all product and category pages
- Image proxy with sharp optimisation and WebP output
- Multi-currency support with configurable decimal precision and tax rates
- GitHub Actions CI/CD pipeline — type check, lint, build, and deploy on every push to main
- Docker-ready for deployment on any cloud provider
- Environment-based configuration throughout — no hardcoded values anywhere in the codebase

---

## 9. SEO and Discoverability

- Structured data schema — `Product`, `Organization`, and `LocalBusiness` JSON-LD on all relevant pages
- `HowTo` schema generated automatically on guide and tutorial pages
- `FAQ` schema generated automatically on content pages with question-and-answer sections
- `WebSite` SearchAction schema for Google Sitelinks Search Box
- `ItemList` schema on all category and listing pages
- Automated meta title and description tags generated from product data
- AI-generated pillar content pages targeting high-value search terms
- Internal linking engine connecting related products and categories
- Citation monitoring for AI search engines — ChatGPT, Perplexity, and Google AI Overviews
