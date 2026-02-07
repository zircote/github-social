---
name: github-social:setup
description: Interactive setup wizard for github-social configuration
allowed-tools: Read, Write, Bash, AskUserQuestion, Skill
---

## Memory

Capture after: `/mnemonic:capture decisions "{REPO} GitHub Social setup configuration"`

Set up the github-social plugin configuration for this project.

## Task

Guide the user through configuring their social preview generation preferences and create the configuration file.

## Process

### 1. Check Existing Configuration

Check if `.claude/github-social.local.md` already exists. If it does:
- Show current configuration
- Ask if user wants to update or start fresh

### 2. Gather Preferences

Ask the user to collect configuration options:

**Provider Selection**:
- svg (Recommended): Claude generates clean SVG graphics directly. Free, instant, editable.
- dalle-3: AI-generated images using OpenAI DALL-E 3. Requires OPENAI_API_KEY (~$0.08/image)
- gemini: AI-generated images using Google Gemini. Requires GEMINI_API_KEY (~$0.039/image)
- manual: Output optimized prompt only for use with Midjourney or other tools

**SVG Style Selection** (only if provider is svg):
- minimal (Recommended): Clean design with 3-5 shapes, generous whitespace
- geometric: Complex arrangements with 8-15 shapes representing domain metaphors
- illustrated: Organic paths with hand-drawn aesthetic and warm colors

**Dark Mode**:
- false (default): Generate light mode only
- true: Generate dark mode only
- both: Generate both light and dark variants

**Additional Options**:
- Include project name in image? (true/false, default: true)
- Color scheme preference (auto/dark/light/custom)
- Output path (default: .github/social-preview.svg for SVG, .github/social-preview.png for AI)
- Upload to repository? (true/false, default: false)

### 3. Validate Provider Setup

If user selects svg:
- No additional setup required
- Explain SVG benefits (free, instant, editable)

If user selects dalle-3:
- Check if OPENAI_API_KEY environment variable exists
- If not, explain how to set it up
- Offer to proceed with SVG fallback

If user selects gemini:
- Check if GEMINI_API_KEY environment variable exists
- If not, explain how to get one from Google AI Studio
- Offer to proceed with SVG fallback

### 4. Create Configuration File

Create `.claude/` directory if needed, then write `.claude/github-social.local.md`:

```yaml
---
# Image generation provider (default: svg)
# Options: svg, dalle-3, gemini, manual
provider: [selected]

# API key environment variable (only needed for dalle-3 or gemini)
# api_key_env: OPENAI_API_KEY    # For dalle-3
# api_key_env: GEMINI_API_KEY    # For gemini

# SVG-specific settings (only used when provider: svg)
svg_style: [minimal | geometric | illustrated]

# Dark mode support (default: false)
# false = light mode only, true = dark mode only, both = generate both variants
dark_mode: [false | true | both]

# Output settings
output_path: [.github/social-preview.svg or .png]
dimensions: 1280x640
include_text: [true/false]
colors: [auto | dark | light | custom]

# README infographic settings
infographic_output: .github/readme-infographic.svg
infographic_style: hybrid       # architecture | features | hybrid

# Upload to repository (requires gh CLI or GITHUB_TOKEN)
upload_to_repo: [true/false]
---

# GitHub Social Plugin Configuration

This configuration was created by `/github-social:setup`.

## Provider Options

### SVG (Default - Recommended)
Claude generates clean, minimal SVG graphics directly. No API key required.
- **Pros**: Free, instant, editable, small file size (10-50KB)
- **Best for**: Professional, predictable results

### DALL-E 3
OpenAI's image generation. Requires `OPENAI_API_KEY`.
- **Pros**: Artistic, creative, varied styles
- **Cost**: ~$0.08 per image

### Gemini
Google's image generation via Gemini API (Nano Banana). Requires `GEMINI_API_KEY`.
- **Models**: `gemini-2.5-flash-image` (fast), `gemini-3-pro-image-preview` (quality)
- **Cost**: ~$0.039 per image

### Manual
Outputs optimized prompts for Midjourney or other tools.

## SVG Style Options

**Minimal** (default): Clean, simple design with project name and subtle geometric accents. Maximum 3-5 shapes, generous whitespace.

**Geometric**: More complex arrangements with 8-15 geometric shapes representing domain metaphors abstractly.

**Illustrated**: Hand-drawn aesthetic using organic SVG paths with warm colors.

## Dark Mode

Set `dark_mode: both` to generate light and dark variants automatically:
- `.github/social-preview.svg` (light)
- `.github/social-preview-dark.svg` (dark)

## Command Overrides

Override any setting via command flags:
```bash
/social-preview --provider=dalle-3 --dark-mode
/readme-enhance --provider=gemini
```
```

### 5. Confirm Setup

Display the created configuration and explain:
- How to generate images: `/social-preview` or ask naturally
- How to enhance README: `/readme-enhance`
- How to run all skills: `/github-social:all`
- How to modify settings: edit `.claude/github-social.local.md` or run `/github-social:setup` again
- Remind about `.gitignore` if they want to keep config private

### 6. Offer Test Generation

Ask if user wants to generate a preview image now to test the configuration.

## Notes

- Configuration file uses YAML frontmatter in markdown format
- The `.local.md` suffix indicates this is a local/personal config
- Consider adding `.claude/github-social.local.md` to `.gitignore` if it contains sensitive preferences
- API keys should NEVER be stored in the config file - only environment variable names

