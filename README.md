# Ian's Skills & Agents

Skills and agents I use with AI coding assistants.

## What's Inside

```
ian-skills-agents/
├── agents/                    # Custom agents for specialized tasks
│   └── markdown-zh-tw-translator.md
├── skills/                    # Workflow guides and design principles
│   ├── book-translation-guide.md
│   ├── information-software-design.md
│   └── pr-screenshot.md
└── README.md
```

## Installation

### Install all skills & agents

```bash
npx skills add madeyexz/ian-skills-agents
```

### Install a specific skill or agent

```bash
# Install only the PR Screenshot skill
npx skills add madeyexz/ian-skills-agents/skills/pr-screenshot

# Install only the translator agent
npx skills add madeyexz/ian-skills-agents/agents/markdown-zh-tw-translator
```

### Compatible agents

Works with [20+ AI coding agents](https://skills.sh) including:
- Claude Code
- Cursor
- Windsurf
- Cline
- GitHub Copilot

### Disable telemetry (optional)

```bash
DISABLE_TELEMETRY=1 npx skills add madeyexz/ian-skills-agents
```

<details>
<summary>Manual installation</summary>

```bash
git clone https://github.com/madeyexz/ian-skills-agents.git
cd ian-skills-agents

# Install agents
mkdir -p ~/.claude/agents
cp agents/*.md ~/.claude/agents/

# Install skills
mkdir -p ~/.claude/skills
cp skills/*.md ~/.claude/skills/
```

</details>

---

## Skills

| Skill | Description |
|-------|-------------|
| **Information Software Design** | Design principles from Bret Victor's "Magic Ink" — ask "what can the user *learn*?" not "what can the user *do*?" |
| **Book Translation Guide** | Workflow for translating entire books: split → batch translate → verify |
| **PR Screenshot** | Add screenshots to PRs using browser automation — capture UI, save to docs/screenshot/, update PR body with GitHub CDN URLs |

## Agents

| Agent | Description |
|-------|-------------|
| **markdown-zh-tw-translator** | Markdown → Traditional Chinese (Taiwan conventions) |

---

## License

MIT
