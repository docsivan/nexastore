# White Label Customisation Guide

Deploy NexaStore as your own branded platform in under an hour.

NexaStore is designed to be completely rebranded. There are no NexaStore logos, watermarks, or customer-facing references in the platform. Every name, colour, font, currency, language, and payment provider is configurable without touching the core application code.

---

## Section 1 — Brand Identity

### Platform Name

Set your platform name in `.env.local`:

```
NEXT_PUBLIC_PLATFORM_NAME=YourStoreName
```

This name appears in the browser tab title, email communications, and any platform-generated content. It does not require a code change or rebuild to update.

### AI Assistant Name

Set your AI assistant's customer-facing name:

```
AI_ASSISTANT_NAME=Aria
```

See [docs/ai-training.md](./ai-training.md) for full AI configuration instructions.

### Logo

Replace the default logo file with your own:

1. Export your logo as an SVG file
2. Name it `logo.svg`
3. Replace the file at `public/logo.svg`
4. The Navbar and Footer components reference this path — your logo appears automatically

For a PNG or WebP logo, update the `src` attribute in `components/global/Navbar.tsx` and `components/global/Footer.tsx`.

### Favicon

1. Create a 32x32 pixel ICO file from your logo
2. Name it `favicon.ico`
3. Replace the file at `public/favicon.ico`

Online tools such as favicon.io can convert any image to ICO format.

### Primary Colour

Open `tailwind.config.ts` and update the `primary` token:

```js
colors: {
  primary: '#003B73',        // Replace with your primary brand colour
  'primary-light': '#0052A3' // Replace with a lighter hover variant
}
```

The primary colour applies to the navigation bar, headings, primary call-to-action buttons, and link text throughout the platform.

### Accent Colour

The accent colour is used for add-to-cart buttons, success states, and availability badges:

```js
colors: {
  accent: '#4CAF50'  // Replace with your accent colour
}
```

### Fonts

Open `app/layout.tsx` and update the Google Fonts imports. The default fonts are Outfit (headings) and IBM Plex Sans (body). Replace them with any Google Fonts or self-hosted font files.

If switching to self-hosted fonts, place font files in `public/fonts/` and update the `@font-face` declarations in `app/globals.css`.

---

## Section 2 — Currency and Pricing

All currency configuration is done via environment variables. One set of variables controls every price display, tax calculation, and currency formatting across the entire platform.

```
NEXT_PUBLIC_CURRENCY=€
NEXT_PUBLIC_CURRENCY_CODE=EUR
NEXT_PUBLIC_CURRENCY_DECIMALS=2
NEXT_PUBLIC_TAX_RATE=0.20
NEXT_PUBLIC_TAX_LABEL=VAT
```

| Variable | Purpose | Example values |
|---|---|---|
| `NEXT_PUBLIC_CURRENCY` | Currency symbol displayed before or after the price | `$`, `£`, `€`, `¥` |
| `NEXT_PUBLIC_CURRENCY_CODE` | ISO 4217 currency code | `USD`, `GBP`, `EUR`, `JPY` |
| `NEXT_PUBLIC_CURRENCY_DECIMALS` | Decimal places in price display | `2` for most currencies, `0` for zero-decimal currencies |
| `NEXT_PUBLIC_TAX_RATE` | Tax rate as a decimal | `0.20` for 20%, `0.05` for 5%, `0` for no tax |
| `NEXT_PUBLIC_TAX_LABEL` | Tax label shown at checkout | `VAT`, `GST`, `Sales Tax`, `Tax` |

---

## Section 3 — Language

### Changing the Default Language

```
NEXT_PUBLIC_DEFAULT_LANGUAGE=en
```

Supported values in v1.0: `en` (English), `ar` (Arabic).

### Adding a New Language

1. Open `context/LanguageContext.tsx`
2. Find the `translations` object
3. Add a new language key alongside `en` and `ar`:

