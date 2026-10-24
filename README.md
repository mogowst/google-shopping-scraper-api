# Google Shopping Scraper API: How ScraperAPI's Structured Endpoint Works, What Each Plan Really Costs, and When a Cheaper Alternative Makes Sense

Most people searching for a Google Shopping scraper API want the same three things: product prices and sellers in clean JSON, without getting blocked, at a price that doesn't explode once you multiply by monthly volume. ScraperAPI answers the first two with a dedicated structured endpoint (`/structured/google/shopping`) that returns product title, price, seller, ranking position, thumbnails, and delivery options — no parser to maintain. The third question, cost, is where the math gets less obvious, because Google requests burn **25 credits each** on ScraperAPI, not 1. This article walks through the endpoint, all seven paid plans, the real per-request math, and where ScraperAPI sits against cheaper competitors.

> **Quick pick:** if you're testing, start with the free trial — 5,000 credits, 7 days, no credit card. For ongoing price-monitoring projects under ~100,000 Shopping requests a month, the Business plan at $299/month (3,000,000 credits ≈ 120,000 Google requests) is the realistic entry point. Country-level targeting outside the US and EU starts at Business, so factor that in before choosing a cheaper tier. 👉 [Start the 7-day free trial with 5,000 credits](https://www.scraperapi.com/signup?fp_ref=coupons)

## What the ScraperAPI Google Shopping endpoint actually returns

