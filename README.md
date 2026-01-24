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

## Quick Start

### 1. Install the Agent

Copy the agent file to your Claude Code agents directory:

```bash
cp agents/markdown-zh-tw-translator.md ~/.claude/agents/
```

### 2. Install the Skill

Copy the skill guide to your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills
cp skills/book-translation-guide.md ~/.claude/skills/
```

### 3. Prepare Your Book

Ensure your book is in Markdown format with proper `#` headings:

```markdown
# Book Title

## Chapter 1: Introduction

Content here...

## Chapter 2: Getting Started

More content...
```

### 4. Run the Translation

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
