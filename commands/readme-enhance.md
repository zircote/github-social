---
name: github-social:readme-enhance
description: Enhance README.md with marketing badges and a NotebookLM-style infographic
argument-hint: "[--provider svg|dalle-3|gemini|manual] [--badges-only] [--infographic-only] [--dark-mode] [--dry-run]"
allowed-tools: ["Read", "Write", "Edit", "Glob", "Grep", "Bash", "mcp__github__create_or_update_file", "Skill"]
---

## Memory

Capture after: `/mnemonic:capture patterns "{REPO} README enhancement badges and infographic"`

# README Enhancement Command

Enhance the current project's README.md with marketing badges (shields.io) and a NotebookLM-style infographic that captures project architecture, features, and purpose.

## Execution Steps

### 1. Parse Arguments

Handle optional flags:
- `--provider [value]`: Override image generation provider (svg, dalle-3, gemini, manual)
- `--badges-only`: Generate only badges, skip infographic
- `--infographic-only`: Generate only infographic, skip badges
- `--dark-mode`: Generate dark mode variant of infographic
- `--dry-run`: Preview changes without modifying README.md

### 2. Load Configuration

Check for `.claude/github-social.local.md` and parse settings:
- `provider`: svg (default), dalle-3, gemini, manual
- `badge_style`: for-the-badge (default), flat, flat-square, plastic
- `infographic_style`: hybrid (default), architecture, features
- `infographic_output`: .github/readme-infographic.svg (default)
- `dark_mode`: false (default), true, both
- `update_readme`: true (default)

Command-line flags override configuration settings.

### 3. Analyze Project

Load the `readme-enhance` skill and follow its analysis workflow:
1. Read README.md, package manifest, CLAUDE.md
2. Detect repository owner/name from git remote
3. Extract project purpose, features, technology stack
4. Identify CI workflow files for build badges
5. Detect license type
6. Determine project type (CLI, library, framework, app, infrastructure, plugin)

### 4. Generate Badges

Based on project analysis, generate appropriate shields.io badges:

**Always include** (if detectable):
- Version badge (npm/crates/pypi/GitHub release)
- License badge
- Build/CI status badge
- Primary language badge

**Include for engagement**:
- GitHub stars
- Downloads (if published)
- Last commit
- PRs welcome

**Context-specific**:
- Claude Code Plugin badge (if plugin.json exists)
- Coverage badge (if codecov/coveralls configured)
- Documentation badge (if docs site exists)

### 5. Generate Infographic

Based on provider setting:

**For SVG (default)**:
1. Determine layout based on project type
2. Generate clean SVG following infographic patterns
3. Apply domain-appropriate color palette
4. Save to `infographic_output` path
5. If `dark_mode: both`, generate dark variant

**For DALL-E 3 / Gemini / Manual**:
1. Create detailed NotebookLM-style prompt
2. If provider configured with API key, generate image
3. Otherwise output prompt for manual use

### 6. Update README.md

Unless `--dry-run` is specified:

**Badge insertion**:
1. Find existing badge section (after H1, lines starting with `[![`)
2. Replace existing badges or insert new section
3. Preserve any custom badges not matching generated patterns

**Infographic insertion with prompt display**:
1. Add centered image after badges
2. Add collapsible `<details>` section with generation prompt
3. Follow documentation-review pattern:

```markdown
<!-- Infographic: Visual summary of project -->
<p align="center">
  <img src=".github/readme-infographic.svg" alt="Project Overview" width="800">
</p>

<details>
<summary>View infographic generation prompt</summary>

```
[Generation prompt or SVG description here]
```

</details>
```

**Dark mode support** (if `dark_mode: both`):
```markdown
<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/readme-infographic-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset=".github/readme-infographic.svg">
    <img src=".github/readme-infographic.svg" alt="Project Overview" width="800">
  </picture>
</p>
```

### 7. Output Results

Display:
1. Summary of badges generated
2. Infographic file location (or prompt if manual)
3. Changes made to README.md (or preview if `--dry-run`)
4. Next steps if applicable

## Provider Comparison

| Provider | Output | Cost | Speed | Best For |
|----------|--------|------|-------|----------|
| **svg** (default) | SVG file | Free | Instant | Clean, professional |
| **dalle-3** | PNG image | ~$0.08 | 5-15s | Artistic variety |
| **gemini** | PNG image | ~$0.039 | 3-10s | Good quality |
| **manual** | Text prompt | Free | Instant | Custom tools |

## Example Usage

```bash
# Full enhancement with SVG infographic (recommended)
/readme-enhance

# Preview without changes
/readme-enhance --dry-run

# Only add/update badges
/readme-enhance --badges-only

# Only generate infographic
/readme-enhance --infographic-only

# Generate with dark mode support
/readme-enhance --dark-mode

# Use DALL-E for artistic infographic
/readme-enhance --provider=dalle-3

# Use Gemini for image generation
/readme-enhance --provider=gemini

# Get prompt only for manual generation
/readme-enhance --provider=manual --infographic-only
```

## Tips

- Run `/github-social:all` to run all enhancement commands in sequence
- SVG infographics are free, instant, and editable
- The generation prompt is always displayed in the README for transparency
- Badges are optimized for the `for-the-badge` style by default
- Custom badges in your README are preserved during updates
- Use `--dark-mode` for repositories used in both light and dark GitHub themes

