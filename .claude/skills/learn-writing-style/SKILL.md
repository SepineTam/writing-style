---
name: learn-writing-style
description: >
  Analyze writing style from user-provided example documents (PDF, DOCX, TXT, MD)
  and generate personalized writing rules. Use when the user wants to:
  (1) Learn or extract writing style from example papers/articles,
  (2) Create personalized writing rules for Claude to follow,
  (3) Build a writing-style preference profile from sample documents,
  (4) Apply a specific academic/professional writing style consistently.
  Triggers on keywords: "writing style", "learn style", "style analysis",
  "preference file", "writing rules", "write like", "模仿风格".
---

# Writing Style Analysis

## Workflow

This skill extracts writing style from user-provided documents through a 5-step pipeline.

### Step 1: Document Preprocessing

1. List all files in the user-specified directory.
2. Check file types. Supported: `.txt`, `.md`, `.docx`, `.pdf`.
3. For `.pdf` files, use `mcp__mineru__parse_documents` to extract text. Save output as `{original_name}.parsed.md` in the same directory.
4. For `.docx` files, extract text and save as `{original_name}.parsed.md`.
5. Generate an `INDEX.md` in the directory root with:
   - File list (original + parsed)
   - Document types and word counts
   - Brief content summary (use a quick scan or `head`)

### Step 2: Style Analysis (SubAgent)

Launch the pre-defined `style-analyzer` agent, specifying the directory:

```
Read INDEX.md and all parsed documents in {directory},
analyze writing style across 12 dimensions, and save the report as {directory}/ANALYSIS.md.
```

### Step 3: User Preference Elicitation

Read `ANALYSIS.md`, then use `AskUserQuestion` to confirm preferences:

**Round 1: Core style choices**
- Which of the observed patterns do you want to keep vs. discard?
- Do you want the style to be more formal or more accessible than the examples?
- Any specific habits from the examples you explicitly want to avoid?

**Round 2: Detailed adjustments**
- Preferred sentence length range?
- Active vs. passive voice preference?
- First-person usage (I/we) — allowed, restricted, or preferred?
- Hedging style (strong claims vs. cautious language)?
- Citation density preference?

**Round 3: Scope confirmation**
- Should these rules apply to all writing or specific genres (papers, emails, reports)?
- Any additional constraints (e.g., journal-specific guidelines, word limits)?

### Step 4: Generate PREFERENCE.md

Synthesize the analysis and user feedback into a structured `PREFERENCE.md` (uppercase filename) in the directory root. Format:

```markdown
# Writing Style Preferences

## Overview
[2-3 sentence summary of the target style]

## Dimensions

### 1. Sentence Structure
[Specific rules and parameters]

### 2. Vocabulary Features
[...]

### 3. Paragraph Organization
[...]

[... continue for all 12 dimensions ...]

## Global Rules
- [Rule 1]
- [Rule 2]

## Anti-Patterns (Avoid)
- [Anti-pattern 1]
- [Anti-pattern 2]
```

### Step 5: Rules Generation (SubAgent)

Launch the pre-defined `rules-generator` agent, specifying the directory:

```
Read {directory}/PREFERENCE.md,
convert preferences into actionable rules, and save to .claude/rules/artical-writing-style.md in the project root.
```

### Step 6: Global Application (Optional)

If the user wants global application:

```bash
mkdir -p ~/.claude/rules && cp .claude/rules/artical-writing-style.md ~/.claude/rules/artical-writing-style.md
```

Confirm with the user before executing.

## Resources

| File | Purpose | When to Load |
|------|---------|--------------|
| `references/dimensions.md` | Detailed definitions and analysis criteria for the 12 writing style dimensions | When the style-analyzer SubAgent needs the full dimension descriptions |
| `references/INDEX.md` | Index of example documents with file list, types, word counts, and brief summaries | Generated during Step 1; used by the style-analyzer to understand the document collection |
| `references/PREFERENCE.md` | Structured writing style preferences synthesized from analysis and user feedback | Generated during Step 4; read by the rules-generator in Step 5 |
