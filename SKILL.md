---
name: shopify-gmc-auditor
description: >-
  Audit a Shopify store for Google Merchant Center (GMC) compliance. Checks store structure, policies, product data, pricing consistency, trust signals, and technical SEO. Maps every finding to a specific Google policy with risk scoring (100-point scale) and generates a professional HTML dashboard report. Triggers: "GMC audit", "Google Merchant Center compliance", "Shopify GMC check", "GMC policy review", "Google Shopping audit", "审核 GMC", "GMC 合规检查".
metadata:
  version: "1.0.0"
  author: Gavin
---

# Shopify GMC Compliance Auditor

Five-layer audit pipeline for Google Merchant Center compliance. Always conduct the audit by actually visiting the store using Playwright browser tools — never guess from URLs alone.

## Pipeline

```
Store Discovery → Store Audit → Product Audit → GMC Policy Mapping → Scoring → HTML Report
```

After scoring, run an AI Semantic Audit on key pages for content quality risks that structural checks miss.

---

## Layer 1: Store Discovery

Navigate the store with Playwright. Do NOT assume page structure — discover it.

### Auto-Discovery

1. Navigate to the homepage, take a snapshot
2. Extract all navigation links (header nav, footer links)
3. Identify these page types:
   - Home, Products/Shop, Collections/Categories, Blog/Articles
   - About, Contact, FAQ, Track Order
   - Refund/Returns, Shipping, Privacy, Terms, Billing, Cancellation, Exchange, Warranty, Cookie, GDPR
4. Crawl footer for policy links
5. Navigate to Search page and Cart page

### Technical Discovery

Fetch and parse (use `WebFetch` tool):
- `{store}/robots.txt`
- `{store}/sitemap.xml`
- `{store}/sitemap_products.xml` (if exists)
- `{store}/sitemap_pages.xml` (if exists)

### Output: Store Structure Table

```
Page         Status    URL
About        ✓ found   /pages/about
Contact      ✓ found   /pages/contact
Privacy      ✓ found   /policies/privacy-policy
Refund       ✓ found   /policies/refund-policy
Shipping     ✗ missing  -
Terms        ✗ missing  -
```

Catalog: total products found, collections found, blog articles found.

---

## Layer 2: Store Audit

### 2.1 Identity Audit

Visit About and Contact pages, check footer. Verify consistency across:
- Brand name (domain, logo alt text, footer, About page, Contact page, schema)
- Logo presence and alt text
- Business address (real street address, not PO box)
- Email (brand domain email, not gmail/outlook/yahoo)
- Phone number
- Google Maps embedding
- Business hours
- ICP备案 (for .cn / Chinese stores)
- Organization schema (`evaluate_script: () => JSON.parse(document.querySelector('script[type="application/ld+json"]')?.textContent || '{}')`)

Flag mismatches as HIGH risk.

### 2.2 Contact Audit

Navigate to Contact page. Check:
- Contact Us page exists and is linked in footer
- Email uses brand domain (not @gmail.com, @outlook.com, @yahoo.com, @qq.com)
- Phone number present
- Physical address present
- Contact form functional (fill and submit if possible)
- Response time承诺 (if stated)

### 2.3 Policy Audit

Navigate to EACH policy page. For each:
- Status: exists (200) / missing (404) / empty / thin content
- Word count (use `evaluate_script` to get `document.body.innerText.length`)

Required policies per GMC:
- Privacy Policy
- Refund/Return Policy
- Shipping Policy
- Terms of Service
- Billing Terms (cancellation policy)

Bonus policies (lower risk):
- Cookie Policy
- GDPR/CCPA notice
- Warranty
- Exchange policy

### 2.4 Pricing Audit

Compare prices across pages:
1. Homepage — any displayed prices
2. Collection page — listed product prices
3. Product page — detail price
4. Cart — cart price after add-to-cart

Any inconsistency → HIGH risk. Document: page, product, price shown, vs other pages.

### 2.5 Trust Audit

Check:
- Trust badges (SSL seal, payment badges, money-back guarantee)
- Reviews/testimonials (real or placeholder?)
- Copyright year (current? stale = low trust)
- Social media links (real profiles, not empty)
- Brand Story / About page substance
- Google Maps embed
- Business registration info

