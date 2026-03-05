# Copilot Instructions

You are working in the github-social plugin repository for Claude Code.

## Project Overview

This is a Claude Code plugin to optimize GitHub repository presentation with social preview images and engaging metadata (descriptions, topics).

## Key Components

- **Commands**: 5 commands (repo-metadata, social-setup, social-preview, readme-enhance, social-all)
- **Skills**: 2 skills (repo-metadata, social-preview)

## Plugin Structure

```
.claude-plugin/plugin.json  # Plugin manifest
commands/                   # 3 commands
skills/                     # 2 skill directories with SKILL.md
```

## Development Guidelines

1. Follow Claude Code plugin standards
2. Keep changes focused and reviewable
3. Test commands locally with `claude --plugin-dir .`

## Testing

```bash
claude --plugin-dir .
```

Then test:
- `/github-social:social-preview` command
- `/github-social:repo-metadata` command
- `/github-social:social-setup` command
