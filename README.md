# Gemini CLI Skill

[![Agent Skill Standard](https://img.shields.io/badge/Agent%20Skill-Standard-blue)](https://agentskills.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![SkillzWave](https://img.shields.io/badge/SkillzWave-Marketplace-purple)](https://skillzwave.ai/skill/SpillwaveSolutions__gemini-skill__gemini__SKILL/)

Shell out to Gemini CLI from Claude for headless AI task execution. This skill enables Claude to leverage Gemini 2.5 Pro alongside its own capabilities for parallel analysis, alternative perspectives, and batch processing.

## Table of Contents

- [Overview](#overview)
- [When to Use](#when-to-use)
- [Quick Start](#quick-start)
- [Core Command Structure](#core-command-structure)
- [Model Selection](#model-selection)
- [Common Use Cases](#common-use-cases)
- [Prompt Engineering](#prompt-engineering)
- [Workflow Patterns](#workflow-patterns)
- [Troubleshooting](#troubleshooting)
- [Installing with Skilz](#installing-with-skilz-universal-installer)
- [Resources](#resources)

## Overview

This skill provides a framework for executing non-interactive Gemini tasks where content is piped to the Gemini CLI, processed with custom prompts, and results are saved directly to files. Use it to combine Claude's and Gemini's strengths for comprehensive analysis and processing.

## When to Use

**Use this skill when you need:**

- Alternative AI perspective on Claude's analysis
- Parallel analysis from both Claude and Gemini
- Gemini's specific strengths for particular tasks
- Headless (non-interactive) AI execution saving to files
- Batch processing of multiple inputs
- Independent validation of Claude's work

**Don't use when:**

- Claude can handle the task directly without needing a second perspective
- Interactive conversation is required (gemini CLI is headless only)

## Quick Start

### Basic Pattern

```bash
cat input.md | gemini "PROMPT" --model gemini-2.5-pro > output.md
```

### Example: Get Alternative Perspective

```bash
# Claude creates analysis in conversation
# Then get Gemini's independent review
cat analysis.md | gemini "Review this analysis and provide your perspective. Identify anything missed or alternative viewpoints." --model gemini-2.5-pro > gemini_perspective.md
```

## Core Command Structure

The fundamental pattern has four components:

1. **Input** (optional): `cat file1.md file2.md` - Concatenate input files
2. **Pipe**: `|` - Send output to next command
3. **Gemini CLI**: `gemini "PROMPT" --model gemini-2.5-pro` - Process with Gemini
4. **Output**: `> output.md` - Save results to file

## Model Selection

### gemini-2.5-pro (Default)

- **Use for**: Complex reasoning, deep analysis, sophisticated multi-step tasks
- **Speed**: Slower, but more capable
- **Best for**: Code reviews, architectural analysis, detailed critiques

### gemini-2.5-flash

- **Use for**: Simple transformations, quick processing, basic analysis
- **Speed**: Much faster
- **Best for**: Batch processing, simple formatting, straightforward summaries

**Specify with**: `--model gemini-2.5-pro` or `--model gemini-2.5-flash`

## Common Use Cases

### 1. Alternative Perspective

Get Gemini's viewpoint on Claude's work:

```bash
cat claude_analysis.md | gemini "Review this analysis. Provide alternative viewpoints and identify gaps." --model gemini-2.5-pro > gemini_view.md
```

### 2. Parallel Analysis

Both AIs analyze the same input:

```bash
# Claude analyzes in conversation, then:
cat document.md | gemini "Analyze this document for key insights and recommendations." --model gemini-2.5-pro > gemini_analysis.md
# Compare both analyses
```

### 3. Code Review

Independent code review:

```bash
cat code.py | gemini "You are a senior Python developer. Review this code for: 1) Design patterns, 2) Potential issues, 3) Security concerns, 4) Optimization opportunities." --model gemini-2.5-pro > code_review.md
```

### 4. Comparative Analysis

Compare multiple approaches:

```bash
cat approach_a.md approach_b.md | gemini "Compare these approaches. Create a table with pros/cons, then recommend when to use each." --model gemini-2.5-pro > comparison.md
```

### 5. Batch Processing

Process multiple files:

```bash
for file in *.md; do
  cat "$file" | gemini "Summarize this document" --model gemini-2.5-flash > "summaries/${file%.md}_summary.md"
done
```

### 6. Research Synthesis

Synthesize multiple sources:

```bash
cat source1.md source2.md source3.md | gemini "Synthesize key insights from these sources. Create an executive summary followed by detailed findings." --model gemini-2.5-pro > synthesis.md
```

## Prompt Engineering

### Effective Prompt Template

```
"You are [ROLE].

Task: [WHAT TO DO]

Input: [WHAT YOU'RE ANALYZING]

Output: [DESIRED FORMAT]

Context: [CONSTRAINTS/AUDIENCE]"
```

### Best Practices

1. **Define the role**: "You are an expert technical writer..."
2. **Specify output format**: "Format as Markdown with clear headings"
3. **Break into steps**: Multi-part instructions for complex tasks
4. **Set constraints**: Audience, focus areas, length limits

### Example Prompt

```bash
gemini "You are an expert editor for top-tier technical publications.

Task: Analyze the provided article using the rubric that follows.

Output: Comprehensive review in Markdown format with scores and specific, actionable feedback for each criterion.

Context: This is for a developer audience seeking advanced technical insights." --model gemini-2.5-pro
```

## Workflow Patterns

### Sequential Processing

1. Claude creates initial analysis and saves to file
2. Gemini reviews and enhances, saves refinement
3. Compare both results

### Parallel Analysis

1. Claude analyzes in conversation
2. Gemini analyzes same content and saves to file
3. Compare perspectives, synthesize insights

### Division of Labor

1. Claude: Interactive exploration, code generation
2. Gemini: Batch processing, alternative perspectives
3. Combine results for complete solution

### Iterative Refinement

```bash
# First pass
cat input.md | gemini "Analyze..." --model gemini-2.5-pro > v1.md

# Second pass with feedback
cat v1.md | gemini "Review and improve this analysis. Focus on..." --model gemini-2.5-pro > v2.md
```

## File Organization

Recommended structure when using this skill:

```
work/
├── gemini_output/      # Gemini-generated files
│   ├── analysis.md
│   ├── review.md
│   └── synthesis.md
├── claude_output/      # Claude-generated files
└── combined/           # Synthesized results
```

## Troubleshooting

### Command Fails

1. Verify gemini CLI is installed: `which gemini`
2. Check file paths exist: `ls -la input.md`
3. Ensure prompt is properly quoted (use `"..."`)
4. Verify model flag: `--model gemini-2.5-pro`

### Poor Output Quality

1. Make prompt more specific
2. Add role definition
3. Request structured format
4. Try `gemini-2.5-pro` instead of `flash`

### Quote Escaping

Use single quotes for outer prompt if inner quotes needed:

```bash
gemini 'Say "Hello World"' --model gemini-2.5-pro
```

## Quick Reference

**Basic command**:

```bash
cat input.md | gemini "PROMPT" --model gemini-2.5-pro > output.md
```

**Multiple inputs**:

```bash
cat file1.md file2.md file3.md | gemini "PROMPT" --model gemini-2.5-pro > output.md
```

**Prompt-only (no input file)**:

```bash
gemini "Generate a checklist for..." --model gemini-2.5-pro > checklist.md
```

**Best practices checklist**:

- Always specify model with `--model`
- Define role in prompt
- Request structured output format
- Use descriptive output filenames
- Save Gemini output to dedicated directory

## Installing with Skilz (Universal Installer)

The recommended way to install this skill across different AI coding agents is using the **skilz** universal installer.

### Install Skilz

```bash
pip install skilz
```

This skill supports [Agent Skill Standard](https://agentskills.io/) which means it supports 14 plus coding agents including Claude Code, OpenAI Codex, Cursor and Gemini.

### Git URL Options

```bash
# HTTPS URL
skilz install -g https://github.com/SpillwaveSolutions/gemini-skill

# SSH URL
skilz install --git git@github.com:SpillwaveSolutions/gemini-skill.git
```

### Claude Code

```bash
skilz install -g https://github.com/SpillwaveSolutions/gemini-skill
skilz install -g https://github.com/SpillwaveSolutions/gemini-skill --project
```

### OpenCode

```bash
skilz install -g https://github.com/SpillwaveSolutions/gemini-skill --agent opencode
skilz install -g https://github.com/SpillwaveSolutions/gemini-skill --project --agent opencode
```

### Gemini

```bash
skilz install -g https://github.com/SpillwaveSolutions/gemini-skill --agent gemini
```

### OpenAI Codex

```bash
skilz install -g https://github.com/SpillwaveSolutions/gemini-skill --agent codex
skilz install -g https://github.com/SpillwaveSolutions/gemini-skill --project --agent codex
```

### Install from Skillzwave Marketplace

```bash
skilz install SpillwaveSolutions_gemini-skill/gemini
skilz install SpillwaveSolutions_gemini-skill/gemini --project
skilz install SpillwaveSolutions_gemini-skill/gemini --agent opencode
skilz install SpillwaveSolutions_gemini-skill/gemini --agent gemini
```

See [skill Listing](https://skillzwave.ai/skill/SpillwaveSolutions__gemini-skill__gemini__SKILL/) for all 14+ agent install options.

### Other Supported Agents

Skilz supports 14+ coding agents including Claude Code, OpenAI Codex, OpenCode, Cursor, Gemini CLI, GitHub Copilot CLI, Windsurf, Qwen Code, Aidr, and more.

For the full list of supported platforms, visit [SkillzWave.ai/platforms](https://skillzwave.ai/platforms/) or see the [skilz-cli GitHub repository](https://github.com/SpillwaveSolutions/skilz-cli)

<a href="https://skillzwave.ai/">Largest Agentic Marketplace for AI Agent Skills</a> and
<a href="https://spillwave.com/">SpillWave: Leaders in AI Agent Development.</a>

## Resources

### SKILL.md

Complete skill documentation including:

- Detailed use cases
- Advanced patterns
- Prompt engineering techniques
- Error handling

## Prerequisites

This skill requires the `gemini` CLI to be installed and configured. Ensure you have:

- Google's gemini CLI installed
- Proper authentication configured
- Access to gemini-2.5-pro and gemini-2.5-flash models

## Example: Complete Workflow

**Scenario**: Claude writes a Python function, get Gemini's independent review.

**Step 1 - Claude's work** (in conversation):

```python
def process_data(items):
    """Process items and return summary."""
    return sum(len(x) for x in items)
```

**Step 2 - Save to file** (Claude writes):

```bash
# Claude saves function to function.py
```

**Step 3 - Get Gemini's perspective**:

```bash
cat function.py | gemini "You are a senior Python developer. Review this function for: 1) Code quality, 2) Potential issues, 3) Improvements. Be specific and provide examples." --model gemini-2.5-pro > gemini_review.md
```

**Step 4 - Compare and Synthesize**:

- Review Gemini's feedback in `gemini_review.md`
- Compare with Claude's implementation
- Synthesize best approach combining both perspectives

---

**Use this skill strategically** to complement Claude's capabilities with Gemini's independent analysis, enabling richer insights through multi-AI collaboration.