Score each: present+real / present+sketchy / missing.

### 2.6 Technical Audit

Use browser tools:
- SSL: check HTTPS enforced, no mixed content warnings in console
- HTTP status: sample 10 internal links, flag any 404/500
- Canonical: check `<link rel="canonical">` on key pages
- Hreflang: check for multi-language stores
- Indexability: check `<meta name="robots">`, `X-Robots-Tag` header
- Structured data: check for Organization, WebSite, BreadcrumbList schema
- Console errors: `list_console_messages` to catch JS errors

### 2.7 Navigation Audit

Verify the buy flow end-to-end:
```
Home → Collection → Product → Add to Cart → Cart → Checkout
```
At each step, take a snapshot. Confirm:
- All links in path are functional
- No dead ends, no redirect loops
- Mobile nav works (resize to 375px and test)

---

## Layer 3: Product Audit

### Sampling

Extract up to 10 product URLs from sitemap or collection pages. If the store has fewer than 10 products, audit all of them.

### For Each Product

Navigate to the product page. Check:

**Structured Data**
Evaluate script for JSON-LD Product schema. Check required fields:
- `name`, `description`, `image`, `sku`, `brand`, `gtin`/`mpn`, `offers.price`, `offers.availability`, `offers.priceCurrency`

**Consistency**
Compare across page elements:
- Title (meta title vs H1 vs schema name)
- Price (schema vs displayed vs variant prices)
- Image (schema vs og:image vs visible product image)
- Description (meta description vs schema vs visible)

**Offer/Availability**
- Schema `availability` matches actual page (InStock vs SoldOut)
- Price currency matches store's target market

**Variant Check** (for products with variants)
- Each variant has distinct price displayed
- Variant images match selected variant

### Claims Audit (per product)

Scan product title and description for risky claims using AI evaluation:
- Superlatives: "#1", "best", "100%", "guaranteed", "perfect", "ultimate"
- Medical: "cure", "treatment", "FDA approved", "medical grade", "clinical"
- Urgency: "limited time", "act now", "while supplies last", "don't miss"
- Before/after: transformation claims, comparison images
- Absolute terms: "never", "always", "permanent", "instant"
- Pricing claims: "lowest price", "cheapest", "best deal"

Log every flagged phrase with its exact text and risk level.

### Image Audit

For each product's main image:
- Check if loads (no broken image)
- Check alt text presence
- Note if image contains heavy marketing text overlay (common GMC rejection)
- Note watermarks

### Buy Flow Audit

On one product, execute full flow:
1. Select variant (if applicable)
2. Click Add to Cart
3. Verify cart updated with correct product, variant, quantity, price
4. Proceed to checkout
5. Verify checkout loads, price matches

---

## Layer 4: GMC Policy Engine

Map EVERY finding to a Google GMC policy. Never report an issue without a policy reference.

### Policy Categories

| Policy | Key Checks |
|--------|-----------|
| **Misrepresentation** | Identity inconsistency, false business info, unrealistic claims, omitted policies |
| **Unavailable promotions** | Out of stock products, expired offers, misleading prices, bait-and-switch |
| **Unsafe claims** | Medical claims, unsubstantiated guarantees, misleading before/after |
| **Insufficient contact** | Missing contact page, no brand email, no physical address |
| **Unclear returns/refunds** | Missing or vague refund policy, no timeline, no conditions |
| **Unclear shipping** | Missing shipping info, no delivery timeline, no carrier info |
| **Incomplete product data** | Missing GTIN/MPN/brand, inaccurate availability, mismatched prices |
| **Image quality** | Promotional overlay on product image, watermarks, placeholders |
| **Abuse of network** | Malware, phishing, cloaking, malicious redirects |

For each finding, output:
```
Finding: Missing Refund Policy
Evidence: /policies/refund-policy returns 404
Policy: Misrepresentation (omitted policy)
Risk: High
```

Reference the detailed policy document: [GMC Policy Reference](references/gmc-policies.md)

---

