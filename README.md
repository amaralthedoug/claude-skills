# Claude Skills

A curated collection of custom skills for Claude Code and other AI agents, covering web automation, content optimization, document preparation, and more.

## 🎯 Quick Overview

| Category | Skills | Purpose |
|----------|--------|---------|
| **🌐 Web Tools** | web-tool-selector | Choose the right web automation tool (Firecrawl/Playwright) |
| **🎯 Content Optimization** | geo-content-reviewer | Audit and optimize content for AI citation (ChatGPT, Claude, Perplexity, Google AI) |
| **📄 Documents & Resumes** | resume-format, resume-tailor | Professional resume formatting and job-specific tailoring |

## 📚 Available Skills

### 🌐 Web Tools

#### [web-tool-selector](./skills/web-tool-selector/)

**Decision framework for choosing the right web automation tool**

Helps you select between Firecrawl MCP, Playwright CLI, and Playwright MCP based on your specific use case, optimizing for token efficiency and functionality.

**When to use:**
- Any task involving web scraping, browser automation, or web testing
- When deciding which tool to use for a web-related task
- QA/Testing workflows requiring hybrid approaches

**Key features:**
- Comparison matrix: Firecrawl MCP (fastest, cheapest) vs Playwright CLI (~27K tokens/task) vs Playwright MCP (~114K tokens/task)
- Token cost analysis (4x difference between tools!)
- Recommended workflows for different scenarios
- Hybrid automation strategies

---

### 🎯 Content Optimization

#### [geo-content-reviewer](./skills/geo-content-reviewer/)

**GEO (Generative Engine Optimization) content auditor and scorer**

Analyzes web pages, blog posts, product listings, comparison pages, and review/lead-gen content for AI citation readiness across ChatGPT, Claude, Perplexity, and Google AI Overviews. Based on Princeton/ACM SIGKDD 2024 research showing structured GEO methods can increase AI visibility by up to 40%.

**When to use:**
- Review content for GEO/AI citation readiness
- Check if a page will be cited by AI engines
- Audit URLs or text for AI visibility
- Optimize content so ChatGPT/Claude recommends it
- Analyze why a page isn't appearing in AI answers
- Score comparison/affiliate/lead-gen pages for AI discoverability

**Key features:**
- 10-dimension scoring system (Answer-First Structure, FAQ Coverage, Factual Density, etc.)
- Content-type-specific weights (Comparison/Lead Gen, Brand Page, Blog, FAQ)
- Platform-specific recommendations (ChatGPT, Perplexity, Claude, Google AI Overviews)
- Top 3 Quick Wins ranked by impact-to-effort ratio
- Research-backed methodology from 19+ authoritative sources

**Output:**
- Weighted GEO Score (0-100) with interpretation
- Dimension-by-dimension analysis
- Actionable improvement recommendations
- Platform-specific optimization notes

---

### 📄 Documents & Resumes

#### [resume-format](./skills/docs/resume-format/)

**Expert resume formatting and ATS optimization guide**

Comprehensive guide synthesized from 30+ authoritative sources including Harvard Career Services, TheLadders eye-tracking research, Jobscan, Indeed, Oxford Careers, and more.

**When to use:**
- Resume formatting questions
- Font selection, margins, spacing decisions
- ATS compliance verification
- File format selection
- Section structure and visual hierarchy
- Professional typography guidance

**Key features:**
- Two-audience approach (ATS systems + Human recruiters)
- Evidence-based recommendations from 30+ sources
- ATS-friendly formatting rules
- Visual hierarchy principles
- Typography and layout best practices

#### [resume-tailor](./skills/docs/resume-tailor/)

**Job-specific resume tailoring system**

Tailors Doug's (Douglas Rafael do Amaral) resume for specific job postings with ethical reframing, keyword alignment, and strategic positioning across two target profiles: QA/Process Engineering and AI Quality/Evaluation/MLOps.

**When to use:**
- Adapting resume to specific job postings
- Keyword alignment and optimization
- Profile customization for different roles
- Honest reframing of experience without fabrication

**Key features:**
- Ethical tailoring without fabrication
- Two target profiles (QA Engineering → AI Quality/MLOps transition)
- Job-specific keyword optimization
- Strategic bullet point reordering
- ATS-ready output

---

## 🚀 Installation

