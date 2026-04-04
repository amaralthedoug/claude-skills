---
name: web-tool-selector
description: >
  Decision framework for choosing between Firecrawl MCP, Firecrawl CLI, Playwright CLI,
  Playwright MCP, and Chrome DevTools MCP for web scraping, browser automation, debugging,
  testing, and page quality audits in Claude Code and AI agent workflows.
  ALWAYS use this skill when the task involves: visiting a URL, scraping a website, extracting
  web content, running browser tests, automating a web flow, logging into a site, debugging
  a network request, inspecting a DOM element, auditing page performance, checking SEO,
  verifying tracking pixels, testing conversion flows, or running Lighthouse.
  Also trigger when the user asks "which tool should I use for X" where X involves the web,
  or any concern about page speed, Core Web Vitals, Google Ads Quality Score, organic rankings,
  or accessibility scores. Use this skill before picking any tool.
---

# Web Tool Selector for Claude Code Agents

A decision framework for selecting the right web tool AND running quality audits that protect
organic traffic, paid traffic, and page scores. Wrong tool choices waste tokens and break
sessions. Wrong audit coverage leaves real problems undetected.

---

## TL;DR Decision Tree

```
Do you need to AUDIT a page for performance, SEO, paid traffic, or accessibility?
  → Go to AUDIT MODE section below — it has specific workflows per concern type.

Are you debugging an EXISTING open Chrome session or active DevTools panel?
  (failing network request you spotted, selected element, live authenticated app)
  YES → Chrome DevTools MCP (reuse session, no re-login, investigate network/elements/perf)

Is the site public and do you only need to READ content (no login, no clicks)?
  Need shell scripting, CI/CD, or piping output to other tools?
    YES → Firecrawl CLI (shell-native, batch ops, pipe to jq/grep/pandoc)
    NO  → Firecrawl MCP (fastest, cheapest, inline LLM-ready output)

Does the task require browser interaction (login, click, scroll, fill form)?
  In Claude Code / have filesystem access?
    YES, session long (30+ steps) or need .spec.ts → Playwright CLI
    YES, but short/exploratory (<15 steps)          → Playwright MCP
    NO (sandboxed env like Claude Desktop)          → Playwright MCP
```

---

## Tool Profiles

### 1. Chrome DevTools MCP (`chrome-devtools-mcp`)
**Best for:** Debugging live sessions, performance audits, and Lighthouse scoring.

| Strength | Detail |
|---|---|
| Reuse existing session | Connects to YOUR running Chrome — already logged in |
| Lighthouse audits | Performance, Accessibility, SEO, Best Practices scores (0–100) |
| Performance traces | LCP subpart breakdown: TTFB, load delay, load duration, render delay |
| Network analysis | List requests, filter by type, inspect failing or slow requests |
| Console errors | Catch JS errors, warnings, failed resource loads |
| Emulation | Simulate slow networks (Fast 3G) and slow CPUs (4x throttle) |
| Manual → AI handoff | You spot a bug in DevTools, Claude takes over investigation |

**Audit capabilities:**
```
lighthouse_audit          → Full Lighthouse report (all 4 categories)
performance_start_trace   → Performance trace with reload
performance_analyze_insight → LCP breakdown, render blocking, TTFB issues
list_network_requests     → Check tracking pixels, redirect chains, slow resources
evaluate_script           → Custom JS to inspect LCP element, check meta tags, etc.
emulate                   → Test under constrained network/CPU conditions
take_snapshot             → Accessibility tree for a11y auditing
```

**Configuration (already installed):**
```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["chrome-devtools-mcp@latest", "--autoConnect", "--channel=beta"]
    }
  }
}
```
> Requires Chrome M144+. Enable at `chrome://inspect/#remote-debugging` first.

---

### 2. Firecrawl MCP
**Best for:** Public web content extraction → clean Markdown/JSON directly in Claude's context.

