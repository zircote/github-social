---
name: README Enhancement Generator
description: This skill should be used when the user asks to "enhance readme", "add badges to readme", "create readme infographic", "improve readme marketing", "add shields.io badges", "create architecture diagram", "generate project infographic", "make readme more engaging", "add project badges", "create visual overview", or needs to improve README.md with marketing badges and visual infographics that capture project architecture, features, and purpose.
---

# README Enhancement Generator

Enhance GitHub repository README files with marketing badges and NotebookLM-style infographics that capture project architecture, features, technology, and purpose. Create engaging visual elements that attract users and communicate project value.

## Overview

This skill analyzes a project's codebase, documentation, and configuration to generate:
1. Marketing badges (shields.io format) for project metrics and status
2. A visual infographic (SVG by default, or AI-generated image prompt)
3. Updated README.md with badges, infographic, and generation prompt display

## Workflow

### Step 1: Check for Configuration

Look for optional settings file at `.claude/github-social.local.md`.

Parse YAML frontmatter for readme-enhance settings:
- `provider`: Image generation provider (svg, dalle-3, gemini, manual) - **default: svg**
- `badge_style`: Badge style (flat, flat-square, plastic, for-the-badge) - **default: for-the-badge**
- `badge_color`: Default badge color
- `infographic_style`: architecture | features | hybrid - **default: hybrid**
- `infographic_output`: Output path for infographic - **default: .github/readme-infographic.svg**
- `dark_mode`: Dark mode support (false, true, both) - **default: false**
- `update_readme`: Whether to modify README.md directly - **default: true**

**Command-line overrides** (highest priority):
- `--provider=svg|dalle-3|gemini|manual`
- `--badges-only` (skip infographic)
- `--infographic-only` (skip badges)
- `--dry-run` (preview without changes)

If no config exists, use defaults: `provider: svg`, `badge_style: for-the-badge`, `infographic_style: hybrid`.

### Step 2: Analyze Project

Gather project context by reading available files:

**Primary sources**:
1. `README.md` - Current content, existing badges
2. `package.json` / `Cargo.toml` / `pyproject.toml` / `go.mod` - Name, version, description
3. `CLAUDE.md` - Project context and guidelines
4. `.github/workflows/` - CI/CD configuration
5. `LICENSE` - License type

**Extract these elements**:
- **Project name**: Official name from manifest
- **Version**: Current version number
- **License**: License type (MIT, Apache-2.0, etc.)
- **Primary language**: From file extensions or manifest
- **Package registry**: npm, crates.io, PyPI, Go modules
- **CI system**: GitHub Actions, CircleCI, Travis
- **Purpose**: What problem does it solve?
- **Architecture**: Key components and data flow
- **Features**: Top 3-5 capabilities
- **Technology stack**: Languages, frameworks, tools

### Step 3: Generate Badge Set

Create shields.io badges based on project analysis.

**Core Badges** (always include if applicable):

| Badge | Condition | Format |
|-------|-----------|--------|
| Version | Has package manifest | `![Version](https://img.shields.io/...)` |
| Build | Has CI workflow | GitHub Actions status badge |
| License | Has LICENSE file | License badge |
| Language | Detected primary | Language badge |

**Engagement Badges** (include for open source):
- GitHub Stars
- GitHub Issues
- Last Commit
- PRs Welcome

**Plugin-Specific Badges**:
- Claude Code Plugin badge (for Claude plugins)
- MCP Server badge (for MCP integrations)

**Context-Dependent Badges** (include if detected):
- Coverage (if test coverage configured)
- Documentation (if docs site exists)
- Downloads (if published to registry)
- Discord/Slack (if community links found)
- Sponsors (if FUNDING.yml exists)

**Badge Generation Rules**:
1. Detect repository owner and name from git remote
2. Use consistent style across all badges
3. Order badges by importance: status > version > metrics > engagement
4. Limit to 8-12 badges to avoid clutter

