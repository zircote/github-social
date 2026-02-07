---
name: github-social:repo-metadata
description: Generate optimized repository description and topics for GitHub
argument-hint: "[--apply]"
allowed-tools: Read, Glob, Grep, Bash, Skill
---

## Memory

Capture after: `/mnemonic:capture patterns "{REPO} repo metadata description and topics"`

Generate an engaging repository description and relevant topics for this GitHub repository.

## Task

Analyze this project and generate optimized GitHub metadata (description and topics) that improves discoverability.

## Process

1. Use the repo-metadata skill to analyze the project
2. Generate an engaging description (max 350 characters)
3. Suggest relevant topics (max 20, lowercase, hyphenated)
4. Output the suggestions for review

## Apply Mode

If `--apply` is provided via $ARGUMENTS, or if user confirms they want to apply:
- Use `gh repo edit --description "..."` to update description
- Use `gh repo edit --add-topic ...` for each topic
- Verify changes with `gh repo view`

## Output Format

Display generated metadata with:
- Character count for description
- Topic count
- Manual update instructions
- CLI commands for applying

