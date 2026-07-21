# Trajekt SEO Checklist — Complete Site Audit
*Updated July 2025 — technical SEO, schema.org validation, LLM/AI optimisation, and rankings strategy*

---

## SECTION 1: Technical SEO — Per Page

### Meta Essentials
- [ ] `<title>` present, 50–60 characters, primary keyword included
- [ ] `<meta name="description">` present, 150–160 characters, unique per page
- [ ] `<link rel="canonical">` matches intended indexable URL exactly
- [ ] `<meta name="robots" content="index, follow">` present
- [ ] `lang="en-GB"` on `<html>` element

### Open Graph / Twitter
- [ ] `og:title`, `og:description`, `og:url`, `og:type` all present
- [ ] `og:image` present (1200x630px recommended)
- [ ] `twitter:card` = `summary_large_image`

### Heading Structure
- [ ] Exactly one `<h1>` per page — primary keyword in H1
- [ ] H2s used for major sections
- [ ] Heading hierarchy logical: H1 > H2 > H3, no skipping levels
- [ ] No duplicate H1s

### Internal Links
- [ ] Page links to at least 3 related internal pages
- [ ] Page is linked from at least one other page (no orphan pages)
- [ ] All internal links resolve to existing pages (no 404s)
- [ ] URL is lowercase, hyphen-separated, no underscores

---

## SECTION 2: Schema.org Structured Data Validation

**Validation tools:**
- Google Rich Results Test: https://search.google.com/test/rich-results
- Schema.org Validator: https://validator.schema.org

### FAQPage Schema (channel, platform, topic, comparison pages)
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Question text here?",
      "acceptedAnswer": { "@type": "Answer", "text": "Answer text here." }
    }
  ]
}
```
- [ ] `@type` = `FAQPage`
- [ ] `mainEntity` array contains `Question` objects (minimum 5)
- [ ] Each `Question` has `name` (question text) and `acceptedAnswer.text` (answer text)
- [ ] `acceptedAnswer.text` is plain text — NO HTML tags inside the string
- [ ] Question text ends with `?`
- [ ] Answer text is at least 50 characters
- [ ] No duplicate question text within the page
- [ ] Validated with no errors in Google Rich Results Test

### BreadcrumbList Schema (all pages except homepage)
```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "Home", "item": "https://trajekt.co.uk/" },
    { "@type": "ListItem", "position": 2, "name": "ChatGPT Product Feed", "item": "https://trajekt.co.uk/chatgpt-product-feed" }
  ]
}
```
- [ ] `@type` = `BreadcrumbList`
- [ ] `itemListElement` is an array of `ListItem` objects
- [ ] Each ListItem has `position` (integer from 1), `name`, and `item` (full URL)
- [ ] Positions are sequential: 1, 2, 3
- [ ] Final breadcrumb URL matches current page canonical URL
- [ ] Home position 1 URL = `https://trajekt.co.uk/`

### SoftwareApplication Schema (core non-blog pages)
```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Trajekt",
  "applicationCategory": "BusinessApplication",
  "operatingSystem": "Web",
  "url": "https://trajekt.co.uk",
  "offers": { "@type": "Offer", "price": "0", "priceCurrency": "GBP", "description": "Free plan available" }
}
```
- [ ] `@type` = `SoftwareApplication`
- [ ] `applicationCategory` = `BusinessApplication`
- [ ] `operatingSystem` = `Web`
- [ ] `offers` block present with `price` (string of numeric value, not "£49") and `priceCurrency`
- [ ] `url` matches canonical URL for that page

### Article Schema (blog posts only)
```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Article title here",
  "description": "Article description here",
  "datePublished": "2025-07-01",
  "dateModified": "2025-07-01",
  "author": { "@type": "Organization", "name": "Trajekt" },
  "publisher": { "@type": "Organization", "name": "Trajekt", "url": "https://trajekt.co.uk" },
  "mainEntityOfPage": { "@type": "WebPage", "@id": "https://trajekt.co.uk/blog/slug-here" }
}
```
- [ ] `headline` matches article H1
- [ ] `datePublished` in ISO 8601 format (`2025-07-01` not `1 July 2025`)
- [ ] `dateModified` present
- [ ] `author` and `publisher` both present
- [ ] `mainEntityOfPage` references canonical article URL
- [ ] Validated with no errors in Google Rich Results Test

