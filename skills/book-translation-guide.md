# Book Translation Guide for Agents

## Prerequisites

**IMPORTANT**: This skill only works if the book has already been parsed to Markdown format. The source file must be a `.md` file with proper heading structure using `#` symbols.

If the book is in PDF, EPUB, or other formats, it must first be converted to Markdown before using this translation workflow.

---

## Overview

This guide provides a systematic approach to translating entire books from English to Traditional Chinese (Taiwan) using the `markdown-zh-tw-translator` agent.

---

## Step 1: Analyze Document Structure

First, understand the book's structure:

```bash
# Check file size and line count
wc -l "book.md" && ls -la "book.md"

# Find all chapter headings (lines starting with #)
grep "^# " "book.md"
```

This reveals:
- Total length of the book
- Number of chapters and their titles
- Starting line numbers for each chapter

---

## Step 2: Create Directory Structure

```bash
mkdir -p "Book-Chapters"        # For original chapter files
mkdir -p "Book-Chapters-ZH-TW"  # For translated chapter files
```

---

## Step 3: Split into Chapters

Use `sed` to extract chapters by line numbers:

```bash
sed -n 'START,ENDp' "book.md" > "Book-Chapters/00-Chapter-Name.md"
```

Example for a typical book:

```bash
sed -n '1,55p' "book.md" > "Book-Chapters/00-Front-Matter.md"
sed -n '56,593p' "book.md" > "Book-Chapters/01-Chapter-One.md"
sed -n '594,841p' "book.md" > "Book-Chapters/02-Chapter-Two.md"
# Continue for all chapters...
```

**CRITICAL**: Keep each file under **50-60KB** to avoid token limits.

---

## Step 4: Handle Oversized Chapters

If a chapter exceeds 60KB, split it further:

```bash
# Check chapter structure
grep "^# " "07-Large-Chapter.md"

# Split into parts at a logical section break
sed -n '1,231p' "07-Large-Chapter.md" > "07a-Part1.md"
sed -n '232,471p' "07-Large-Chapter.md" > "07b-Part2.md"
```

After translation, merge the parts:

```bash
cat "Book-ZH-TW/07a-Part1.md" "Book-ZH-TW/07b-Part2.md" > "Book-ZH-TW/07-Large-Chapter.md"
rm "Book-ZH-TW/07a-Part1.md" "Book-ZH-TW/07b-Part2.md"
```

---

## Step 5: Batch Translation Strategy

Translate **5 chapters at a time**, waiting for completion before proceeding:

```
Batch 1: Chapters 00-04 → Wait for completion
Batch 2: Chapters 05-09 → Wait for completion
Batch 3: Chapters 10-14 → Wait for completion
...continue until done
```

Benefits:
- Avoids rate limits
- Allows early detection of issues
- Ensures consistent translation quality

---

## Step 6: Invoke Translation Agent

For each chapter, call the translation agent:

```
Task(
  subagent_type: "markdown-zh-tw-translator",
  description: "Translate Chapter X to ZH-TW",
  prompt: "Translate the markdown file at /path/to/Book-Chapters/XX-Chapter.md
           from English to Traditional Chinese (繁體中文).

           Save the translated file to /path/to/Book-Chapters-ZH-TW/XX-Chapter.md

           Use your translation guidelines to ensure high quality translation
           while preserving markdown formatting."
)
```

Run 5 agents in parallel for each batch.

---

## Step 7: Verify Translation Completeness

After each batch, verify translations are complete:

```bash
# Compare original ending
tail -5 "Book-Chapters/01-Chapter.md"

# Compare translated ending
tail -5 "Book-Chapters-ZH-TW/01-Chapter.md"
```

Ensure the final sentences match in meaning.

For bulk verification:

```bash
for i in 00 01 02 03 04; do
  echo "=== $i ==="
  tail -3 "Book-Chapters-ZH-TW/$i-"*.md
  echo ""
done
```

---

## Translation Quality Standards

The `markdown-zh-tw-translator` agent should follow these conventions:

| Element | Handling |
|---------|----------|
| Personal names | Keep original (Donald Trump, Einstein) |
| Technical terms | Bilingual: 人工智慧 (AI), 演算法 (Algorithm) |
| Book titles | Use official translations: 《人類大歷史》(Sapiens) |
| Taiwan conventions | Use 軟體, 程式, 資料, 網路 (not Mainland terms) |
| Markdown formatting | Preserve all: headers, images, links, footnotes |
| LaTeX/Math | Keep original notation |

---

## Complete Workflow Diagram

```
Original File (book.md)
        ↓
[1] Analyze Structure (grep chapter headings)
        ↓
[2] Create Directories
        ↓
[3] Split into Chapters (sed by line numbers)
        ↓
[4] Check File Sizes (split if >60KB)
        ↓
[5] Batch Translate (5 chapters per batch)
        ↓
[6] Merge Split Chapters (if any)
        ↓
[7] Verify Completeness (tail comparison)
        ↓
[8] Done - All chapters translated
```

---

## Troubleshooting

### Rate Limit Errors
- Reduce batch size from 5 to 3 chapters
- Wait longer between batches

### Token Limit Exceeded
- Split the chapter into smaller parts
- Target 40-50KB per file instead of 60KB

### Incomplete Translation
- Check if agent hit limits mid-translation
- Re-run translation for that specific chapter
- Verify by comparing file endings

### Missing Chapters
- List output directory to identify gaps
- Re-translate missing files individually

---

## Example: Real Book Translation

| Book | Original Size | Chapters | Translation Time |
|------|---------------|----------|------------------|
| 21 Lessons for the 21st Century | 714 KB | 25 | ~30 minutes |
| Homo Deus | 960 KB | 16 | ~40 minutes |

---

## Notes

1. Always verify the source markdown has proper `#` heading structure
2. Back up original files before starting
3. Keep a checklist of chapters to track progress
4. For books >1MB, consider translating over multiple sessions
5. The `markdown-zh-tw-translator` agent must be properly configured before use