| Strength | Detail |
|---|---|
| Speed | Fastest — no browser launch overhead |
| Output | Clean Markdown, ready for LLM consumption |
| Anti-bot | Handles proxies, CAPTCHAs, dynamic JS |
| Audit use | Extract meta tags, OG tags, canonical, structured data from any public page |

**Use when:** Scraping docs, blogs, checking on-page SEO tags quickly, in-context research.
**Do NOT use when:** Site requires login, needs interaction, or you need shell pipelines.

---

### 3. Firecrawl CLI (`firecrawl` command)
**Best for:** Shell-native batch operations — bulk audits, CI/CD, crawling, piping.

| Strength | Detail |
|---|---|
| Shell-native | Pipes to `jq`, `grep`, `pandoc`, etc. |
| Bulk crawl | Entire sites with depth/path filters and progress tracking |
| Bulk download | `download` maps + scrapes site into local files |
| Autonomous agent | `firecrawl agent "prompt"` — AI extracts autonomously |
| Audit use | Map all site URLs, find broken pages, batch-check meta tags across pages |

**Key audit commands:**
```bash
firecrawl map https://example.com --json          # Discover all URLs
firecrawl crawl https://example.com --wait        # Crawl all pages
firecrawl scrape https://example.com --format links  # Extract all links
firecrawl agent "find pages missing meta descriptions" --wait
```

**Install:**
```bash
npm install -g firecrawl-cli
# or: npx -y firecrawl-cli@latest init --all --browser
```

---

### 4. Playwright CLI (`@playwright/cli`)
**Best for:** Long interactive automation in Claude Code — test flows, CI/CD, .spec.ts generation.

Token cost: **~27,000 tokens per task**

| Strength | Detail |
|---|---|
| Token efficiency | ~4x cheaper than Playwright MCP |
| Long sessions | Context stays flat at step 50 same as step 5 |
| Audit use | Simulate user journeys, verify UTM params, test conversion flows, check redirect chains |

**Key commands:**
```bash
playwright-cli snapshot          # Page structure as YAML
playwright-cli click e21         # Click by element uid
playwright-cli fill e35 "text"   # Fill input
playwright-cli screenshot        # Save screenshot
playwright-cli state-save auth.json  # Persist login
playwright-cli state-load auth.json  # Restore login
```

---

### 5. Playwright MCP (`@playwright/mcp`)
**Best for:** Short exploratory sessions, sandboxed environments, multi-agent coordination.

Token cost: **~114,000 tokens per task**

| Strength | Detail |
|---|---|
| No filesystem needed | Works in Claude Desktop |
| Rich introspection | Full accessibility tree inline |
| Audit use | Quick element mapping, CTA visibility checks, first impression of a landing page |

**Do NOT use for:** Sessions > 15 steps (context bloat), Claude Code on large codebases.

---

## Audit Mode

Use this section when the goal is to evaluate page quality — not just automate or scrape.
Choose the audit type that matches your concern, or run the Quick Health Scan for an overview.

---

### Score Thresholds (Reference)

**Core Web Vitals (Google ranking factors):**
| Metric | Good | Needs Improvement | Poor |
|---|---|---|---|
| LCP (load) | < 2.5s | 2.5–4.0s | > 4.0s |
| INP (interactivity) | < 200ms | 200–500ms | > 500ms |
| CLS (visual stability) | < 0.1 | 0.1–0.25 | > 0.25 |

**Lighthouse Scores:**
| Range | Rating |
|---|---|
| 90–100 | Good |
| 50–89 | Needs improvement |
| 0–49 | Poor |

**Google Ads Quality Score factors (landing page experience):**
- Page load speed on mobile
- Relevance of content to the ad
- Mobile UX (no intrusive interstitials, responsive layout)
- Bounce rate signals (inferred from engagement)

---

### Audit Type 1: Performance & Core Web Vitals

**Goal:** Detect what's slowing down the page and killing your ranking/ad scores.

**Tools:** Chrome DevTools MCP (primary), Playwright CLI (for multi-viewport)