### Organization + WebSite Schema (homepage only)
- [ ] `@type: Organization` with `name`, `url`, `description`
- [ ] `@type: WebSite` with `name`, `url`, `publisher` referencing Organization
- [ ] Validated without errors in schema.org validator

### Common Schema Errors — Quick Reference

| Error | Cause | Fix |
|-------|-------|-----|
| `acceptedAnswer.text contains HTML` | HTML tags in answer string | Strip all `<tags>` from answer text |
| `BreadcrumbList item missing` | `item` property absent | Add `"item": "https://trajekt.co.uk/[path]"` to each ListItem |
| `datePublished not ISO 8601` | Wrong format | Use `"2025-07-01"` not `"1 July 2025"` |
| `offers.price not a number` | Price formatted with currency | Use `"price": "49"` (string of digits only) |
| `FAQPage has no mainEntity` | Empty or misspelled | Check: `mainEntity` (not `mainEntities`) |
| `Question name not a string` | Wrong type | `"name"` must be a plain string, not an object |

---

## SECTION 3: LLM / AI Crawler Files

AI assistants like ChatGPT, Perplexity, Claude, and Gemini crawl websites when answering questions. These files ensure Trajekt is accurately understood and cited.

### llms.txt
- [ ] `/llms.txt` exists at site root and returns HTTP 200
- [ ] Contains: what Trajekt is, what it does, pricing (Free/£19/£49/£99/POA), channels supported, platforms supported, key features
- [ ] Written in plain Markdown with `##` headers for scannability
- [ ] Factually accurate — matches `/pricing` page and current feature set
- [ ] Accessible to all crawlers (no auth, no bot blocks)

### llms-full.txt
- [ ] `/llms-full.txt` exists and returns HTTP 200
- [ ] Contains everything in `llms.txt` plus: site structure (all page paths), blog article list, technical details

### robots.txt
- [ ] `/robots.txt` exists and returns HTTP 200
- [ ] `Allow: /` present
- [ ] Explicit Allow rules for: GPTBot, ChatGPT-User, Claude-Web, anthropic-ai, PerplexityBot
- [ ] `Sitemap: https://trajekt.co.uk/sitemap.xml` present
- [ ] No `Disallow` rules blocking AI crawlers from content pages

### .well-known/ai-plugin.json
- [ ] `/.well-known/ai-plugin.json` exists and returns HTTP 200
- [ ] `description_for_model` accurately describes Trajekt's purpose and pricing
- [ ] `contact_email` is correct (`hello@trajekt.co.uk`)

### sitemap.xml
- [ ] `/sitemap.xml` exists and returns HTTP 200
- [ ] All 70+ pages included with correct `<loc>` URLs
- [ ] `<lastmod>` dates are maintained when pages change
- [ ] `<priority>` values set appropriately (1.0 homepage, 0.9 core pages, 0.7 blog)
- [ ] Submitted to Google Search Console and Bing Webmaster Tools

---

## SECTION 4: Content Quality Audit

### Pricing Accuracy
- [ ] All pages reference correct pricing: Free / £19 / £49 / £99 / Agency POA
- [ ] No references to old pricing (£29/£79/£149/£299) on any page
- [ ] `/pricing` page is the master reference — all other pages agree with it

### SaaS Focus (not a tutorial)
Each page should sell Trajekt, not teach the user to do it themselves:
- [ ] Technical processes describe what Trajekt handles automatically (not how to do it manually)
- [ ] Every major section has a CTA (not just one at page bottom)
- [ ] Features framed as benefits: "Trajekt handles X so you don't have to" not "Here is how X works"
- [ ] No section inadvertently makes a competing tool look better

### Factual Accuracy — Stats in Use
Verify these specific stats are accurate before using:
- [ ] "ChatGPT processes 50 million shopping queries daily" — sourced from OpenAI 2025 statement
- [ ] "UK ecommerce market £286 billion in 2025" — sourced from IMRG/ONS data
- [ ] "60% of UK shoppers felt misled by incorrect product data" — source on file
- [ ] "Optimised titles and descriptions increase CTR by 30%" — sourced study referenced
- [ ] "14 million UK monthly active Pinterest users" — sourced from Pinterest UK 2024/25 business data
- [ ] "56 million UK monthly Bing users" — sourced from Microsoft Advertising UK data