The structured endpoint turns Google Shopping results into JSON with fields that matter for price comparison and competitor monitoring: product `position` (ranking), `docid` (Google's product ID), `title`, `price` and `extracted_price`, `source` (the merchant), `thumbnail`, and `delivery_options` — including actual delivery fees, like a sample response showing "Consegna a 9,00 €" for a €89.90 product.

The granularity goes further than most listicle descriptions suggest. In ScraperAPI's own example output for a New Balance 9060 listing, the response includes the store's tax estimate (+$11.20), shipping cost ($9.95 from newbalance.com, free from Foot Locker), a 45-day return window, store ratings (4.7/5 from 107 reviews vs 4.4/5 from 87), and accepted payment methods (PayPal, Google Pay, Afterpay, Klarna). If you're building a price tracker, that's the data you'd otherwise have to scrape merchant sites individually.

You can also fetch a single product page — the `google.com/shopping/product/...` URL format — and set `output_format` to `markdown` or `text`. The output comes back as LLM-ready text with product names, prices, ranking positions, URLs, and sources, with no parsing step. That's a practical shortcut if you're feeding Shopping data into an AI pipeline rather than a database.

Getting started takes three parameters in a plain GET request: your `api_key`, a `query` (ScraperAPI's example uses 'graphic card'), and a `country` code like `us`. 👉 [Try the Google Shopping endpoint with a free account](https://www.scraperapi.com/solutions/ecommerce-data-collection/google-shopping-scraper?fp_ref=coupons)

## Full plan comparison: all seven tiers plus Enterprise

Every paid plan includes the same core feature set: JS rendering, premium residential and mobile IPs, advanced bypassing, JSON auto-parsing, structured data APIs, the DataPipeline scheduler, full crawler access, automatic retries, unlimited bandwidth, and a 99.9% uptime guarantee. What changes between tiers is credits, concurrency, geotargeting scope, and analytics depth.

| Plan | Price (monthly / billed annually) | Credits | Concurrent threads | ≈ Google Shopping requests/mo | Purchase |
| --- | --- | --- | --- | --- | --- |
| Hobby | $49 / $44.10 | 100,000 | 20 | ~4,000 | [Start trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Startup | $149 / $134.10 | 1,000,000 | 50 | ~40,000 | [Start trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Business | $299 / $269.10 | 3,000,000 | 100 | ~120,000 | [Start trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Scaling | $475 / $427.50 | 5,000,000 | 200 | ~200,000 | [Start trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Professional | $975 / $877.50 | 10,500,000 | 300 | ~420,000 | [Start trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Advanced | $1,975 / $1,777.50 | 21,500,000 | 500 | ~860,000 | [Start trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Enterprise | Custom quote | 22,000,000+ | 500+ | ~880,000+ | [Contact sales](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

Annual billing takes 10% off across the board. There's also a permanent free plan: 1,000 credits with a maximum of 5 concurrent connections, separate from the 7-day trial.

One billing note that's easy to miss: the credit multiplier is per domain, and Google sits at the expensive end. A standard page costs 1 credit, Amazon 5, Google and Bing (including subdomains) 25, LinkedIn 30, and sites behind Cloudflare, Datadome, or PerimeterX add 10 credits per request when bypassed. The dashboard's Domain Cost Estimator shows the exact cost for any URL, and you can set a `max_cost` per scrape so a single runaway request can't drain your balance.

## The real cost of Google Shopping requests (the math most reviews skip)

Here's the calculation that changes plan selection. At 25 credits per Google request, the headline credit counts translate to:

- **Hobby ($49):** 100,000 ÷ 25 = **4,000 Shopping requests** — $12.25 per 1,000
- **Business ($299):** 3,000,000 ÷ 25 = **120,000 requests** — about $2.49 per 1,000
- **Advanced ($1,975):** 21,500,000 ÷ 25 = **860,000 requests** — about $2.30 per 1,000

Two things make this worse than it looks on paper. First, since September 11, 2025, Google has removed the `num=100` parameter, so every SERP API now paginates 10 results at a time. If your project needs deep pagination — say, tracking the top 100 listings per keyword — one "query" becomes 10 API calls, and your effective cost per keyword is 10× the per-request rate. Calculate at the depth you actually need, not at one call per keyword.

Second, the scale cliff is steep. Hobby's 4,000 monthly Shopping requests sound fine until you're monitoring 500 products daily (15,000 requests), at which point you're on Startup with 40,000 requests — and if growth continues, Business. If your volume genuinely stays in the low thousands per month, the cheapest tiers work; if it's anywhere near six figures, do the division before committing.

There's also a legal-shape caveat worth knowing: Google's own sanctioned channel for Shopping data is the Merchant API, and Google's terms prohibit automated access that violates the machine-readable instructions on its pages. Every third-party scraper API in this market operates in that gray zone; it's a risk you accept with any vendor, and it's why Google doesn't publish its anti-bot measures — vendors charge surcharges precisely because the protection is real.

## Hidden limits that only show up in the docs

Three constraints live in ScraperAPI's billing documentation rather than its marketing pages, and each one can invalidate a plan choice:

1. **Geotargeting outside the US and EU requires Business or higher.** Hobby and Startup are locked to US/EU locations. If you need to see Shopping results as they appear in Japan, Brazil, or Australia — common for cross-border e-commerce research — the $49 and $149 tiers won't do it.
2. **Pay-as-you-go overflow only exists from Scaling up.** On Scaling ($475), Professional, Advanced, and Enterprise, you can keep scraping past your credit limit at a fixed per-credit rate up to a monthly cap you set. On the four cheaper plans, credits running out means scraping pauses until renewal — unless you turn on auto-upgrade, which automatically bills you the next tier (available on Hobby through Professional, opt-in).
3. **Concurrency overages return a 429, not a queue.** If you exceed your thread limit, requests are turned away rather than held. There's no extra charge, but your job fails and needs a retry — relevant if you're running parallel crawls close to your plan's thread count.

None of these are dealbreakers, but the geotargeting one regularly surprises people who picked Hobby for a global monitoring project. 👉 [Check current plan prices and the annual discount](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## How ScraperAPI compares on price (verified competitor data)

Google Shopping scraping is rated "Hard" across the industry: JavaScript rendering is mandatory, and you're facing rate limits, CAPTCHAs, bot fingerprinting, and IP reputation checks. Most vendors price it at 10 credits per page. Here's what independent price tracking (checked October 2026) shows for 10,000 / 100,000 / 1,000,000 Shopping pages per month:

| Provider | 10K pages/mo | 100K pages/mo | 1M pages/mo | Credits per page |
| --- | --- | --- | --- | --- |
| ScraperAPI | $49 (Hobby) | $299 (Business) | custom (Advanced covers ~860K at $1,975) | 25 |
| ScrapingDog | $40 (Lite) | $90 (Standard) | $1,000 (Business Plus) | 10 |
| Scrape.do | $29 (Hobby) | $99 (Pro) | $699 (Advanced) | 10 |
| ScrapeOps | $19 (100K credits) | $99 (1.25M credits) | $699 (10M credits) | 10 |
| ScrapingBee (Google API light) | $49 (Freelance) | $99 (Startup) | $999 | 10 |
| Oxylabs Web Scraper API | $30 (Starter) | $100 (Starter) | $700 (Flexible) | $0.001/page |

Read honestly, this table costs ScraperAPI some deals: at 100K pages a month, ScrapingDog's Standard plan runs $90 against ScraperAPI's $299, and at the 1M tier, Scrape.do's Advanced plan undercuts everything. If raw Shopping volume is your only metric and you're comfortable with a Google-specialized tool, those vendors are legitimately cheaper.

Where ScraperAPI earns its premium: it's a general-purpose scraping platform on a 40M+ IP pool across 50+ countries, so the same account and credits cover Amazon (5 credits), regular websites (1 credit), and protected targets — useful when Shopping is one feed among several in your stack. Its structured Google endpoints (Search, News, Shopping, Jobs) are included in every plan including the free tier, with no per-endpoint surcharge beyond the domain multiplier. And it only bills successful requests (200 and 404 responses), so failed fetches don't eat credits. For teams that live entirely in Google Shopping and want minimum spend, a Shopping-specialized competitor wins on price; for teams with mixed scraping needs, one unified account tends to pencil out.

## FAQ

**How many credits does one Google Shopping request cost?** 25. All Google properties and their subdomains bill at 25 credits per request on ScraperAPI, so a 100,000-credit Hobby plan covers roughly 4,000 Shopping requests per month.

**Is there a free trial?** Yes — 5,000 credits for 7 days with no credit card required. Separately, there's a permanent free plan with 1,000 credits and 5 concurrent connections, and support can grant extra testing credits if you ask.

**Can I get a refund?** ScraperAPI offers a 7-day no-questions-asked refund policy on paid plans; contact support and they process it. Combined with the free trial, you can evaluate the endpoint thoroughly before money is at stake.

**What happens when I run out of credits?** By default, scraping pauses until your credits renew. Hobby through Professional can enable auto-upgrade to the next tier, and Scaling and above can use pay-as-you-go overflow at a fixed per-credit rate with a self-set monthly cap. Annual plans can also renew early for a fresh batch of credits.

**Can it scrape individual product pages, not just search results?** Yes. Pass a `google.com/shopping/product/...` URL with your API key, and optionally set `output_format` to `markdown` or `text` for LLM-ready output including names, prices, and seller details.

**Does the free plan include the structured Shopping endpoint?** Yes. The structured data APIs are available on all plans, including the 1,000-credit free tier — the only difference is how few Google requests 1,000 credits buys (about 40).

## Which setup fits your project

Match the plan to your request volume, not your ambition:

- **Testing or a small prototype (under ~4,000 Shopping requests/month):** the free trial and then Hobby at $49 is enough; don't pay for credits you'll watch expire.
- **Ongoing price monitoring in the US/EU at moderate volume:** Startup ($149) covers ~40,000 requests; Business ($299) covers ~120,000 and unlocks non-US/EU country targeting — pick it if geography matters at all.
- **Production pipelines where a mid-month hard stop would hurt:** Scaling ($475) is the first tier with pay-as-you-go overflow, which is the real reason to jump past Business.
- **Pure high-volume Google Shopping on a tight budget:** the specialized competitors in the table above are genuinely cheaper per page — ScraperAPI isn't the right tool for that single use case, and pretending otherwise wouldn't help you.

Before you buy, run your actual keyword list through the dashboard's Domain Cost Estimator and set a `max_cost` on your scrapes — 25 credits per request compounds fast, and knowing your true monthly count is the difference between a $49 plan and a surprise upgrade. When the numbers check out, 👉 [open a free ScraperAPI account and test the Google Shopping endpoint](https://www.scraperapi.com/signup?fp_ref=coupons) with your own queries before the trial clock starts.