**Workflow:**
```
Step 1 → Chrome DevTools MCP: lighthouse_audit
         Categories: performance, best-practices
         → Note the Performance score and which CWV are failing

Step 2 → Chrome DevTools MCP: performance_start_trace (reload: true, autoStop: true)
         → performance_analyze_insight: LCPBreakdown
         → Identify which subpart is the bottleneck:
           - TTFB high?        → Server/CDN issue
           - Resource delay?   → Image loaded via JS, lazy-load on LCP, missing preload
           - Resource slow?    → File too large, no CDN, no WebP/AVIF
           - Render delay?     → Render-blocking scripts/CSS, no SSR

Step 3 → Chrome DevTools MCP: performance_analyze_insight: RenderBlocking
         → List resources blocking paint

Step 4 → Chrome DevTools MCP: emulate (network: "Fast 3G", cpuThrottlingRate: 4)
         → Re-run trace to simulate mobile users (most paid traffic is mobile)

Step 5 → Chrome DevTools MCP: list_network_requests (resourceTypes: ["Image", "Font"])
         → Check file sizes, cache headers, CDN usage
```

**Key findings to report:**
- LCP element and its subpart breakdown
- Render-blocking resources
- Images without `fetchpriority="high"`, lazy-loaded LCP image
- Missing `Cache-Control` headers
- No CDN detected

---

### Audit Type 2: SEO / Organic Traffic Health

**Goal:** Identify technical SEO issues that hurt crawlability, indexability, and rankings.

**Tools:** Firecrawl MCP (on-page tags), Firecrawl CLI (bulk crawl), Chrome DevTools MCP (Lighthouse SEO)

**Workflow:**
```
Step 1 → Chrome DevTools MCP: lighthouse_audit (categories: seo, accessibility)
         → Get baseline SEO score and a11y score

Step 2 → Firecrawl MCP: scrape the page
         → Extract and verify:
           - <title> tag (50–60 chars, contains target keyword)
           - <meta name="description"> (150–160 chars)
           - <link rel="canonical"> (correct self-referencing URL)
           - OG tags (og:title, og:description, og:image) — critical for social sharing
           - Twitter card tags
           - <meta name="robots"> (check for accidental noindex)
           - H1 tag (one per page, contains keyword)
           - Structured data (JSON-LD / Schema.org)

Step 3 → Firecrawl CLI: map the site
         firecrawl map https://example.com --json
         → Find pages, check for orphaned URLs

Step 4 → Firecrawl CLI: crawl key sections
         firecrawl crawl https://example.com --include-paths /blog,/products --wait
         → Identify pages returning 404, 500, redirect chains

Step 5 → Playwright CLI: test internal links
         → Navigate key landing pages, verify internal links resolve correctly
         → Check mobile layout (Google indexes mobile-first)
```

**Key findings to report:**
- Pages with missing or duplicate title/description
- Accidental `noindex` directives
- Broken internal links or redirect chains
- Missing canonical tags
- Missing OG/Twitter card tags (hurts paid social traffic too)
- No structured data on key pages (products, articles, FAQs)
- Pages not included in sitemap

---

### Audit Type 3: Paid Traffic Health

**Goal:** Verify the page won't waste ad budget — check tracking, Quality Score signals, and conversion flow.

**Tools:** Chrome DevTools MCP (primary), Playwright CLI (conversion flow)

