---
name: geo-content-reviewer
description: >
  GEO (Generative Engine Optimization) Content Reviewer for analyzing and scoring web pages, blog posts,
  product listings, comparison pages, and review/lead-gen content for AI citation readiness across
  ChatGPT, Claude, Perplexity, and Google AI Overviews. Use this skill whenever someone asks to:
  review a page for GEO, check if content will be cited by AI, audit a URL or text for AI visibility,
  score content for generative engine readiness, optimize a page so ChatGPT or Claude recommends it,
  improve content for AI citations, check if a post is GEO-optimized, or analyze why a page isn't
  appearing in AI answers. Also trigger for requests like "will AI cite this?", "is this content
  AI-ready?", "how do I get Claude to recommend our brand?", or any review of comparison/affiliate/
  lead-gen pages for AI discoverability. Always use this skill for GEO-related content analysis tasks.
---

# GEO Content Reviewer

A skill for auditing and scoring web content for Generative Engine Optimization (GEO) — the practice
of making content easy for AI systems like ChatGPT, Claude, Perplexity, and Google AI Overviews to
find, understand, cite, and recommend.

> **Academic foundation**: Based on the Princeton/ACM SIGKDD 2024 paper *"GEO: Generative Engine
> Optimization"* (Aggarwal et al.), which demonstrated that structured GEO methods can increase AI
> visibility by up to 40%, with citation/statistics addition being the top-performing strategies.

---

## What GEO Is (vs. SEO)

| Dimension | Traditional SEO | GEO |
|-----------|----------------|-----|
| Goal | Rank in search results | Be cited in AI answers |
| Optimization target | Google bots | LLM context windows |
| Key signal | Backlinks + keywords | Authority + structure + completeness |
| Success metric | Click-through rate | Citation frequency + mention share |
| Content strategy | Drive clicks | Answer completely (zero-click is OK) |
| Currency | "Link juice" | "Trust embedding" |

---

## How to Use This Skill

When given a URL or text to review, follow this workflow:

### Step 1 — Identify Content Type
Classify the content before scoring:
- **Comparison/review page** (e.g., "Best hearing aids for seniors")
- **Brand/product page** (e.g., Jabra Enhance brand page)
- **Lead generation page** (form + bridge page)
- **Blog post / informational**
- **FAQ / knowledge base**

Content type determines which scoring weights to emphasize (see Section 4).

### Step 2 — Run the GEO Audit (10 Dimensions)
Score each dimension from 0–10. Then compute weighted score per content type.

### Step 3 — Identify Top 3 Quick Wins
Always end with 3 concrete, actionable improvements ranked by impact-to-effort ratio.

### Step 4 — Provide Platform-Specific Notes
Flag any issues specific to ChatGPT, Perplexity, Claude, or Google AI Overviews behavior.

---

## The 10 GEO Audit Dimensions

### 1. Answer-First Structure (0–10)
Does the page lead with a direct, complete answer to the most likely user question?
- **10**: First paragraph answers the primary query in ≤75 words, self-contained
- **5**: Answer exists but buried or requires context to understand
- **0**: Page never directly answers the implied user question

*Why it matters*: LLMs pull individual passages, not entire pages. The first extractable answer wins.

### 2. FAQ & Natural Language Coverage (0–10)
Does the page contain explicitly written Q&A content in conversational language?
- **10**: 5+ FAQs using full question phrasing ("How does X work?"), each answered completely
- **5**: Some FAQ-like content but not explicitly structured
- **0**: No Q&A content; only marketing copy

*Why it matters*: AI engines match against user query phrasing. FAQs pre-answer what users ask.

### 3. Factual Density & Statistics (0–10)
Does the page include verifiable data points, numbers, and statistics?
- **10**: Multiple specific stats with cited sources (e.g., "reduces cost by 43%, per J.D. Power 2024")
- **5**: Some numbers but unsourced or vague ("many users prefer...")
- **0**: Purely qualitative claims with no data

*Why it matters*: Princeton GEO study found statistics addition increased AI visibility by up to 40%.
Adding quantitative data is the single highest-impact GEO tactic.

### 4. Comparison & Competitive Content (0–10)
Does the page address "X vs Y" or "best X for [use case]" queries?
- **10**: Side-by-side comparison table or clear differentiation vs. alternatives
- **5**: Mentions alternatives but doesn't compare directly
- **0**: Treats subject in isolation; no comparative context

