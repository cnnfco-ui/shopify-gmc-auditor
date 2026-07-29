# GMC Compliance Scoring Rubric

## Score Calculation: 100 Points Total

---

## 1. Identity Authenticity (20 points)

| Check | Points | Deduction |
|-------|--------|-----------|
| Logo present | 3 | -3 if missing |
| Logo alt text has brand name | 2 | -2 if missing or generic |
| Brand name consistent (domain ↔ footer ↔ About ↔ schema) | 5 | -5 for any mismatch |
| Business address present (not PO box) | 3 | -3 if missing/PO box |
| Brand-domain email (not free email) | 3 | -3 if gmail/yahoo/qq/etc |
| Phone number present | 2 | -2 if missing |
| Organization schema | 2 | -2 if missing |

---

## 2. Policy Completeness (25 points)

Each policy scored:

| Policy | Points | Deduction |
|--------|--------|-----------|
| Privacy Policy | 5 | -5 missing, -3 empty/thin, -2 thin |
| Refund/Return Policy | 5 | -5 missing, -3 empty, -2 thin |
| Shipping Policy | 5 | -5 missing, -3 empty, -2 thin |
| Terms of Service | 5 | -5 missing, -3 empty, -2 thin |
| Billing/Cancellation | 3 | -3 missing |
| Cookie Policy | 1 | -1 missing |
| GDPR/Data Rights | 1 | -1 missing (for EU-targeting stores) |

Content quality thresholds:
- "Missing" = page not found (404) or not linked anywhere
- "Empty" = page exists but < 50 words
- "Thin" = page has 50-200 words but lacks key details

---

## 3. Trust Signals (15 points)

| Check | Points | Deduction |
|-------|--------|-----------|
| Reviews/testimonials present | 3 | -3 missing, -2 if fake/stock |
| Trust badges (SSL, payment, money-back) | 2 | -2 missing |
| Social media links (real profiles) | 2 | -2 missing or broken |
| Copyright year current | 1 | -1 stale (>1 year old) |
| About page has substance (>200 words) | 3 | -3 missing, -2 thin |
| Professional design (not empty/unfinished) | 2 | -2 if looks incomplete |
| Google Maps / location embed | 2 | -2 missing |

---

## 4. Product Data Quality (20 points)

Average across sampled products (up to 10):

| Check | Points per product | Deduction |
|-------|-------------------|-----------|
| Product schema (JSON-LD) present | 3 | -3 missing |
| GTIN or MPN present | 3 | -3 missing |
| Brand field in schema | 2 | -2 missing |
| Availability matches actual | 3 | -3 mismatch |
| Price matches between schema↔page↔cart | 3 | -3 mismatch |
| Image loads + has alt text | 2 | -2 broken or no alt |
| Variant data consistent | 2 | -2 if variants have issues |
| No watermarks/promos on image | 2 | -2 if watermarked |

Deduct points per product, then scale to 20-point max:
```
Product_Score = 20 * (avg_points_per_product / 20)
```

---

## 5. Technical Health (10 points)

| Check | Points | Deduction |
|-------|--------|-----------|
| HTTPS enforced sitewide | 3 | -3 if any HTTP |
| No mixed content | 1 | -1 if mixed content detected |
| No 404 on critical pages | 2 | -1 per critical 404 (max -2) |
| Canonical tags present | 1 | -1 missing |
| Hreflang correct (if multi-lang) | 1 | -1 missing or broken |
| Sitemap accessible | 1 | -1 404 |
| No console JS errors | 1 | -1 if errors on key pages |

---

## 6. Claims Risk (10 points)

Start at 10, deduct per violation:

| Violation | Deduction |
|-----------|-----------|
| Medical claim (cure, treatment, FDA) | -3 each |
| Fake scarcity (countdown, "only X left") | -2 each |
| Unsubstantiated guarantee | -2 each |
| Absolute superlative ("best", "#1", "perfect") | -1 each |
| Before/after with risk of misleading | -2 each |
| Price anchoring deception | -2 each |

Minimum score: 0 (cannot go negative).

---

## Final Grade Scale

| Score | Grade | Meaning |
|-------|-------|---------|
| 95-100 | Excellent | GMC Ready — submit with confidence |
| 90-94 | Good | Minor fixes recommended before submission |
| 80-89 | Fair | Moderate risk — fix issues first |
| 70-79 | Needs Fix | Significant issues — likely disapproval |
| 60-69 | High Risk | Very likely GMC suspension |
| Below 60 | Critical | Almost certain suspension — do not submit |

---

## Issue Severity Classification

| Severity | Criteria |
|----------|----------|
| **Critical** | Missing required policy, identity mismatch, medical claims, bait-and-switch pricing |
| **High** | Thin policy, free email, broken buy flow, out-of-stock as InStock, watermark on image |
| **Medium** | Missing bonus policy, no social links, thin About page, no alt text, stale copyright |
| **Low** | Missing cookie policy, minor console error, missing hreflang, non-critical schema omissions |