### Option 1: Copy Individual Skills

```bash
# Copy a specific skill to Claude's global skills directory
cp -r skills/web-tool-selector ~/.claude/skills/

# Or copy all skills at once
cp -r skills/* ~/.claude/skills/
```

### Option 2: Create Compressed .skill Files

```bash
# Create a compressed skill package
cd skills/geo-content-reviewer
zip -r ../../geo-content-reviewer.skill .
```

Then copy the `.skill` file to `~/.claude/skills/`

### Option 3: Clone Repository

```bash
# Clone the repository and symlink skills
git clone https://github.com/yourusername/claude-skills.git
ln -s $(pwd)/claude-skills/skills/* ~/.claude/skills/
```

### Using Skills in Conversations

**Invoke by name:**
```
/geo-content-reviewer
/web-tool-selector
/resume-format
```

**Or describe your need:**
- "Check if this page will be cited by ChatGPT"
- "Which tool should I use for web scraping?"
- "Format my resume for ATS systems"

Claude will automatically suggest and load relevant skills based on your request.

---

## 📁 Repository Structure

```
claude-skills/
├── .claude/                                # Claude Code configuration (local settings)
│   └── settings.local.json                 # Project-specific settings
├── .git/                                   # Git repository data
├── README.md                               # This file
└── skills/                                 # Skills directory
    ├── docs/                               # Document-related skills
    │   ├── resume-format/                  # Resume formatting & ATS guide
    │   │   └── skill.md
    │   └── resume-tailor/                  # Job-specific resume tailoring
    │       └── skill.md
    ├── geo-content-reviewer/               # GEO content analysis
    │   ├── SKILL.md                        # Main skill file
    │   └── references/                     # Research sources
    │       └── geo-sources.md
    └── web-tool-selector/                  # Web automation tool selection
        └── skill.md
```

**Note about `.claude/` directory:** This is a standard Claude Code configuration directory that stores project-specific settings. It's automatically created by Claude Code and should remain at the project root. It's similar to `.vscode/` or `.idea/` directories in other IDEs.

---

## ❓ FAQ

### Why is there a `.claude/` directory in this project?

The `.claude/` directory is a standard Claude Code configuration directory that stores project-specific settings like:
- Local settings overrides
- Project-specific memory
- Custom keybindings
- MCP server configurations

It's automatically created by Claude Code when you work on a project, similar to how VSCode creates `.vscode/` or JetBrains IDEs create `.idea/`. This directory should remain at the project root and can be committed to version control (though you may want to add specific files to `.gitignore` if they contain sensitive data).

### Do I need to install skills globally?

No! You can:
- **Global installation**: Copy skills to `~/.claude/skills/` to use them across all projects
- **Project-specific**: Keep skills in your project's `skills/` directory and reference them locally
- **Hybrid approach**: Use global skills for general-purpose tools, project-specific for custom domain skills

### How do I know if a skill is loaded?

When you start a conversation, Claude Code will list available skills in a system reminder. You can also invoke a skill by name (e.g., `/geo-content-reviewer`) and Claude will confirm if it's loaded.

---

## 🤝 Contributing

### Suggesting Improvements

Feel free to suggest improvements to existing skills via issues or pull requests!

### Adding New Skills

When creating a new skill, follow this structure:

```
skills/
└── your-skill-name/
    ├── SKILL.md or skill.md      # Main skill file with frontmatter
    ├── references/                # Optional: research sources, docs
    │   └── sources.md
    └── examples/                  # Optional: example outputs
        └── example-1.md
```

**Skill file frontmatter:**
```yaml
---
name: your-skill-name
description: >
  Clear description of when to use this skill. Include trigger phrases
  and use cases that would make Claude automatically suggest this skill.
---
```

**Best practices:**
- Write clear, actionable instructions
- Include examples and templates
- Document data sources and research
- Specify when NOT to use the skill
- Test with Claude Code before submitting

### Skill Quality Checklist

- [ ] Clear name and description in frontmatter
- [ ] Explicit trigger conditions (when to use)
- [ ] Step-by-step workflow or process
- [ ] Examples or templates included
- [ ] References to authoritative sources (if applicable)
- [ ] Edge cases and limitations documented
- [ ] Tested in Claude Code conversations

## 📄 License

MIT