### Technical Accuracy — Channel Specs
Check these against current platform documentation quarterly:
- [ ] ChatGPT JSONL field names match current OpenAI spec (last checked: _____)
- [ ] OpenAI SFTP delivery process still accurate (last checked: _____)
- [ ] Google Shopping XML namespace and required fields still accurate
- [ ] Meta Automotive Inventory Ads CSV spec still accurate
- [ ] TikTok Shop CSV format still accurate (currency column separate)
- [ ] Awin field names still accurate (aw_product_id, aw_deep_link, merchant_image_url)

---

## SECTION 5: Competitive Differentiation

### Core Positioning Claims (verify per page)
Every core page should substantiate at least one unique claim:
- [ ] ChatGPT Shopping feed support (competitors: no)
- [ ] AI Rules powered by Claude (competitors: basic/no AI)
- [ ] UK-built, UK-timezone support (competitors: US-based)
- [ ] Permanent free plan, no trial (competitors: trial only)
- [ ] Vehicle feed specialisation for UK dealers (competitors: generic)

### Comparison Pages
- [ ] `/vs/datafeedwatch` — mentions: no ChatGPT, US-first pricing, no AI enrichment
- [ ] `/vs/shoptimised` — mentions: Google-only, no ChatGPT, no multi-channel
- [ ] `/vs/channable` — mentions: enterprise complexity, pricing, EU-focus
- [ ] Competitor facts are verifiable and current (check competitor sites quarterly)
- [ ] No defamatory or unverifiable claims

---

## SECTION 6: Rankings Strategy & Next Steps

### Immediate Actions (pre-launch)
1. **Google Search Console** — verify site ownership, submit sitemap.xml
2. **Bing Webmaster Tools** — submit sitemap
3. **All og:image URLs** — ensure image files exist in production at the referenced paths
4. **Add FAQPage schema to homepage** — currently missing; significant rich result opportunity

### Month 1 Post-Launch
5. **Link building to `/chatgpt-product-feed`** — outreach to:
   - UK ecommerce blogs (eConsultancy, Internet Retailing)
   - Shopify community forums and App Store adjacent content
   - PPC communities (PPCTalk, r/PPC, r/ecommerce)
   - UK digital marketing LinkedIn groups
6. **Submit to SaaS directories** — ProductHunt, G2, Capterra, AlternativeTo, SaaSHub
7. **Register with OpenAI merchant directory** — ensures Trajekt can be surfaced in ChatGPT Shopping tool recommendations
8. **Submit llms.txt to AI directories** — LLMstxt.site, any emerging llms.txt registries

### Month 2–3
9. **Create `/ai-commerce` hub page** — groups ChatGPT, Perplexity, Google AI Mode, and AI Rules into one topical hub, building cluster authority
10. **Build a free tool** — `/tools/chatgpt-feed-validator` or `/tools/ean-checker` generates backlinks and repeat visits
11. **Publish monthly blog article** — prioritise ChatGPT-related queries (near-zero competition currently)
12. **Pursue press coverage** — The Drum, Marketing Week, Econsultancy for tool mentions
13. **Case study page** — real merchant data builds E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) signals critical for Google's ranking of SaaS sites

### Month 3+
14. **Changelog page** — signals active development to both users and Google
15. **Docs/help centre** — reduces support load and generates long-tail keyword rankings
16. **Review schema quarterly** — platforms change specs; update structured data when channel specs change
17. **Monitor PAA (People Also Ask)** — use Search Console data to identify questions to address in FAQ sections

---

## SECTION 7: SEO Monitoring Checklist (Monthly)

- [ ] Google Search Console: any new crawl errors?
- [ ] Check Core Web Vitals report in Search Console
- [ ] Review top 20 ranking pages: any significant position drops?
- [ ] Check: have any competitor pages jumped above Trajekt on priority queries?
- [ ] Update `<lastmod>` in sitemap.xml for changed pages
- [ ] Verify pricing on all pages still matches app pricing
- [ ] Check ChatGPT/OpenAI changelog for spec updates to the JSONL feed
- [ ] Publish one new blog article targeting a keyword gap
- [ ] Review Google's Rich Results Test for 5 core pages — any new errors?
- [ ] Check that llms.txt is still being crawled (look for GPTBot in server logs)

---

*Trajekt SEO Checklist — maintained alongside site build. Last updated: July 2025.*
