# github-social

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-Plugin-blueviolet?style=for-the-badge&logo=anthropic&logoColor=white)](https://claude.ai/code)
[![Version](https://img.shields.io/badge/version-0.6.0-green.svg?style=for-the-badge)](.claude-plugin/plugin.json)
[![GitHub Stars](https://img.shields.io/github/stars/zircote/github-social?style=for-the-badge&logo=github)](https://github.com/zircote/github-social/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/zircote/github-social?style=for-the-badge&logo=github)](https://github.com/zircote/github-social/issues)
[![Last Commit](https://img.shields.io/github/last-commit/zircote/github-social?style=for-the-badge&logo=github)](https://github.com/zircote/github-social/commits)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](https://github.com/zircote/github-social/pulls)

<p align="center">
  <img src=".github/social-preview.png" alt="github-social - Claude Code Plugin for GitHub Repository Optimization" width="800">
</p>

A Claude Code plugin that optimizes GitHub repository presentation by analyzing project intent and purpose.

## Features

### Social Preview Images
- Analyzes project files (README, package.json, CLAUDE.md, etc.) to understand purpose
- **SVG generation by default** - Free, instant, editable vector graphics
- Supports multiple providers: SVG (default), DALL-E 3, Gemini, or manual prompts
- Dark mode support with automatic theme switching
- Outputs images meeting GitHub's social preview requirements (1280x640px, <1MB)

### Repository Metadata
- Generates engaging, SEO-friendly repository descriptions (max 350 chars)
- Suggests relevant topics/labels for discoverability (max 20 topics)
- Can apply changes directly via `gh` CLI or output for manual review
- Works with zero configuration

### README Enhancement
- Generates marketing badges (shields.io) based on project analysis
- Creates NotebookLM-style infographic prompts capturing architecture and features
- Supports multiple badge types: version, build, license, downloads, language, and more
- Hybrid infographic style adapts to project type (CLI, library, framework, app)
- Direct README.md modification with intelligent badge section management

### Citation Management
- Creates, validates, and manages CITATION.cff files for academic citation of software
- Auto-detects project metadata from any ecosystem (Node.js, Python, Rust, Go, etc.)
- Syncs version, authors, and keywords from project sources
- Exports citations as BibTeX, APA, RIS, CodeMeta, or EndNote
- Enriches with ORCID iDs, DOIs, and preferred-citation for published papers
- Generates GitHub Actions workflow for CI validation

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

Without configuration, the plugin generates a clean SVG image directly. Use `--provider=manual` to output a text prompt instead.

### With Configuration (Optional)

Run the setup command to configure your preferences:

```
/github-social:social-setup
```

This creates `.claude/github-social.local.md` with your settings.

## Configuration Options

Create `.claude/github-social.local.md` with YAML frontmatter:

```yaml
---
provider: svg            # svg (default) | dalle-3 | gemini | manual
svg_style: minimal       # minimal | geometric | illustrated (for SVG provider)
dark_mode: false         # false | true | both (generate both variants)
api_key_env: OPENAI_API_KEY  # For dalle-3; use GEMINI_API_KEY for gemini
output_path: .github/social-preview.svg
dimensions: 1280x640     # Recommended for GitHub
include_text: true       # Include project name in image
upload_to_repo: false    # Auto-upload to GitHub repository
---

# Optional: Additional brand guidelines or context
```

### Supported Providers

| Provider | Requirements | Cost | Notes |
|----------|-------------|------|-------|
| `svg` (default) | None | Free | Claude generates clean SVG graphics directly |
| `dalle-3` | `OPENAI_API_KEY` env var | ~$0.08/image | Artistic, creative AI images |
| `gemini` | `GEMINI_API_KEY` env var | ~$0.039/image | Google's image generation |
| `manual` | None | Free | Outputs optimized prompt only |

### SVG Style Options

- **minimal** (default): Clean design with 3-5 shapes, generous whitespace
- **geometric**: Complex arrangements with 8-15 shapes representing domain metaphors
- **illustrated**: Organic paths with hand-drawn aesthetic and warm colors

## GitHub Requirements

Social preview images must meet these requirements:
- **Minimum size**: 640x320 pixels
- **Recommended size**: 1280x640 pixels (2:1 aspect ratio)
- **Maximum file size**: 1MB
- **Supported formats**: SVG, PNG, JPG, GIF

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

### readme-enhance

Enhances README with badges and infographics. Triggered by:
- "enhance readme"
- "add badges to readme"
- "create readme infographic"
- "improve readme marketing"
- "add shields.io badges"
- "generate project infographic"

### citation-cff

Creates, validates, and exports CITATION.cff files. Triggered by:
- "create citation file"
- "generate CITATION.cff"
- "validate citation"
- "export citation as bibtex"
- "sync citation version"
- "enrich citation metadata"

## Commands

| Command | Description |
|---------|-------------|
| `/social-preview` | Generate a social preview image for the current project |
| `/github-social:social-setup` | Interactive setup wizard for configuration |
| `/repo-metadata` | Generate optimized description and topics (use `--apply` to update GitHub) |
| `/readme-enhance` | Add marketing badges and infographic to README |
| `/citation-cff` | Create, validate, update, and export CITATION.cff files |
| `/github-social:social-all` | Run all skills in sequence (metadata, social preview, README enhancement) |

## License

MIT
