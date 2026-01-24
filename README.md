# Book Translation Agent for Claude Code

A systematic workflow for translating entire books between **any languages** using Claude Code agents.

## Supported Languages

This workflow supports translation between **any language pair**. Examples:

- English → Traditional Chinese (Taiwan)
- English → Japanese
- English → Spanish
- French → English
- German → Korean
- Any source language → Any target language

The included example agent translates English → Traditional Chinese (Taiwan), but you can easily create agents for other language pairs.

## Prerequisites

**This workflow only works with books that have been parsed to Markdown format.**

The source file must be a `.md` file with proper heading structure using `#` symbols. If your book is in PDF, EPUB, or other formats, you must first convert it to Markdown before using this translation workflow.

### Recommended Tools for Converting to Markdown

- [Pandoc](https://pandoc.org/) - Universal document converter
- [Calibre](https://calibre-ebook.com/) - E-book management (for EPUB)
- PDF to Markdown converters (various online tools)
- OCR tools for scanned books

## What This Does

This agent skill enables you to:

1. **Analyze** a book's structure (chapters, sections, headings)
2. **Split** the book into manageable chapter files
3. **Translate** each chapter using a translator agent
4. **Verify** translation completeness
5. **Produce** a fully translated book in your target language

## Components

```
book-translation-agent/
├── README.md                           # This file
├── agents/
│   └── markdown-zh-tw-translator.md    # Example: EN→ZH-TW translator agent
└── skills/
    └── book-translation-guide.md       # Step-by-step workflow guide
```

## Installation

### Option 1: Ask Your Agent to Install (Recommended)

Simply open Claude Code and say:

```
Install the book translation agent from https://github.com/madeyexz/book-translation-agent
```

Your Claude Code agent will:
1. Clone the repository
2. Copy the agent and skill files to the correct locations
3. Confirm the installation is complete

### Option 2: Manual Installation

**Step 1**: Clone this repository

```bash
git clone https://github.com/madeyexz/book-translation-agent.git
cd book-translation-agent
```

**Step 2**: Copy the agent file

```bash
mkdir -p ~/.claude/agents
cp agents/markdown-zh-tw-translator.md ~/.claude/agents/
```

**Step 3**: Copy the skill file

```bash
mkdir -p ~/.claude/skills
cp skills/book-translation-guide.md ~/.claude/skills/
```

**Step 4**: Verify installation

```bash
ls ~/.claude/agents/markdown-zh-tw-translator.md
ls ~/.claude/skills/book-translation-guide.md
```

---

## Quick Start

### 1. Prepare Your Book

Ensure your book is in Markdown format with proper `#` headings:

```markdown
# Book Title

## Chapter 1: Introduction

Content here...

## Chapter 2: Getting Started

More content...
```

### 2. Run the Translation

In Claude Code, specify your source and target languages:

```
Translate the book at /path/to/book.md from English to Traditional Chinese using the book translation workflow.
```

Other examples:

```
Translate the book at /path/to/book.md from English to Japanese.
```

```
Translate the book at /path/to/book.md from French to English.
```

```
Translate the book at /path/to/book.md from German to Spanish.
```

---

## Creating Custom Translator Agents

The included `markdown-zh-tw-translator.md` is an example for English → Traditional Chinese. To create a translator for other language pairs:

### 1. Copy and Rename the Template

```bash
cp ~/.claude/agents/markdown-zh-tw-translator.md ~/.claude/agents/markdown-ja-translator.md
```

### 2. Modify the Agent

Edit the new file and change:

- `name`: e.g., `markdown-ja-translator`
- `description`: Update for your language pair
- Language-specific conventions in the prompt
- Quality standards for your target language

### Example: Japanese Translator Agent

Key modifications for a Japanese translator:

```markdown
name: markdown-ja-translator
description: "Translates markdown content to Japanese..."

## Quality Standards
1. Use appropriate politeness levels (敬語/丁寧語/普通体)
2. Handle technical terms: カタカナ for loanwords, or 日本語 (English)
3. Maintain natural Japanese sentence structure
```

### Example: Spanish Translator Agent

Key modifications for a Spanish translator:

```markdown
name: markdown-es-translator
description: "Translates markdown content to Spanish..."

## Quality Standards
1. Use appropriate formality (tú vs usted)
2. Handle regional variations if needed (Spain vs Latin America)
3. Technical terms: término en español (English term)
```

---

## Translation Quality Guidelines

General principles that apply to all languages:

| Element | Handling |
|---------|----------|
| Personal names | Keep original (language-dependent exceptions apply) |
| Technical terms | Bilingual format recommended: 翻譯 (Translation) |
| Markdown | Preserve all formatting |
| Code blocks | Never translate code |
| Cultural references | Adapt appropriately for target audience |

---

## Workflow Overview

```
book.md (Markdown source)
    ↓
Analyze Structure (grep headings)
    ↓
Split into Chapters (sed by line numbers)
    ↓
Check File Sizes (<60KB each)
    ↓
Batch Translate (5 chapters at a time)
    ↓
Verify Completeness (compare endings)
    ↓
Translated Book (multiple .md files)
```

---

## Tested Results

| Book | Source | Target | Size | Chapters | Time |
|------|--------|--------|------|----------|------|
| 21 Lessons for the 21st Century | EN | ZH-TW | 714 KB | 25 | ~30 min |
| Homo Deus | EN | ZH-TW | 960 KB | 16 | ~40 min |

---

## Troubleshooting

### Rate Limit Errors
Reduce batch size from 5 to 3 chapters.

### Token Limit Exceeded
Split large chapters (>60KB) into smaller parts.

### Incomplete Translation
Re-run translation for the specific chapter; verify by comparing file endings.

### Wrong Language Conventions
Create a custom translator agent with specific rules for your target language/region.

---

## License

MIT License - Feel free to use, modify, and distribute.

## Contributing

Pull requests welcome! Especially:
- New translator agents for different language pairs
- Improvements to the workflow
- Bug fixes and documentation updates
