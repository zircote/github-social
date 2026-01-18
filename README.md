# github-social

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-Plugin-blueviolet)](https://claude.ai/claude-code)
[![Version](https://img.shields.io/badge/version-0.2.0-green.svg)](.claude-plugin/plugin.json)
[![GitHub Stars](https://img.shields.io/github/stars/zircote/github-social?style=social)](https://github.com/zircote/github-social)
[![GitHub Issues](https://img.shields.io/github/issues/zircote/github-social)](https://github.com/zircote/github-social/issues)
[![Last Commit](https://img.shields.io/github/last-commit/zircote/github-social)](https://github.com/zircote/github-social/commits)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/zircote/github-social/pulls)

A Claude Code plugin that optimizes GitHub repository presentation by analyzing project intent and purpose.

## Features

### Social Preview Images
- Analyzes project files (README, package.json, CLAUDE.md, etc.) to understand purpose
- Generates detailed image prompts capturing the project's spirit
- Supports multiple image generation providers (DALL-E, Stable Diffusion, etc.)
- Outputs images meeting GitHub's social preview requirements (1280x640px, <1MB)

### Repository Metadata
- Generates engaging, SEO-friendly repository descriptions (max 350 chars)
- Suggests relevant topics/labels for discoverability (max 20 topics)
- Can apply changes directly via `gh` CLI or output for manual review
- Works with zero configuration

## Installation

```bash
# Install via Claude Code plugin directory
claude --plugin-dir /path/to/github-social
```

## Usage

### Quick Start (No Configuration)

Simply invoke the skill by asking Claude:

```
Generate a social preview image for this project
```

Or use the command:

```
/social-preview
```

Without configuration, the plugin outputs a detailed image prompt you can use with your preferred image generation tool.

### With Configuration (Optional)

Run the setup command to configure your preferences:

```
/social-preview:setup
```

This creates `.claude/github-social.local.md` with your settings.

## Configuration Options

Create `.claude/github-social.local.md` with YAML frontmatter:

```yaml
---
provider: dalle-3        # dalle-3 | stable-diffusion | midjourney | manual
api_key_env: OPENAI_API_KEY  # Environment variable name (NOT the key itself)
style: abstract          # abstract | illustrated | minimalist | custom
output_path: .github/social-preview.png
dimensions: 1280x640     # Recommended for GitHub
include_text: true       # Include project name in image
---

# Optional: Additional brand guidelines or context
```

### Supported Providers

| Provider | Requirements | Notes |
|----------|-------------|-------|
| `dalle-3` | `OPENAI_API_KEY` env var | Best quality, requires OpenAI API |
| `stable-diffusion` | Local SD installation or API | Flexible, various backends |
| `midjourney` | Manual process | Plugin outputs optimized prompt |
| `manual` | None | Outputs prompt only |

### Style Options

- **abstract**: Clean geometric shapes, gradients, tech-inspired patterns
- **illustrated**: Hand-drawn style, artistic representations
- **minimalist**: Simple design with project name and subtle elements
- **custom**: Specify your own style in `custom_style` field

## GitHub Requirements

Social preview images must meet these requirements:
- **Minimum size**: 640x320 pixels
- **Recommended size**: 1280x640 pixels (2:1 aspect ratio)
- **Maximum file size**: 1MB
- **Supported formats**: PNG, JPG, GIF

## Example Output

The plugin generates images that:
1. Capture the project's purpose and spirit
2. Use appropriate visual metaphors for the technology/domain
3. Include readable project name (optional)
4. Meet all GitHub size requirements

## Skills

### social-preview

Analyzes your project and generates social preview images. Triggered by:
- "generate social preview"
- "create github social image"
- "repository preview image"
- "og image for this project"

### repo-metadata

Generates optimized repository descriptions and topics. Triggered by:
- "update repo description"
- "improve repository description"
- "generate topics"
- "add labels to repo"
- "optimize github metadata"
- "make repo more discoverable"

## Commands

| Command | Description |
|---------|-------------|
| `/social-preview` | Generate a social preview image for the current project |
| `/social-preview:setup` | Interactive setup wizard for configuration |
| `/repo-metadata` | Generate optimized description and topics (use `--apply` to update GitHub) |

## License

MIT
