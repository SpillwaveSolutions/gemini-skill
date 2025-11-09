---
name: gemini
description: Shell out to Gemini CLI from Claude for any task. Use this skill when you need to leverage Gemini 2.5 Pro's capabilities alongside Claude, pipe content to Gemini for processing, or execute headless (non-interactive) AI tasks that save results to files. Useful for getting alternative perspectives, parallel analysis, or leveraging Gemini's specific strengths.
---

# Gemini CLI

## Overview

Execute tasks using Google's `gemini` command-line tool to leverage Gemini 2.5 Pro alongside Claude. This skill enables "headless" (non-interactive) execution where content is piped to Gemini, processed with custom prompts, and results are saved directly to files.

Use this skill to combine Claude's and Gemini's strengths, get alternative perspectives, or run parallel analysis tasks.

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
3. **`--model gemini-2.5-pro`** - Specify the model (Pro for complex tasks, Flash for simple)
4. **`> [output-file]`** - Redirect output to a file

**Pipe Operator (`|`)**: Takes output from left side and sends as input to right side.

**Output Redirection (`>`)**: Writes command output to a file instead of displaying on screen.

## Model Selection

**gemini-2.5-pro**: Complex reasoning, analysis, sophisticated tasks
- Use for: deep analysis, complex reasoning, multi-step tasks
- Speed: Slower, but more capable
- Default choice for most tasks in this skill

**gemini-2.5-flash**: Simpler tasks, faster responses
- Use for: quick processing, simple transformations, basic analysis
- Speed: Much faster
- Good for: batch processing, simple formatting

**Specify with**: `--model gemini-2.5-pro` or `--model gemini-2.5-flash`

## Command Patterns

### 1. Single File Input

Process one file:
```bash
cat document.md | gemini "Summarize this document in 3 bullet points" --model gemini-2.5-pro > summary.md
```

### 2. Multiple File Input (Concatenated)

Process multiple files together:
```bash
cat file1.txt file2.txt file3.txt | gemini "Find common themes across these documents" --model gemini-2.5-pro > themes.md
```

### 3. Prompt-Only (No Input File)

Generate from scratch:
```bash
gemini "Create a checklist for code review best practices. Include 10 items." --model gemini-2.5-pro > checklist.md
```

### 4. With Context Ordering

Order matters when concatenating:
```bash
cat main_content.md reference_material.md | gemini "Analyze main content using reference..." --model gemini-2.5-pro > analysis.md
```

## Prompt Engineering for Gemini

### 1. Define the Role

Start by defining Gemini's role:

```bash
gemini "You are an expert [ROLE]. Your task is to [TASK]..." --model gemini-2.5-pro
```

**Examples**:
- "You are an expert technical writer..."
- "You are a senior software architect..."
- "You are a data analyst specializing in..."

### 2. Specify Output Format

Always request structured output:

```bash
gemini "... Format the output as Markdown with clear headings." --model gemini-2.5-pro
```

**Format options**:
- "Format as Markdown with headings"
- "Provide a numbered list"
- "Create a table with columns: X, Y, Z"
- "Output as JSON"
- "Use bullet points"

### 3. Multi-Part Instructions

Break complex tasks into steps:

```bash
gemini "Your task is to:
1. [First step]
2. [Second step]
3. [Third step]

Format: [desired format]" --model gemini-2.5-pro
```

### 4. Set Context and Constraints

Specify important constraints:

```bash
gemini "Analyze this for [audience]. Focus on [aspect]. Limit to [constraint]." --model gemini-2.5-pro
```

**Examples**:
- "Consider beginner programmers"
- "Focus on security implications"
- "Limit to 500 words"

## Common Use Cases

### Use Case 1: Alternative Perspective

Get Gemini's viewpoint alongside Claude's:

```bash
# Claude analyzes in conversation
# Then get Gemini's perspective
cat analysis.md | gemini "Review this analysis and provide your perspective. Identify anything missed or alternative viewpoints." --model gemini-2.5-pro > gemini_perspective.md
```

### Use Case 2: Comparative Analysis

Compare two approaches:

```bash
cat approach_a.md approach_b.md | gemini "Compare these approaches. Create a table with pros/cons for each, then recommend when to use each." --model gemini-2.5-pro > comparison.md
```

### Use Case 3: Content Review

Review and critique content:

```bash
cat document.md criteria.md | gemini "Review the document against the criteria. Provide scores and specific feedback." --model gemini-2.5-pro > review.md
```

### Use Case 4: Research Synthesis

Synthesize information from multiple sources:

```bash
cat source1.md source2.md source3.md | gemini "Synthesize key insights from these sources. Create an executive summary followed by detailed findings." --model gemini-2.5-pro > synthesis.md
```

### Use Case 5: Data Analysis