*Why it matters*: "Best X" and comparison queries are the highest-intent AI searches. These trigger
AI responses >70% of the time (Yotpo 2025). Comparison/lead-gen sites *must* own this dimension.

### 5. E-E-A-T Signals (0–10)
Does the page demonstrate Experience, Expertise, Authoritativeness, Trustworthiness?
- **10**: Named author with credentials, publication date visible, external citations to .gov/.edu/.org
- **5**: Brand-level authority signals but no individual author or sourced citations
- **0**: Anonymous, undated, no external references

*Why it matters*: E-E-A-T remains critical for GEO. Content with transparent authorship and reputable
citations consistently outranks shallow material in AI responses (Profound, 2025).

### 6. Schema Markup & Structured Data (0–10)
Does the page implement machine-readable schema that AI systems can parse?
- **10**: FAQPage + Review/AggregateRating + Product/Service schemas present and valid
- **5**: Basic schema (e.g., Organization) but missing FAQ or Review schema
- **0**: No structured data

*Why it matters*: Schema acts as an "API contract" for AI crawlers — formatted information that fits
within LLM context windows (Strapi, 2025). Missing schema = invisible to AI systems.

### 7. Content Freshness & Update Signals (0–10)
Is the content clearly recent and regularly maintained?
- **10**: Visible "Last updated" date within 90 days; changelog or versioning visible
- **5**: Publication date visible but >6 months old with no update signal
- **0**: No dates; content appears static or stale

*Why it matters*: LLMs favor the most recent version of content matching a query. Perplexity
especially weights recency (Firebrand, 2026). Stale content loses citation authority even if accurate.

### 8. Third-Party Mentions & External Validation (0–10)
Is the brand/content referenced or validated by external, independent sources?
- **10**: Cited in industry publications, Reddit discussions, review platforms (G2/Trustpilot), forums
- **5**: Some external mentions but primarily self-referential
- **0**: Only self-published; no third-party footprint

*Why it matters*: LLMs heavily weight third-party sources and earned mentions. Reddit, LinkedIn, and
YouTube were among the top-cited sources by LLMs in 2025 (Search Engine Land). Earned mentions are
references AI uses to assess brand credibility.

### 9. AI Crawlability & Technical Accessibility (0–10)
Is the content technically accessible to AI bots?
- **10**: Fast load (<3s), mobile-optimized, no crawl blocks in robots.txt for AI agents,
  clean URL structure, canonical tags set
- **5**: Accessible but with issues (slow load, some JS-gated content)
- **0**: Blocked by robots.txt, paywalled, or JS-only rendering

*Why it matters*: AI crawlers (ClaudeBot, GPTBot, PerplexityBot) must be able to access and index
pages. A technically excellent page that blocks bots gets zero citations.

### 10. Entity Clarity & Topical Focus (0–10)
Does the page establish one clear primary entity with supporting context?
- **10**: One primary entity per page (brand, product, or topic) with 3–6 supporting entities
  connected to Wikipedia/Wikidata or industry standards via internal/external links
- **5**: Multiple topics on one page without clear hierarchy
- **0**: Scattered content; no clear entity ownership

*Why it matters*: After ChatGPT's October 2025 entity update, AI models now explicitly recognize and
recommend brands with clear entity definitions. Pages with one clear topic and entity linkage are
significantly more likely to be cited (Directive Consulting, 2025).

---

## Scoring Weights by Content Type

| Dimension | Comparison/Lead Gen | Brand Page | Blog/Informational | FAQ Page |
|-----------|--------------------|-----------|--------------------|----------|
| 1. Answer-First | 10% | 10% | 15% | 20% |
| 2. FAQ Coverage | 15% | 10% | 15% | 25% |
| 3. Factual Density | 15% | 10% | 15% | 10% |
| 4. Comparison Content | **20%** | 10% | 10% | 5% |
| 5. E-E-A-T | 10% | 15% | 15% | 10% |
| 6. Schema Markup | 10% | 15% | 5% | 10% |
| 7. Freshness | 5% | 10% | 10% | 5% |
| 8. Third-Party Mentions | 5% | 15% | 5% | 5% |
| 9. AI Crawlability | 5% | 5% | 5% | 5% |
| 10. Entity Clarity | 5% | 10% | 5% | 5% |