See `references/badge-catalog.md` for comprehensive badge patterns.

### Step 4: Generate Infographic

Route to appropriate generation method based on provider setting.

---

#### Step 4a: Generate SVG Infographic (Default)

When `provider: svg` or not configured, generate a clean SVG infographic directly.

**Determine Layout** based on project type:

| Project Type | Detection | Layout Weight |
|--------------|-----------|---------------|
| CLI Tool | Has bin field, command patterns | Feature-focused (70%) |
| Library/SDK | Exports APIs, no UI | Balanced (50/50) |
| Framework | Plugin system, lifecycle hooks | Architecture-focused (60%) |
| Application | Has UI, entry points | User flow + features |
| Infrastructure | Docker, K8s, cloud configs | Architecture-heavy (70%) |

**SVG Infographic Structure** (1280x640):

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1280 640">
  <defs>
    <linearGradient id="bg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:[domain-color-1]"/>
      <stop offset="100%" style="stop-color:[domain-color-2]"/>
    </linearGradient>
  </defs>

  <!-- Background -->
  <rect width="1280" height="640" fill="url(#bg)"/>

  <!-- Header Panel (top 15%) -->
  <text x="640" y="60" font-family="..." font-size="36" font-weight="700"
        text-anchor="middle" fill="#f8fafc">PROJECT-NAME</text>
  <text x="640" y="90" font-family="..." font-size="18"
        text-anchor="middle" fill="#94a3b8">Tagline here</text>

  <!-- Main Content Area (middle 70%) -->
  <!-- Architecture section OR Features section OR Hybrid -->

  <!-- Footer Strip (bottom 15%) -->
  <text x="640" y="600" font-family="..." font-size="14"
        text-anchor="middle" fill="#64748b">
    Install: npm install project-name | docs.example.com
  </text>
</svg>
```

**Infographic Sections**:

1. **Header Panel** (top 15%)
   - Project name with stylized typography
   - One-line tagline capturing essence
   - Optional: key metric badge (stars, downloads)

2. **Architecture Panel** (weight varies)
   - Component boxes with connections
   - Data flow arrows
   - Layer visualization

3. **Features Panel** (weight varies)
   - 3-5 capabilities as icons + labels
   - Benefit statements
   - Visual hierarchy

4. **Technology Strip** (optional)
   - Language/framework icons as simple shapes
   - Stack visualization

5. **Footer Strip** (bottom 15%)
   - Install command
   - Links hint

See `references/svg-infographic-patterns.md` for complete layout templates.

---

#### Step 4b: Generate AI Image Prompt

When `provider: dalle-3`, `gemini`, or `manual`:

Craft a detailed NotebookLM-style infographic prompt:

**Prompt Structure**:
```
NotebookLM-style infographic for [project-name], [style] layout,
[Header: project name, tagline],
[Architecture section: component visualization],
[Features section: capability icons and labels],
[Technology section: stack badges],
[Color palette based on domain],
clean modern design, professional typography,
1280x640 pixels for GitHub README display