Analyze data or logs:

```bash
cat logs.txt | gemini "Analyze these logs for errors and patterns. Categorize by severity and provide recommendations." --model gemini-2.5-pro > log_analysis.md
```

### Use Case 6: Code Review

Review code:

```bash
cat code_file.py | gemini "Review this code for: 1) Design patterns, 2) Potential issues, 3) Security concerns, 4) Optimization opportunities. Provide specific examples." --model gemini-2.5-pro > code_review.md
```

### Use Case 7: Batch Processing

Process multiple files:

```bash
for file in *.md; do
  cat "$file" | gemini "Summarize this document" --model gemini-2.5-flash > "summaries/${file%.md}_summary.md"
done
```

## Combining Claude + Gemini

### Pattern 1: Sequential Processing

Claude processes first, Gemini refines:

```
1. Claude creates initial analysis → saves to file
2. Gemini reviews and enhances → saves refinement
3. Compare both results
```

### Pattern 2: Parallel Analysis

Both analyze the same input:

```
1. Claude analyzes in conversation
2. Gemini analyzes same content → saves to file
3. Compare perspectives, synthesize insights
```

### Pattern 3: Division of Labor

Each handles what they do best:

```
1. Claude: Interactive exploration, code generation
2. Gemini: Batch processing, alternative perspectives
3. Combine results for complete solution
```

## Advanced Patterns

### Iterative Refinement

Process, review, refine:

```bash
# First pass
cat input.md | gemini "Analyze..." --model gemini-2.5-pro > v1.md

# Second pass with feedback
cat v1.md | gemini "Review and improve this analysis..." --model gemini-2.5-pro > v2.md
```

### Chaining with CLI Tools

Combine with grep, awk, etc.:

```bash
# Extract specific sections
cat document.md | gemini "..." --model gemini-2.5-pro | grep "##" > headings.txt

# Count results
cat data.txt | gemini "..." --model gemini-2.5-pro | wc -l
```

### Environment Variables

Use variables for reusable prompts:

```bash
PROMPT="Analyze for security vulnerabilities. Format as Markdown."
cat code.py | gemini "$PROMPT" --model gemini-2.5-pro > security_review.md
```

## File Organization

**Recommended structure**:
```
work/
  ├── gemini_output/      # Gemini-generated files
  │   ├── analysis.md
  │   ├── review.md
  │   └── synthesis.md
  ├── claude_output/      # Claude-generated files
  └── combined/           # Synthesized results
```

## Error Handling

**If command fails**:
1. Verify `gemini` CLI is installed: `which gemini`
2. Check file paths exist: `ls -la input.md`
3. Ensure prompt is properly quoted (use `"..."` for prompts)
4. Verify model flag: `--model gemini-2.5-pro`

**If output is poor**:
1. Make prompt more specific
2. Add role definition
3. Request structured format
4. Try `gemini-2.5-pro` instead of `flash`

**Quote escaping**:
- Use single quotes for outer prompt if inner quotes needed:
```bash
gemini 'Say "Hello World"' --model gemini-2.5-pro
```

## Quick Reference

**Basic Pattern**:
```bash
cat input.md | gemini "PROMPT" --model gemini-2.5-pro > output.md
```

**Prompt Template**:
```
"You are [ROLE].

Task: [WHAT TO DO]

Input: [WHAT YOU'RE ANALYZING]

Output: [DESIRED FORMAT]

Context: [CONSTRAINTS/AUDIENCE]"
```

**Model Choice**:
- Complex task → `--model gemini-2.5-pro`
- Simple/fast task → `--model gemini-2.5-flash`

**Best Practices**:
1. ✅ Always specify model with `--model`
2. ✅ Define role in prompt
3. ✅ Request structured output format
4. ✅ Use descriptive output filenames
5. ✅ Save Gemini output to dedicated directory

## Resources

### references/call_gemini.md

Detailed guide to gemini CLI mechanics, including:
- Complete command breakdown
- Pipe operator (`|`) explanation
- Output redirection (`>`) details
- Troubleshooting tips
- Workflow examples

Load this reference for deeper understanding of command-line concepts.

## Example: Getting Alternative Perspective

**Scenario**: Claude helped write a function, now get Gemini's review.

**Claude's work** (in conversation):
```python
def process_data(items):
    """Process items and return summary."""
    return sum(len(x) for x in items)
```

**Get Gemini's perspective**:
```bash
cat function.py | gemini "You are a senior Python developer. Review this function for: 1) Code quality, 2) Potential issues, 3) Improvements. Be specific." --model gemini-2.5-pro > gemini_review.md
```

**Result**: Gemini's independent review saved to `gemini_review.md` for comparison.

**Next**: Compare Claude's and Gemini's perspectives, synthesize best approach.
