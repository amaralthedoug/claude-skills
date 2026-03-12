---
name: web-tool-selector
description: >
  Decision framework for choosing between Firecrawl MCP, Playwright MCP, and Playwright CLI
  for web scraping, browser automation, and testing tasks in Claude Code and AI agent workflows.
  ALWAYS use this skill when the task involves: visiting a URL, scraping a website, extracting
  web content, running browser tests, automating a web flow, logging into a site, or any
  combination of "read/extract/test/automate + website/URL/browser". Also trigger when the
  user asks "which tool should I use for X" where X involves the web. Use this skill before
  picking any tool — wrong choices waste tokens and break long sessions.
---

# Web Tool Selector for Claude Code Agents

A decision framework for selecting the right web automation tool. Wrong choices cost tokens,
break long sessions, or fail entirely. Always consult this skill before acting on any
web-related task.

---

## TL;DR Decision Tree

```
Is the site public and do you only need to READ content (no clicks, no login)?
  YES → Firecrawl MCP (fastest, cheapest, LLM-ready output)

Does the task require interaction (login, click, scroll, fill form)?
  Are you in Claude Code / have filesystem access?
    YES → Playwright CLI (4x more token-efficient than MCP)
    NO  → Playwright MCP (sandboxed environments like Claude Desktop)

Are you running existing .spec.ts tests or CI/CD?
  → Playwright CLI (npx playwright test)

Exploring an unknown app or doing short sessions (<10 steps)?
  → Playwright MCP (rich page introspection, no disk needed)
```

---

## Tool Profiles

### Firecrawl MCP
**Best for:** Public web content extraction → clean Markdown/JSON for LLM consumption.

| Strength | Detail |
|---|---|
| Speed | Fastest option — no browser launch overhead |
| Output | Returns clean Markdown, ready to pass directly to Claude |
| Anti-bot | Handles proxies, CAPTCHAs, and dynamic JS automatically |
| Scale | Batch scraping, site crawling, structured data extraction |
| Cost | Cheapest — uses API credits, not model tokens |

**Use when:**
- Scraping blogs, newsletters, documentation, news sites
- Extracting structured data from public pages
- Monitoring multiple URLs (batch mode)
- The Firecrawl `/agent` endpoint — give it a prompt and it navigates autonomously

**Do NOT use when:** Site requires login, has aggressive bot detection, or needs real interaction.

---

### Playwright CLI (`@playwright/cli`)
**Best for:** Claude Code and any agent with filesystem + shell access.

Token cost per task: **~27,000 tokens** ✅

| Strength | Detail |
|---|---|
| Token efficiency | ~4x cheaper than MCP (27K vs 114K tokens per task) |
| Long sessions | Context stays flat — step 50 costs same as step 5 |
| State management | Saves snapshots as YAML to disk; agent reads only when needed |
| Test generation | `--codegen` flag generates `.spec.ts` files automatically |
| CI/CD | Runs `npx playwright test` deterministically |

**Use when:**
- You are in Claude Code (has filesystem access by default)
- Sessions longer than 10–15 steps
- Generating or running Playwright test files
- CI/CD pipelines
- Juggling browser automation + large codebase in same session

**Key commands:**
```bash
playwright-cli snapshot          # Get page structure as YAML (saved to disk)
playwright-cli click e21         # Click element by reference ID
playwright-cli fill e35 "text"   # Fill input field
playwright-cli screenshot        # Save screenshot to disk
playwright-cli state-save auth.json  # Persist login session
playwright-cli state-load auth.json  # Restore login session
```

**Install:**
```bash
claude mcp add --scope user playwright npx @playwright/mcp@latest
# Then in sessions, use playwright-cli commands via Bash tool
```

---

### Playwright MCP (`@playwright/mcp`)
**Best for:** Sandboxed environments, short exploratory sessions, multi-agent coordination.

Token cost per task: **~114,000 tokens** ⚠️

| Strength | Detail |
|---|---|
| No filesystem needed | Works in Claude Desktop, browser-based assistants |
| Rich introspection | Full accessibility tree inline — great for exploration |
| Persistent state | Browser context survives across turns in same session |
| Multi-agent | Multiple agents can share the same browser session |
| Login UX | Opens visible Chrome window — you log in manually, cookies persist |

**Use when:**
- Running in Claude Desktop (no shell access)
- Short exploratory sessions (< 10–15 steps)
- Building test charters — mapping all interactive elements of an app
- Multi-agent workflows sharing browser state
- First-time exploration of an unknown application

**Do NOT use for:**
- Sessions longer than 15 steps (context bloats to 60–80K tokens of stale state)
- Claude Code on large codebases (leaves no room for code in context)
- CI/CD pipelines

**Install (Claude Code):**
```bash
claude mcp add --scope user playwright npx @playwright/mcp@latest
```
> Always say "playwright mcp" explicitly in first message — Claude Code may default to Bash otherwise.

---

## Recommended Hybrid Workflow (for QA/Testing)

The most powerful pattern — used by production teams:

```
Phase 1: EXPLORATION  → Playwright MCP
  Map the app, find flows, understand elements
  Keep it short: < 10 steps per area

Phase 2: EXECUTION    → Playwright CLI
  Run the concrete test steps (30–50+ steps)
  Token-efficient, context stays clean

Phase 3: REGRESSION   → npx playwright test
  No AI at all — run the generated .spec.ts deterministically
  Don't burn tokens on stable tests
```

---

## Quick Comparison Table

| Criteria | Firecrawl MCP | Playwright CLI | Playwright MCP |
|---|---|---|---|
| Token cost | Minimal (API credits) | ~27K per task | ~114K per task |
| Needs filesystem | No | Yes | No |
| Login/interaction | No (use Browser Sandbox for that) | Yes | Yes |
| Long sessions (30+ steps) | N/A | ✅ Best | ❌ Context bloat |
| Public content extraction | ✅ Best | Overkill | Overkill |
| CI/CD / test execution | No | ✅ Best | No |
| Exploratory testing | No | Limited | ✅ Best |
| Multi-agent coordination | No | No | ✅ Yes |
| Sandboxed environment | Yes | No | Yes |

---

## Forcing the Right Tool in Claude Code

If Claude picks the wrong tool, be explicit:

```
"Use Firecrawl MCP to extract content from..."
"Use Playwright CLI (via Bash) to navigate and..."
"Use Playwright MCP to open the browser and explore..."
```

---

## CLAUDE.md Snippet (add to project)

```markdown
## Web Tool Selection
Follow skill `web-tool-selector`. Quick rules:
- Public content extraction → Firecrawl MCP
- Interactive automation, long sessions, CI → Playwright CLI
- Short exploration, sandboxed env → Playwright MCP

Project exceptions:
- BestGuide E2E tests: always use Playwright CLI (specs in /tests)
```

---

## Sources
- Microsoft Playwright MCP README: https://github.com/microsoft/playwright-mcp
- Playwright CLI vs MCP benchmark: https://testcollab.com/blog/playwright-mcp
- Token analysis (27K vs 114K): https://www.morphllm.com/playwright-mcp
- Hybrid workflow pattern: https://scrolltest.medium.com/playwright-mcp-burns-114k-tokens
- Firecrawl MCP capabilities: https://github.com/firecrawl/firecrawl-mcp-server
- Browser automation tools comparison: https://www.firecrawl.dev/blog/browser-automation-tools-comparison
