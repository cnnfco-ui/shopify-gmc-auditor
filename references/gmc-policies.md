# Google Merchant Center Policy Reference

## Misrepresentation

**Policy URL:** https://support.google.com/merchants/answer/6150127

Merchants must be transparent and honest about their business, products, and policies.

### What triggers Misrepresentation:

| Trigger | Example |
|---------|---------|
| Omitted or incomplete business information | No business address, no contact info |
| Inconsistent brand identity | Different names on logo/footer/domain/About page |
| False claims about business | "Official retailer" when not authorized |
| Missing required policies | No refund, shipping, or privacy policy |
| Empty or placeholder policy pages | Policy URL returns blank page or lorem ipsum |
| Unrealistic promises | "Lose 10kg in 3 days", "Get rich quick" |
| Bait-and-switch pricing | Different price on page vs checkout |
| Fake scarcity/urgency | "Only 2 left" when always showing |
| Fake reviews or testimonials | Stock photos as customer reviews |
| Hidden costs | Fees only revealed at checkout |

### Policy Pages Required:

- Refund / Return Policy
- Shipping / Delivery Policy
- Privacy Policy
- Terms of Service

### Brand Authorisation Claims (official stores / resellers)

Sites claiming "official store", "authorised distributor", or "authorised reseller" status must be able to prove it. Required proof:

- Authorisation letter/certificate naming the authorising brand owner AND the authorised operator (both full legal names)
- Validity period, territory, product scope
- Trademark registration number(s)
- Verifiable links (company register lookup, brand official site)

Critical rule: the authorising party MUST actually own the brand. If the letter is issued by a company that is NOT the registered trademark owner — and holds no chain of authorisation from the owner (A→B→C) — the claim is misrepresentation. Verify:

1. Trademark ownership in public registers: IP Australia, USPTO, EUIPO, WIPO, CNIPA
2. Authorisation chain: owner → intermediate → site operator
3. Brand's official "Where to Buy" / authorised-dealer list

Limitation: authenticity of signatures/seals on documents requires manual verification; flag where proof is present but not independently verifiable.

---

## Insufficient Contact Information

**Policy URL:** https://support.google.com/merchants/answer/6150127

### Requirements:

- Physical business address (not PO Box)
- Phone number or email (brand domain preferred)
- Contact Us page linked in footer
- Business registration number (varies by country)

### Red Flags:

- Only a contact form with no other info
- @gmail.com / @yahoo.com / @qq.com email
- No physical location for physical goods
- Contact page 404 or thin content

### Brand-domain Email判定标准

Compare email domains case-insensitively, ignoring the `www.` prefix. PASS: email `@` domain matches the site's primary domain (any prefix like support@/info@ is fine); official brand domains are also fine. VIOLATION: free-mail providers (gmail/outlook/yahoo/qq/163) or domains with no traceable link to the brand.

### Anti-hallucination rule (CRITICAL)

Emails MUST be extracted verbatim from actual page text. NEVER infer or fabricate an email from a company legal name — e.g., "Hypewave Technology Pty Ltd" appearing in a policy does NOT mean support@hypewave.com.au exists. An email-mismatch finding requires the exact quoted email strings as evidence; without them, no finding. If no email is on a page, say "no email found".

---

## Unavailable Promotions

**Policy URL:** https://support.google.com/merchants/answer/6150135

Products must be available for purchase and delivered as promised.

### Key Checks:

- Products marked InStock in schema must be purchasable
- Sale/promotion pricing must be genuine (not fake markdowns)
- Shipping times must be realistic
- No out-of-stock products in active feeds

---

## Dangerous or Unsafe Claims

**Policy URL:** https://support.google.com/merchants/answer/6150004

### Prohibited Claims:

- Medical/treatment claims without FDA/regulatory approval
- "Miracle cure", "instant results", "guaranteed weight loss"
- Unsubstantiated before/after comparisons
- Health claims on supplements without disclaimers

### High-Risk Keywords to Flag:

Medical: "cure", "treatment", "therapeutic", "heal", "diagnose", "prevent disease", "FDA approved", "medical grade", "clinically proven"

Absolute: "guaranteed", "100%", "permanent", "never fail", "instant", "miracle", "magic"

Superlative: "#1", "best in the world", "top rated", "number one", "leading", "unbeatable"

---

## Incomplete Product Data

**Policy URL:** https://support.google.com/merchants/answer/7052112

### Required Attributes:

| Field | Requirement |
|-------|-------------|
| GTIN | Required for products with barcodes |
| MPN | Required if no GTIN (manufacturer part number) |
| Brand | Required — must match actual manufacturer |
| Condition | New, used, refurbished |
| Availability | Must match actual stock |
| Price | Must match landing page exactly |
| Image | Clear product photo, no watermarks, no overlays |

---

## Image Requirements

**Policy URL:** https://support.google.com/merchants/answer/188487

### Prohibited:

- Watermarks, logos over product
- Promotional text or price overlays
- Borders or frames
- Placeholder or generic images
- Low resolution (< 250x250 for non-apparel, < 100x100 for apparel)
- Images that don't match the product

---

## Abuse of Network

**Policy URL:** https://support.google.com/merchants/answer/6150124

### Check:

- No malware/phishing indicators
- No cloaking (different content for Google vs users)
- No automatic redirects to different domains
- HTTPS enforced sitewide
- No mixed content (HTTP resources on HTTPS page)

### Purchase-Flow Redirect Detection (dynamic)

Google flags redirected/hacked stores with a "Site hacked / 被入侵网站" indicator; redirects to unknown third-party domains are treated as cloaking / AB redirect under Abuse of Network.

Dynamically simulate the full buy flow (Home → Product → Add to Cart → Cart → Checkout, without paying). At each step:

1. Record the current hostname (`location.hostname`)
2. List all outbound network requests and flag any non-primary hostname
3. Watch for delayed JS redirects, hidden iframes, and 302 chains that leave the primary domain

**Whitelisted (never flag):** Shopify Payments, Stripe, PayPal, Klarna, Afterpay, Adyen; cdn.shopify.com and standard CDNs; GA / Meta pixel analytics.

**Red flags (Critical):** checkout moved to a third-party domain, click lands on an unrelated domain, hidden iframe injection, delayed JS cloaking redirect, 302 chain ending off-domain.

---

## Editorial & Professional Requirements

**Policy URL:** https://support.google.com/merchants/answer/6150108

### Requirements:

- Professional website design (not obviously templated/empty)
- Correct spelling and grammar
- Working navigation
- No placeholder content ("coming soon", lorem ipsum)
- Complete product detail pages

---

## Return & Refund Policy Requirements

### Must Include:

- Return window (e.g., 30 days)
- Condition requirements (unused, original packaging)
- Return shipping responsibility
- Refund timeline and method
- Restocking fees (if any)
- Non-returnable items listed
- Return process steps

---

## Shipping Policy Requirements

### Must Include:

- Processing time before shipment
- Delivery timeframe by region
- Shipping carriers used
- Shipping costs or free shipping threshold
- Tracking availability
- International shipping info (if applicable)
