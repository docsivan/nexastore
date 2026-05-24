# Configuring Nexa AI for Your Business

Nexa AI ships ready to use. To make it work specifically for your business, you configure it with your business context, your product knowledge, and your preferred communication style. This takes approximately 30 minutes and transforms Nexa AI from a generic assistant into an expert on your specific business. Nexa AI can be renamed to anything — see Step 1.

---

## Step 1 — Rename Your AI Assistant

Your AI assistant's name appears in the chat widget, in automated messages, and in any customer-facing content it generates. Change it to anything that fits your brand.

1. Open `.env.local`
2. Find the line: `AI_ASSISTANT_NAME=Nexa AI`
3. Replace `Nexa AI` with your chosen name — for example: `Aria`, `Max`, `Sage`, or `[YourBrand] Assistant`
4. Save the file and restart the development server

Every customer-facing reference updates automatically. No code changes required.

---

## Step 2 — Set Your Business Context

The business context is the most important configuration you will set. It tells your AI assistant what your business does, who your customers are, and how to communicate with them.

1. Open `lib/haya.ts`
2. Find the `BUSINESS_CONTEXT` variable near the top of the file
3. Replace the placeholder text with your own business description

Write your business context as if you are briefing a knowledgeable new employee on their first day. Include:

- What your business sells — be specific about product types and quality level
- Who your customers are — their role, their knowledge level, what problems they are solving
- Your tone of voice — formal and technical, or warm and approachable
- Any important facts customers frequently ask about — delivery times, return policy, minimum orders
- What your business does not do — so the AI can redirect customers appropriately

Keep the business context under 500 words. Shorter and specific is more effective than long and vague.

**Example:**

```
We sell professional audio equipment to recording studios, live sound engineers, and serious home studio owners. 
Our customers are knowledgeable — they know the difference between condenser and dynamic microphones and do not 
need basic explanations. They value technical accuracy over simplicity. 

We carry microphones, preamps, audio interfaces, studio monitors, and cables from professional brands. 
We do not carry consumer-grade or budget equipment. 

Our tone is direct and knowledgeable — like talking to a well-informed colleague, not a salesperson.

Delivery is 3-5 business days on standard orders. We offer 30-day returns on unopened products.
Minimum order for trade accounts is 500 in the account currency.
```

---

## Step 3 — Configure Your AI Personality

In `lib/haya.ts`, find the `AI_CONFIG` object and adjust the following settings:

**Tone**
- `formal` — professional, full sentences, no contractions
- `conversational` — approachable, natural language, contractions allowed
- `technical` — precise, specification-focused, assumes domain knowledge

**Response length**
- `brief` — answers in 1-3 sentences, links to product for detail
- `detailed` — thorough answers with context and reasoning
- `adaptive` — brief for simple questions, detailed for complex ones (recommended)

**Out-of-scope handling**
- Set `OUT_OF_SCOPE_RESPONSE` to the message your AI gives when asked something unrelated to your products — for example: `"That's outside what I can help with. For anything related to [your product range], I'm here."` 

---

## Step 4 — Add Product Knowledge

Your AI reads your product catalogue automatically from the database. For most businesses this is sufficient — the assistant can answer questions about any product in your catalogue without additional configuration.

For businesses with specialist products that require technical knowledge beyond the product listing, add a `PRODUCT_CONTEXT` variable to `.env.local`:

```
PRODUCT_CONTEXT=Your technical context here
```

Use this for:

- Safety specifications or compliance standards relevant to your products
- Technical terminology that differs from common usage in your industry
- Compatibility matrices — which products work together, which do not
- Common technical questions and their correct answers
- Anything a new product specialist would need to know that is not obvious from a product listing

Keep `PRODUCT_CONTEXT` under 1,000 words. If your product knowledge is extensive, prioritise the information that customers ask about most frequently.

---

## Step 5 — Configure the Disclaimer System

The disclaimer system displays a confirmation card before your AI responds to topics that may require professional advice or carry sensitivity in your context. The user must acknowledge the disclaimer before the response is shown.

This is important for businesses selling products where customers might seek guidance that should come from a licensed professional.

1. Open `lib/haya.ts`
2. Find the `DISCLAIMER_KEYWORDS` array
3. Add the keywords relevant to your business that should trigger a disclaimer

**Examples by business type:**

For businesses where regulatory or professional advice is relevant:
```
DISCLAIMER_KEYWORDS: ['clinical', 'prescription', 'diagnos', 'treatment', 'medical advice']
```

For businesses where legal advice could be sought:
```
DISCLAIMER_KEYWORDS: ['legal advice', 'contract', 'liability', 'regulatory', 'compliance']
```

For businesses where financial advice could be sought:
```
DISCLAIMER_KEYWORDS: ['invest', 'portfolio', 'returns', 'financial advice', 'tax']
```

For most general retail businesses, you can leave `DISCLAIMER_KEYWORDS` as an empty array — no disclaimers will be shown.

---

## Step 6 — Test Your AI

Before going live, run through at least 20 test conversations that represent real customer interactions.

1. Start your development server with `npm run dev`
2. Open your store at `http://localhost:3000`
3. Click the chat widget in the bottom right corner
4. Work through the following test categories:

**Product questions** — ask about specific products in your catalogue, their specifications, and their availability.

**Comparison questions** — ask the AI to compare two similar products. Verify it gives accurate, helpful comparisons.

**Out-of-scope questions** — ask something completely unrelated to your business. Verify the AI redirects gracefully.

**Edge cases** — ask about a product you do not carry. Verify the AI does not invent a product or give misleading information.

**Disclaimer triggers** — if you configured disclaimer keywords, ask a question containing those keywords. Verify the disclaimer card appears.

**Language** — if you have configured a second language, test the same questions in that language.

Adjust `BUSINESS_CONTEXT` if responses feel too generic. The most common cause of poor AI responses is a business context that is too short or too vague.

---

## Step 7 — Common Issues and Fixes

**AI gives generic responses that do not mention your specific products**
Your `BUSINESS_CONTEXT` is too vague. Add more specific detail about what you sell, who your customers are, and what makes your product range distinctive.

**AI mentions incorrect product names or specifications**
Check that your product data in Airtable is accurate and complete. The AI reads directly from the database — incorrect data produces incorrect answers.

**Disclaimer not triggering when expected**
Check that the keyword you expect to trigger it is spelled correctly in `DISCLAIMER_KEYWORDS`. The match is case-insensitive but must match the keyword exactly as a substring of the user's message.

**AI responds in the wrong language**
Check the `NEXT_PUBLIC_DEFAULT_LANGUAGE` variable in `.env.local` and the language configuration in `context/LanguageContext.tsx`. The AI uses the active language of the session.

**AI assistant name not updating**
Ensure you restarted the development server after changing `AI_ASSISTANT_NAME` in `.env.local`. Environment variable changes require a server restart to take effect.

**AI gives overly cautious responses on normal product questions**
Your `DISCLAIMER_KEYWORDS` array may be too broad. Review the keywords and remove any that are triggering on routine product queries.
