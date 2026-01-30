# Claude Code Skills & Agents

A personal collection of skills and agents for Claude Code.

## What's Inside

```
claude-skills-agents/
├── agents/                    # Custom agents for specialized tasks
│   └── markdown-zh-tw-translator.md
├── skills/                    # Workflow guides and design principles
│   ├── book-translation-guide.md
│   └── information-software-design.md
└── README.md
```

## Installation

### Option 1: Ask Claude Code to Install (Recommended)

```
Install skills and agents from https://github.com/madeyexz/claude-skills-agents
```

Claude Code will clone the repo and copy files to the correct locations.

### Option 2: Manual Installation

```bash
git clone https://github.com/madeyexz/claude-skills-agents.git
cd claude-skills-agents

# Install agents
mkdir -p ~/.claude/agents
cp agents/*.md ~/.claude/agents/

# Install skills
mkdir -p ~/.claude/skills
cp skills/*.md ~/.claude/skills/
```

---

## Skills

### Information Software Design

Design principles from Bret Victor's "Magic Ink" for building software that helps users **learn, understand, compare, and make decisions**.

**Core insight**: Ask "What can the user *learn*?" not "What can the user *do*?"

**Use when designing**: dashboards, search results, calendars, recommendation systems, listings, or any interface where users seek answers.

**Key principles**:
- Infer context from environment and history; interaction is a last resort
- Show enough data to answer questions without clicking
- Arrange data spatially to enable comparison by eye, not memory
- Minimize navigation (it's almost always pure excise)

### Book Translation Guide

A systematic workflow for translating entire books between any language pairs using Claude Code agents.

**Workflow**:
1. Analyze book structure (grep headings)
2. Split into chapters (<60KB each)
3. Batch translate (5 chapters at a time)
4. Verify completeness (compare endings)

**Tested on**:
- 21 Lessons for the 21st Century (714 KB, 25 chapters)
- Homo Deus (960 KB, 16 chapters)

---

## Agents

### markdown-zh-tw-translator

Translates markdown content to Traditional Chinese (Taiwan conventions).

**Features**:
- Preserves all markdown formatting
- Keeps personal names in original language
- Technical terms in bilingual format: 中文 (English)
- Uses Taiwan conventions: 軟體, 程式, 資料, 網路

**Usage**:
```
Translate this README.md to Traditional Chinese
```

---

## Creating New Skills & Agents

### Adding a Skill

Create a markdown file in `skills/` with:
- Clear purpose and when to use it
- Step-by-step workflow or design principles
- Examples and anti-patterns
- Checklists where appropriate

### Adding an Agent

Create a markdown file in `agents/` with frontmatter:

```yaml
---
name: agent-name
description: "When to use this agent..."
tools: Bash, Read, Write, Edit, ...
model: inherit
---

[Agent instructions here]
```

---

## Contributing

Pull requests welcome for:
- New skills (design principles, workflows, guides)
- New agents (translators, analyzers, generators)
- Improvements to existing content

---

## License

MIT
