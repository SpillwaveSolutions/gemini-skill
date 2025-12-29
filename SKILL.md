---
name: gemini
description: Shell out to Gemini CLI from Claude for any task. Use when asked to "pipe content to Gemini", "get Gemini perspective", "use gemini CLI", "run headless AI task", "parallel analysis with Gemini", or "get second opinion from Gemini". Useful for alternative perspectives, batch processing, or leveraging Gemini 2.5 Pro capabilities alongside Claude.
---

# Gemini CLI

## Table of Contents

- [Overview](#overview)
- [When to Use This Skill](#when-to-use-this-skill)
- [Core Command Pattern](#core-command-pattern)
- [Model Selection](#model-selection)
- [Command Patterns](#command-patterns)
- [Prompt Engineering for Gemini](#prompt-engineering-for-gemini)
- [Common Use Cases](#common-use-cases)
- [Combining Claude + Gemini](#combining-claude--gemini)
- [Advanced Patterns](#advanced-patterns)
- [Error Handling](#error-handling)
- [Quick Reference](#quick-reference)

## Overview

Execute tasks using Google's `gemini` command-line tool to leverage Gemini 2.5 Pro alongside Claude. This skill enables headless (non-interactive) execution where content is piped to Gemini, processed with custom prompts, and results are saved directly to files.

## When to Use This Skill

Invoke this skill when:
- **Alternative perspective**: Get Gemini's viewpoint on a problem Claude is solving
- **Parallel analysis**: Run Claude and Gemini analysis side-by-side for comparison
- **Specific Gemini strengths**: Leverage capabilities where Gemini excels
- **Headless processing**: Need non-interactive AI execution that saves to files
- **Batch processing**: Process multiple inputs through Gemini
- **Second opinion**: Validate Claude's analysis with Gemini's assessment

**When NOT to use**: Tasks Claude can handle directly without needing a second AI perspective.

## Core Command Pattern

The fundamental pattern for using gemini CLI:

```bash
cat [input-files] | gemini "[PROMPT]" --model gemini-2.5-pro > [output-file]
```

**Components**:
1. **`cat [input-files]`** - Concatenate input content (optional if prompt-only)
2. **`| gemini "[PROMPT]"`** - Pipe to gemini with custom instructions
3. **`--model gemini-2.5-pro`** - Specify the model
4. **`> [output-file]`** - Redirect output to a file

## Model Selection

| Model | Use For | Speed |
|-------|---------|-------|
| `gemini-2.5-pro` | Complex reasoning, deep analysis, multi-step tasks | Slower |
| `gemini-2.5-flash` | Quick processing, simple transformations, batch jobs | Faster |

Specify with: `--model gemini-2.5-pro` or `--model gemini-2.5-flash`

## Command Patterns

### Single File Input

```bash
cat document.md | gemini "Summarize in 3 bullet points" --model gemini-2.5-pro > summary.md
```

### Multiple File Input

```bash
cat file1.txt file2.txt | gemini "Find common themes" --model gemini-2.5-pro > themes.md
```

### Prompt-Only (No Input)

```bash
gemini "Create a 10-item code review checklist" --model gemini-2.5-pro > checklist.md
```

## Prompt Engineering for Gemini

### Define Role and Task

```bash
gemini "You are an expert [ROLE]. [TASK]. Format as [FORMAT]." --model gemini-2.5-pro
```

### Multi-Part Instructions

```bash
gemini "Task:
1. [First step]
2. [Second step]
3. [Third step]

Format: Markdown with headings" --model gemini-2.5-pro
```

## Common Use Cases

### Alternative Perspective

Get Gemini's viewpoint alongside Claude's:

```bash
cat analysis.md | gemini "Review this analysis. Identify gaps or alternative viewpoints." --model gemini-2.5-pro > gemini_perspective.md
```

**Verify output**: `head -20 gemini_perspective.md` to confirm format matches expectations.

### Comparative Analysis

Compare two approaches:

```bash
cat approach_a.md approach_b.md | gemini "Compare these approaches. Create pros/cons table, recommend when to use each." --model gemini-2.5-pro > comparison.md
```

**Verify output**: Check comparison.md contains expected table structure.

### Code Review

Review code with Gemini:

```bash
cat code_file.py | gemini "Review for: 1) Design patterns, 2) Potential issues, 3) Security concerns. Provide specific examples." --model gemini-2.5-pro > code_review.md
```

**Sample Output** (code_review.md):
```markdown
## Code Review

### Design Patterns: 7/10
- Uses functional approach appropriately
- Consider adding type hints

### Potential Issues
1. No input validation on line 15
2. Missing error handling for empty lists

### Security Concerns
- None identified for this scope
```

**Verify output**: `grep "##" code_review.md` to confirm sections exist.

### Research Synthesis

Synthesize multiple sources:

```bash
cat source1.md source2.md source3.md | gemini "Synthesize key insights. Create executive summary followed by detailed findings." --model gemini-2.5-pro > synthesis.md
```

### Batch Processing

Process multiple files:

```bash
for file in *.md; do
  cat "$file" | gemini "Summarize this document" --model gemini-2.5-flash > "summaries/${file%.md}_summary.md"
done
```

## Combining Claude + Gemini

### Sequential Processing

Claude processes first, Gemini refines:

1. Claude creates initial analysis, saves to file
2. Gemini reviews and enhances, saves refinement
3. Compare both results

### Parallel Analysis

Both analyze the same input:

1. Claude analyzes in conversation
2. Gemini analyzes same content, saves to file
3. Compare perspectives, synthesize insights

### Division of Labor

- **Claude**: Interactive exploration, code generation
- **Gemini**: Batch processing, alternative perspectives

## Advanced Patterns

### Iterative Refinement

```bash
# First pass
cat input.md | gemini "Analyze..." --model gemini-2.5-pro > v1.md

# Second pass with feedback
cat v1.md | gemini "Review and improve this analysis..." --model gemini-2.5-pro > v2.md
```

### Chaining with CLI Tools

```bash
cat document.md | gemini "Extract key points" --model gemini-2.5-pro | grep "##" > headings.txt
```

### Environment Variables for Reusable Prompts

```bash
PROMPT="Analyze for security vulnerabilities. Format as Markdown."
cat code.py | gemini "$PROMPT" --model gemini-2.5-pro > security_review.md
```

## Error Handling

**If command fails**:
1. Verify `gemini` CLI is installed: `which gemini`
2. Check file paths exist: `ls -la input.md`
3. Ensure prompt is properly quoted
4. Verify model flag: `--model gemini-2.5-pro`

**If output is poor**:
1. Make prompt more specific
2. Add role definition
3. Request structured format
4. Try `gemini-2.5-pro` instead of `flash`

**Quote escaping**:
```bash
gemini 'Say "Hello World"' --model gemini-2.5-pro
```

## Quick Reference

**Prompt Template**:
```
"You are [ROLE].
Task: [WHAT TO DO]
Input: [WHAT YOU'RE ANALYZING]
Output: [DESIRED FORMAT]
Context: [CONSTRAINTS/AUDIENCE]"
```

**Best Practices**:
1. Always specify model with `--model`
2. Define role in prompt
3. Request structured output format
4. Use descriptive output filenames
5. Verify output with `head` or `grep` after generation
