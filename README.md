# Book Translation Agent for Claude Code

A systematic workflow for translating entire books from English to Traditional Chinese (Taiwan) using Claude Code agents.

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
3. **Translate** each chapter using the `markdown-zh-tw-translator` agent
4. **Verify** translation completeness
5. **Produce** a fully translated book in Traditional Chinese

## Components

```
book-translation-agent/
├── README.md                           # This file
├── agents/
│   └── markdown-zh-tw-translator.md    # Translation agent prompt
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

In Claude Code, ask:

```
Translate the book at /path/to/book.md to Traditional Chinese using the book translation workflow.
```

## Translation Quality

The translator agent follows these conventions:

| Element | Handling |
|---------|----------|
| Personal names | Keep original (e.g., "John Smith" stays as "John Smith") |
| Technical terms | Bilingual format: 機器學習 (Machine Learning) |
| Taiwan conventions | Uses 軟體, 程式, 資料, 網路 |
| Markdown | Preserves all formatting |

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

## Tested Results

| Book | Original Size | Chapters | Time |
|------|---------------|----------|------|
| 21 Lessons for the 21st Century | 714 KB | 25 | ~30 min |
| Homo Deus | 960 KB | 16 | ~40 min |

## Troubleshooting

### Rate Limit Errors
Reduce batch size from 5 to 3 chapters.

### Token Limit Exceeded
Split large chapters (>60KB) into smaller parts.

### Incomplete Translation
Re-run translation for the specific chapter; verify by comparing file endings.

## License

MIT License - Feel free to use, modify, and distribute.

## Contributing

Pull requests welcome! Please ensure any changes maintain compatibility with Claude Code's agent system.