```ts
const translations = {
  en: { ... },
  ar: { ... },
  fr: {
    nav_products: 'Produits',
    nav_cart: 'Panier',
    // ... all keys
  }
}
```

4. Add the new language code to the language switcher in `components/global/Navbar.tsx`
5. Set `NEXT_PUBLIC_DEFAULT_LANGUAGE=fr` in `.env.local`

### RTL Support

RTL layout is automatic for Arabic. For other RTL languages (Persian, Hebrew, Urdu), add the language code to the `RTL_LANGUAGES` array in `context/LanguageContext.tsx`:

```ts
const RTL_LANGUAGES = ['ar', 'fa', 'he', 'ur']
```

The layout system applies `dir="rtl"` to the document and reverses all directional CSS automatically.

---

## Section 4 — Payment Gateway

### PayTabs

PayTabs is the default payment gateway. Configure it with your merchant credentials:

```
PAYTABS_PROFILE_ID=your_profile_id
PAYTABS_SERVER_KEY=your_server_key
```

### Stripe

To replace PayTabs with Stripe:

1. Install the Stripe SDK: `npm install stripe @stripe/stripe-js`
2. Replace `app/api/payment/create/route.ts` with a Stripe Payment Intent creation
3. Replace `app/api/payment/callback/route.ts` with Stripe webhook verification using `stripe.webhooks.constructEvent`
4. Update the checkout page to use Stripe Elements or redirect to Stripe Checkout

### Razorpay

To replace PayTabs with Razorpay:

1. Install the Razorpay SDK: `npm install razorpay`
2. Replace the payment creation route with a Razorpay Order creation
3. Replace the callback route with Razorpay signature verification
4. Update the checkout component to load the Razorpay checkout script

### Any Other Gateway

The payment route structure is intentionally simple:

- `POST /api/payment/create` — receives an order ID, returns a redirect URL or client secret
- `POST /api/payment/callback` — receives a webhook, verifies the signature, updates the order status

Any payment gateway that follows this pattern can be integrated by replacing the contents of these two route files.

---

## Section 5 — Removing NexaStore References

The customer-facing platform contains no NexaStore branding by default. The name only appears in:

- `package.json` — the `name` field
- `README.md` — documentation
- Code comments

To do a full rebrand across all files, a find-and-replace script is provided:

```bash
npm run rebrand -- --name="YourPlatformName"
```

This script runs `scripts/rebrand.ts`, which performs a case-sensitive and case-insensitive replacement of `nexastore`, `NexaStore`, and `NEXASTORE` across all non-documentation files.

Alternatively, use your editor's global find-and-replace with the following:

| Find | Replace with |
|---|---|
| `NexaStore` | `YourPlatformName` |
| `nexastore` | `yourplatformname` |
| `NEXASTORE` | `YOURPLATFORMNAME` |

---

## Section 6 — What You Can Sell

NexaStore has no product type restrictions. The commerce engine is category-agnostic.

**Physical products** — the default mode. Stock tracking, low-stock alerts, and inventory management are all built for physical goods with quantity management.

**Digital products** — set `stock_qty` to a high number (e.g. 999999) to simulate unlimited stock. Disable the delivery address step in the checkout flow by setting `NEXT_PUBLIC_REQUIRE_DELIVERY_ADDRESS=false`.

**Services** — use the product catalogue to list services with configurable pricing tiers. Disable stock tracking per product with the `track_stock: false` flag in the Products table.

**B2B wholesale** — enable contract pricing tiers by configuring the `pricing_tier` field in the Customers table. Each customer account can be assigned a tier that determines which price column is displayed throughout the storefront.

**B2C retail** — the default storefront mode. Standard pricing, promotional discounts, and flash sales are all available without any additional configuration.

**Mixed B2B and B2C** — both modes operate simultaneously from the same platform. Registered accounts with a `pricing_tier` set see their contract prices. Guest and standard customer accounts see the default retail price.