**Workflow:**
```
Step 1 → Chrome DevTools MCP: lighthouse_audit (categories: performance, seo)
         → Google Ads Quality Score is directly impacted by:
           - Performance score (load speed)
           - SEO score (content relevance signals)
         → Meta Ads penalizes pages with Performance < 50 on mobile

Step 2 → Chrome DevTools MCP: emulate (network: "Fast 3G", cpuThrottlingRate: 4)
         + performance_start_trace
         → Simulate the mobile experience (majority of paid traffic)
         → If LCP > 2.5s on mobile, expect higher CPCs and lower ROAS

Step 3 → Chrome DevTools MCP: list_network_requests
         → Filter for tracking pixel calls:
           - Google Tag Manager: requests to googletagmanager.com
           - GA4: requests to google-analytics.com or analytics.google.com
           - Meta Pixel: requests to facebook.com/tr
           - Any conversion API calls
         → Verify pixels FIRE on page load (not just on conversion)
         → Check for double-firing (same event sent twice)

Step 4 → Chrome DevTools MCP: list_console_messages
         → Look for JS errors that could block tracking scripts
         → GTM errors, blocked scripts, Content Security Policy violations

Step 5 → Playwright CLI: simulate the full conversion flow
         - Start from the landing URL WITH UTM params
           (e.g., ?utm_source=google&utm_medium=cpc&utm_campaign=test)
         - Navigate through the funnel (CTA click → form → thank you page)
         - Verify UTM params are preserved throughout
         - Verify redirect chains don't strip UTM params
         - Confirm conversion event fires on thank-you page

Step 6 → Chrome DevTools MCP: take_snapshot
         → Check CTA button visibility above the fold
         → Verify no intrusive popups or interstitials that trigger Google penalties
         → Check form fields render correctly on mobile viewport
```

**Key findings to report:**
- Pixels not firing or firing incorrectly
- JS errors blocking tracking
- UTM params lost through redirects
- LCP > 2.5s on mobile (direct Quality Score impact)
- CTA not visible above the fold on mobile
- Redirect chain > 1 hop (each hop strips context and adds latency)
- Missing conversion event on thank-you page
- Intrusive interstitials (Google penalty for mobile)

---

### Audit Type 4: Accessibility (WCAG / a11y)

**Goal:** Identify accessibility failures that hurt SEO rankings, user trust, and legal compliance.

**Tools:** Chrome DevTools MCP (primary), Playwright CLI (keyboard nav testing)

**Workflow:**
```
Step 1 → Chrome DevTools MCP: lighthouse_audit (categories: accessibility)
         → Score < 90 = real problems. Note which audits failed.

Step 2 → Chrome DevTools MCP: take_snapshot
         → Inspect accessibility tree:
           - Images have alt text?
           - Buttons have accessible labels?
           - Form inputs have associated labels?
           - Heading hierarchy correct (H1 → H2 → H3)?

Step 3 → Playwright CLI: keyboard navigation test
         → Tab through the page — is focus order logical?
         → Are focus indicators visible?
         → Can all interactive elements be reached without a mouse?

Step 4 → Chrome DevTools MCP: evaluate_script
         → Check color contrast ratios for key text elements
         → Verify no `user-scalable=no` in viewport meta (blocks pinch zoom)
```

**Key findings to report:**
- Images without alt text (hurts SEO + a11y)
- Buttons/links without accessible labels
- Color contrast failures (text must meet 4.5:1 ratio for normal text)
- Missing form labels
- `user-scalable=no` blocking zoom (mobile accessibility failure)
- Focus traps or broken keyboard navigation

---

### Quick Health Scan (All-in-One)

When you need a fast overview without going deep — run this first, then choose a specific audit.

```
1. Chrome DevTools MCP: lighthouse_audit (all categories: performance, accessibility, seo, best-practices)
   → 4 scores in one shot. Identify which category needs the most attention.

2. Firecrawl MCP: scrape the page
   → Quickly verify: title, description, canonical, OG tags, H1, noindex status

3. Chrome DevTools MCP: list_console_messages
   → Any JS errors? Any blocked resources? Any CSP violations?

4. Chrome DevTools MCP: list_network_requests (filter: status 4xx or 5xx)
   → Broken resources that users and bots see
```

**Output format for health scan:**
- Lighthouse scores: Performance X | Accessibility X | SEO X | Best Practices X
- On-page tags: ✅/❌ for title, description, canonical, OG, noindex, H1
- Console errors: count and severity
- Broken requests: count and affected resources