Negative: cluttered, low contrast, hard to read text, realistic photos
```

For `dalle-3` or `gemini`, call the respective API (see social-preview skill for API details).

For `manual`, output the prompt for user to copy.

---

### Step 5: Update README.md

**Badge Placement Strategy**:

1. Locate existing badge section (lines starting with `[![` after H1)
2. If found, replace entire badge block
3. If not found, insert after first H1 heading
4. Preserve any custom badges not matching generated patterns

**Badge Section Format**:
```markdown
# Project Name

[![License](badge-url)](link)
[![Version](badge-url)](link)
[![Build](badge-url)](link)
...
```

**Infographic Placement with Prompt Display**:

Insert after badges, before main description. **Always include the collapsible prompt section** (like documentation-review pattern):

```markdown
# Project Name

[![badges...](badges)]

Short one-line description.

<!-- Infographic: Visual summary of project -->
<p align="center">
  <img src=".github/readme-infographic.svg" alt="Project Overview" width="800">
</p>

<details>
<summary>View infographic generation prompt</summary>

```
[The exact prompt or SVG generation description used]

Layout: [style] layout (1280x640)
Background: [color description]

Sections:
1. HEADER: "[project-name]" - [tagline]
2. [ARCHITECTURE/FEATURES]: [description]
3. [TECHNOLOGY]: [stack]
4. FOOTER: [install command] | [links]

Color palette: [colors with hex codes]
Style: [style description]
```

</details>

## Features

...rest of README...
```

**Dark Mode Support**:

If `dark_mode: both`, use the `<picture>` element:

```markdown
<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/readme-infographic-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset=".github/readme-infographic.svg">
    <img src=".github/readme-infographic.svg" alt="Project Overview" width="800">
  </picture>
</p>
```

### Step 6: Output Results

**If update_readme is true**:
1. Write SVG infographic to `infographic_output` path
2. If `dark_mode: both`, write dark variant as well
3. Update README.md with badges, infographic, and prompt display
4. Report changes made

**If update_readme is false** (or `--dry-run`):
1. Output badge markdown for manual insertion
2. Display SVG content or image prompt
3. Show README changes that would be made
4. Provide placement instructions

## Color Palettes by Domain

**DevTools/CLI**:
- Background: #0f172a → #1e293b
- Accent: #06b6d4 (cyan)
- Text: #f8fafc

**AI/ML**:
- Background: #1e1b4b → #312e81
- Accent: #8b5cf6 (violet)
- Text: #f8fafc

**Web/Frontend**:
- Background: #0c4a6e → #075985
- Accent: #3b82f6 (blue)
- Text: #f8fafc

**Data/Analytics**:
- Background: #1e3a5f → #0f172a
- Accent: #f59e0b (amber), #3b82f6 (blue)
- Text: #f8fafc

**Security**:
- Background: #0f172a → #1e293b
- Accent: #22c55e (green), #eab308 (yellow)
- Text: #f8fafc

**Infrastructure**:
- Background: #1e293b → #334155
- Accent: #06b6d4 (cyan)
- Text: #f8fafc

## Error Handling

**No README.md found**:
Create minimal README with project name from manifest, then enhance.

**No package manifest found**:
Use git remote URL to extract project name, prompt for missing info.

**Git remote not configured**:
Ask user for repository owner/name for badge URLs.

**Existing custom badges**:
Preserve badges that don't match generated patterns, merge intelligently.

**SVG file too large** (>100KB):
Simplify layout, reduce sections, warn user.

## Configuration Reference

Full configuration options in `.claude/github-social.local.md`:

```yaml
---
# Image generation provider
provider: svg              # svg | dalle-3 | gemini | manual

# Badge settings
badge_style: for-the-badge # flat | flat-square | plastic | for-the-badge
badge_color: blueviolet    # Default color for custom badges

# Infographic settings
infographic_style: hybrid  # architecture | features | hybrid
infographic_output: .github/readme-infographic.svg
dark_mode: false           # false | true | both

# Behavior
update_readme: true        # Modify README.md directly
custom_badges: []          # Additional custom badges to include
exclude_badges: []         # Badge types to exclude
---
```

## Additional Resources

### Reference Files

For detailed patterns and comprehensive catalogs:
- **`references/badge-catalog.md`** - Complete badge patterns by category
- **`references/svg-infographic-patterns.md`** - SVG layout templates by project type
- **`references/infographic-prompts.md`** - AI image prompt templates
- **`references/readme-templates.md`** - Badge placement and README structure patterns

### Example Files

Working examples for different project types:
- **`examples/badges-devtools.md`** - Badge set for CLI/DevTools projects
- **`examples/badges-library.md`** - Badge set for libraries and SDKs
- **`examples/infographic-example.md`** - Complete infographic prompt example