## Layer 5: Risk Scoring

Score on 100-point scale across weighted categories:

| Category | Max | Weight |
|----------|-----|--------|
| Identity Authenticity | 20 | Brand/address/email/schema consistency |
| Policy Completeness | 25 | All required policies present, not empty |
| Trust Signals | 15 | Reviews, badges, social, about page |
| Product Data | 20 | Schema, GTIN, consistency, availability |
| Technical Health | 10 | SSL, status codes, canonical, indexability |
| Claims Risk | 10 | No misleading/medical/superlative claims |

**Scale:**
- 95+ — Excellent, GMC Ready
- 90-94 — Good, minor fixes recommended
- 80-89 — Fair, moderate risk
- 70-79 — Needs Fix, significant issues
- 60-69 — High Risk, likely disapproval
- Below 60 — Likely Suspension

### Deduction Rules

Reference the detailed scoring rubric: [Scoring Rubric](references/scoring-rubric.md)

---

## Layer 6: AI Semantic Audit

After structural audit, review content QUALITY on key pages. Navigate to each page, read the full text content, then evaluate:

### About Us
- Does it tell a real brand story, or is it generic filler?
- Is the brand origin/mission clear?
- Are there real people/team mentioned?

### Refund Policy
- Is it reasonable? (e.g., "all sales final, no refunds" → high risk)
- Clear timeline, conditions, process?
- Contact info for returns?

### Shipping Policy
- Processing time stated?
- Delivery timeframe realistic?
- Carrier info, tracking info, cost info?
- Any contradictions (e.g., "ships in 24h" + "processing 7-14 days")?

### Product Descriptions
- Original content or obvious dropshipping copy-paste?
- Any exaggerated claims?
- Do descriptions match product images?

### Homepage Copy
- Any risky marketing claims?
- Price anchoring that may trigger GMC scrutiny?
- Countdown timers, fake scarcity?

### Brand Consistency
- Does the brand voice match across pages?
- Logo, colors, domain, email domain, social media — unified or fragmented?
- Is this clearly one business or does it appear cobbled together?

Flag each semantic risk finding with: page, issue, risk level, recommendation.

---

## HTML Report Generation

Generate a single professional HTML dashboard. Use the template at `templates/report-template.html` as the base, filling in all audit data.

The report must include:
1. Header: store URL, audit date, overall score with color-coded badge
2. Executive Summary: total issues by severity (Critical/High/Medium/Low)
3. Score Breakdown: radar or bar chart of 6 category scores
4. Issue Cards: each finding with severity badge, evidence, policy reference, recommendation
5. Action Plan: phased (Phase 1 = today, Phase 2 = this week, Phase 3 = long-term)
6. AI Semantic Review: qualitative findings section
7. Footer: tool attribution, disclaimer

Save the report to the user's desktop or current working directory as `gmc-audit-{domain}-{date}.html`.

---

## Workflow Summary

```
1. Navigate to store homepage (Playwright)
2. Store Discovery — crawl nav, footer, sitemaps
3. Store Audit — 7 modules (identity, contact, policy, pricing, trust, technical, navigation)
4. Product Audit — sample 10 products, structured data, claims, buy flow
5. AI Semantic Audit — read key pages, evaluate content quality
6. GMC Policy Mapping — map every finding to Google policy
7. Risk Scoring — calculate 100-point score
8. Generate HTML Report — render template with findings
```

## Important Rules

- Use Playwright MCP tools (`browser_navigate`, `browser_snapshot`, `browser_evaluate`, `browser_network_requests`, `browser_console_messages`, `browser_take_screenshot`) for all website interactions
- Use `WebFetch` for robots.txt and sitemap files (faster than browser navigation)
- Do NOT skip any layer — even if a section seems fine, report that it passed
- Every finding MUST reference a Google GMC policy
- For stores with 100+ products, sample 10 randomly
- For product audit, capture screenshots of product pages for the report
- Output the final HTML report file — never just describe results in text

## References

- [GMC Policy Reference](references/gmc-policies.md) — Google Merchant Center policy details
- [Scoring Rubric](references/scoring-rubric.md) — Detailed score deductions