---

## Hybrid Workflows

### Performance + Paid Traffic (most impactful combo)

```
Phase 1: BASELINE      → Chrome DevTools MCP: lighthouse_audit (all categories)
Phase 2: MOBILE SIM    → Chrome DevTools MCP: emulate Fast 3G + trace
Phase 3: TRACKING      → Chrome DevTools MCP: list_network_requests (pixels)
Phase 4: FLOW TEST     → Playwright CLI: simulate full funnel with UTM params
Phase 5: REPORT        → Summarize scores, blocking issues, tracking gaps
```

### SEO + Performance (organic traffic protection)

```
Baseline scores  → Chrome DevTools MCP: lighthouse_audit
On-page SEO      → Firecrawl MCP: extract tags and structured data
Site-wide crawl  → Firecrawl CLI: map + crawl → broken links, missing pages
CWV deep-dive    → Chrome DevTools MCP: performance_start_trace + LCPBreakdown
```

### Debug + Fix

```
Spot bug         → Chrome DevTools MCP: console + network analysis
Reproduce        → Playwright CLI: automate the failing scenario
Find fix         → Firecrawl MCP: scrape relevant public documentation
```

### QA / Testing

```
Phase 0: INVESTIGATION → Chrome DevTools MCP  (known bugs or audit)
Phase 1: EXPLORATION   → Playwright MCP        (map the app, < 10 steps)
Phase 2: EXECUTION     → Playwright CLI        (full flows, 30–50+ steps)
Phase 3: REGRESSION    → npx playwright test   (CI/CD, no AI)
```

---

## Quick Comparison Table

| Criteria | Chrome DevTools MCP | Firecrawl MCP | Firecrawl CLI | Playwright CLI | Playwright MCP |
|---|---|---|---|---|---|
| Token cost | Low (targeted) | Minimal (API credits) | None (shell) | ~27K/task | ~114K/task |
| Lighthouse audit | **Yes** | No | No | No | No |
| Core Web Vitals trace | **Yes** | No | No | No | No |
| Check tracking pixels | **Yes** | No | No | Partial | No |
| On-page SEO tags | Via JS | **Best** | Good | No | No |
| Bulk site crawl | No | No | **Best** | No | No |
| Conversion flow test | No | No | No | **Best** | Limited |
| UTM param validation | No | No | No | **Best** | No |
| Debugging live session | **Best** | No | No | No | No |
| Long automation (30+) | No | No | No | **Best** | Context bloat |
| Exploratory testing | No | No | No | Limited | **Best** |
| Shell pipelines / CI/CD | No | No | **Best** | Good | No |

---

## Forcing the Right Tool in Claude Code

If Claude picks the wrong tool, be explicit:

```
"Use Chrome DevTools MCP to run a Lighthouse audit on this page"
"Use Chrome DevTools MCP to check if the Meta Pixel fires correctly"
"Use Firecrawl MCP to extract the meta tags and OG tags from this URL"
"Use Firecrawl CLI to crawl the site and find all broken links"
"Use Playwright CLI to simulate the full checkout flow with UTM params"
"Use Playwright MCP to explore the landing page and map its interactive elements"
```

---

## Sources
- Core Web Vitals thresholds: https://web.dev/articles/vitals
- Chrome DevTools MCP: https://developer.chrome.com/blog/chrome-devtools-mcp
- LCP optimization guide: https://web.dev/articles/optimize-lcp
- Google Ads landing page experience: https://support.google.com/google-ads/answer/2404197
- Lighthouse scoring: https://developer.chrome.com/docs/lighthouse/performance/performance-scoring
- Microsoft Playwright MCP: https://github.com/microsoft/playwright-mcp
- Token benchmark (27K vs 114K): https://www.morphllm.com/playwright-mcp
- Firecrawl CLI docs: https://github.com/mendableai/firecrawl/tree/main/apps/js-sdk/firecrawl-cli
