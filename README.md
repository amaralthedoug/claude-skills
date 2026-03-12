# Claude Skills

A curated collection of custom skills for Claude Code and other AI agents.

## 📚 Available Skills

### 🌐 Web Tools

#### [web-tool-selector](./skills/web-tool-selector/)

Decision framework for choosing the right web automation tool across different scenarios:
- **Firecrawl MCP** - Public content extraction (fastest, cheapest)
- **Playwright CLI** - Interactive automation with filesystem access (~27K tokens/task)
- **Playwright MCP** - Exploratory sessions in sandboxed environments (~114K tokens/task)

**When to use:** Any task involving web scraping, browser automation, or web testing.

**Key benefits:**
- Avoid wrong choices that waste tokens (4x difference!)
- Recommended hybrid workflow for QA/Testing
- Detailed cost comparison and use cases

---

### 📄 Documents & Resumes

#### [resume-format](./skills/docs/resume-format/)

Expert guide on resume layout, formatting, typography, spacing, visual hierarchy, and ATS optimization — synthesized from 30 authoritative sources including Harvard Career Services, TheLadders eye-tracking research, Jobscan, Indeed, Oxford Careers, and more.

**When to use:** Resume formatting questions, font selection, margins, spacing, ATS compliance, file formats, section structure, visual design.

**Key benefits:**
- Two-audience approach (ATS + Human recruiters)
- Evidence-based recommendations from 30+ sources
- ATS-friendly formatting rules
- Professional typography and layout guidance

#### [resume-tailor](./skills/docs/resume-tailor/)

Tailors Doug's (Douglas Rafael do Amaral) resume for specific job postings — honestly, strategically, and ATS-ready. Combines ethical reframing of real experience, keyword alignment, and positioning across two target profiles: QA/Process Engineering and AI Quality/Evaluation/MLOps.

**When to use:** Adapting resume to job postings, keyword alignment, profile customization, honest reframing.

**Key benefits:**
- Ethical tailoring without fabrication
- Two target profiles (QA Engineering → AI Quality transition)
- Job-specific keyword optimization
- Strategic bullet point reordering

---

## 🚀 Installation

### For Claude Code

```bash
# Copy skill to Claude's skills directory
cp skills/web-tool-selector/skill.md ~/.claude/skills/web-tool-selector/skill.md

# Or create as compressed .skill file
cd skills/web-tool-selector
zip -r ../../web-tool-selector.skill .
```

### During Conversations

Simply mention or invoke the skill:
```
/web-tool-selector
```

Claude will automatically suggest it when detecting web-related tasks.

---

## 📁 Repository Structure

```
claude-skills/
├── README.md                               # This file
└── skills/                                 # Skills directory
    ├── docs/                               # Document-related skills
    │   ├── resume-format/                  # Resume formatting & ATS guide
    │   │   └── skill.md
    │   └── resume-tailor/                  # Job-specific resume tailoring
    │       └── skill.md
    └── web-tool-selector/                  # Web automation tool selection
        └── skill.md
```

---

## 🤝 Contributing

Feel free to suggest improvements or add new skills via issues or pull requests!

## 📄 License

MIT
