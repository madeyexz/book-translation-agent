# Ian's Skills & Agents

Skills and agents I use with AI coding assistants.

## What's Inside

```
ian-skills-agents/
├── agents/
│   └── markdown-zh-tw-translator.md
├── skills/
│   ├── book-translation-guide/
│   │   └── SKILL.md
│   ├── information-software-design/
│   │   └── SKILL.md
│   └── pr-screenshot/
│       └── SKILL.md
└── README.md
```

## Installation

### Install all skills

```bash
npx skills add madeyexz/ian-skills-agents
```

### Install a specific skill

```bash
# PR Screenshot
npx skills add madeyexz/ian-skills-agents/skills/pr-screenshot

# Information Software Design
npx skills add madeyexz/ian-skills-agents/skills/information-software-design

# Book Translation Guide
npx skills add madeyexz/ian-skills-agents/skills/book-translation-guide
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

# Copy all skills
cp -r skills/*/ ~/.claude/skills/
```

</details>

---

## Skills

| Skill | Description |
|-------|-------------|
| **pr-screenshot** | Add screenshots to PRs using browser automation — capture UI, save to docs/screenshot/, update PR body with GitHub CDN URLs |
| **information-software-design** | Design principles from Bret Victor's "Magic Ink" — ask "what can the user *learn*?" not "what can the user *do*?" |
| **book-translation-guide** | Workflow for translating entire books: split → batch translate → verify |

## Agents

Agents are not installable via `npx skills`. Copy manually to `~/.claude/agents/`.

| Agent | Description |
|-------|-------------|
| **markdown-zh-tw-translator** | Translate markdown to Traditional Chinese (Taiwan conventions) |

---

## License

MIT
