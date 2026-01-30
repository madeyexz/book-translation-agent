# Ian's Skills & Agents

Skills and agents I use with AI coding assistants.

## What's Inside

```
ian-skills-agents/
├── agents/                    # Custom agents for specialized tasks
│   └── markdown-zh-tw-translator.md
├── skills/                    # Workflow guides and design principles
│   ├── book-translation-guide.md
│   └── information-software-design.md
└── README.md
```

## Installation

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

---

## Skills

| Skill | Description |
|-------|-------------|
| **Information Software Design** | Design principles from Bret Victor's "Magic Ink" — ask "what can the user *learn*?" not "what can the user *do*?" |
| **Book Translation Guide** | Workflow for translating entire books: split → batch translate → verify |

## Agents

| Agent | Description |
|-------|-------------|
| **markdown-zh-tw-translator** | Markdown → Traditional Chinese (Taiwan conventions) |

---

## License

MIT