**Weighted Score Formula**: Multiply each dimension score (0–10) by its weight, sum all. Result is
0–10. Multiply by 10 for a 0–100 GEO Score.

---

## Score Interpretation

| GEO Score | Label | Meaning |
|-----------|-------|---------|
| 85–100 | 🟢 AI-Ready | High probability of citation across major LLMs |
| 65–84 | 🟡 Partially Optimized | Cited occasionally; clear gaps to address |
| 40–64 | 🟠 Needs Work | Rarely cited; fundamental GEO improvements needed |
| 0–39 | 🔴 Not AI-Ready | Virtually invisible to AI engines |

---

## Platform-Specific Behavior Notes

Always include a "Platform Notes" section in your review flagging model-specific issues:

### ChatGPT
- Cites only when Browse/web search is active; relies on training data otherwise
- Wikipedia presence is valuable — top cited source at 7.8% of citations
- Favors content **depth** and encyclopedic completeness
- ~90% of ChatGPT citations come from sources outside top 20 Google results
- Average 7.92 citations per response (vs Perplexity's 21.87)

### Perplexity
- Cites by default; real-time web retrieval is core to its product
- Highly favors **recency** — new content can appear in citations within hours
- Sends trackable referral traffic (visible in GA4 as perplexity.ai)
- Prefers community-driven content: Reddit (46.7%), YouTube (13.9%)
- 2.76× more citations per answer than ChatGPT — more opportunities for inclusion

### Claude (claude.ai)
- Does not cite unless explicitly requested by the user
- Prioritizes **technical accuracy** and Constitutional AI principles (helpful, harmless, honest)
- Sends low traffic volume but highest session value ($4.56/session vs $3.12 for Perplexity)
- Massive opportunity for early movers — still underoptimized by most brands
- Content that is well-structured, accurate, and technically detailed performs best

### Google AI Overviews
- Strongest correlation with traditional SEO rankings (93.67% of citations link to top-10 organic)
- 10.2 average links from 4 unique domains per Overview response
- Now appears in >50% of US searches (as of late 2025)

---

## Output Format

Always structure your GEO review as follows:

```
## GEO Content Review: [Page Title or URL]
**Content Type**: [Comparison/Brand/Blog/Lead Gen/FAQ]
**Review Date**: [Date]

---

### Dimension Scores

| # | Dimension | Score | Notes |
|---|-----------|-------|-------|
| 1 | Answer-First Structure | X/10 | [brief note] |
| 2 | FAQ & Natural Language | X/10 | [brief note] |
| 3 | Factual Density | X/10 | [brief note] |
| 4 | Comparison Content | X/10 | [brief note] |
| 5 | E-E-A-T Signals | X/10 | [brief note] |
| 6 | Schema Markup | X/10 | [brief note] |
| 7 | Content Freshness | X/10 | [brief note] |
| 8 | Third-Party Mentions | X/10 | [brief note] |
| 9 | AI Crawlability | X/10 | [brief note] |
| 10 | Entity Clarity | X/10 | [brief note] |

**Weighted GEO Score: XX/100** — [Label]

---

### Top 3 Quick Wins (Ranked by Impact ÷ Effort)

1. **[Action]** — [Why it matters] [Estimated impact: +X points]
2. **[Action]** — [Why it matters] [Estimated impact: +X points]
3. **[Action]** — [Why it matters] [Estimated impact: +X points]

---

### Platform-Specific Notes

- **ChatGPT**: [specific issue or recommendation]
- **Perplexity**: [specific issue or recommendation]
- **Claude**: [specific issue or recommendation]
- **Google AI Overviews**: [specific issue or recommendation]

---

### Summary

[2–3 sentence overall assessment. Be honest about the gap between current state and AI-ready.]
```

---

## Key Research References

For deep dives, see `references/geo-sources.md` which contains the full 19-source research base
used to build this skill, including:
- Princeton GEO paper (Aggarwal et al., ACM SIGKDD 2024)
- Qwairy citation behavior study (118K responses, Q3 2025)
- Omnius GEO Industry Report 2025
- Profound AI Platform Citation Patterns (680M citations)
- Superprompt.com analysis (400+ sites, 2025)
