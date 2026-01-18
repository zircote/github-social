---
description: Interactive setup wizard for github-social configuration
allowed-tools: Read, Write, Bash
---

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
- dalle-3: Automated generation using OpenAI DALL-E 3 (requires OPENAI_API_KEY)
- stable-diffusion: Local or API-based Stable Diffusion
- manual: Output prompt only for use with any tool

**Style Selection**:
- abstract: Geometric shapes, gradients, tech patterns
- illustrated: Hand-drawn, artistic, warm feel
- minimalist: Clean, simple, text-focused
- custom: User provides custom style description

**Additional Options**:
- Include project name in image? (true/false)
- Color scheme preference (auto/dark/light/custom)
- Output path (default: .github/social-preview.png)

### 3. Validate Provider Setup

If user selects dalle-3:
- Check if OPENAI_API_KEY environment variable exists
- If not, explain how to set it up
- Offer to proceed with manual fallback

If user selects stable-diffusion:
- Ask for API endpoint or confirm local setup
- Note any additional environment variables needed

### 4. Create Configuration File

Create `.claude/` directory if needed, then write `.claude/github-social.local.md`:

```yaml
---
provider: [selected]
api_key_env: [if applicable]
style: [selected]
output_path: .github/social-preview.png
dimensions: 1280x640
include_text: [true/false]
colors: [selected]
custom_style: [if custom]
custom_colors: [if custom]
---

# Additional Context (Optional)

[Any brand guidelines or preferences the user mentioned]
```

### 5. Confirm Setup

Display the created configuration and explain:
- How to generate images: `/social-preview` or ask naturally
- How to modify settings: edit `.claude/github-social.local.md` or run `/social-preview:setup` again
- Remind about `.gitignore` if they want to keep config private

### 6. Offer Test Generation

Ask if user wants to generate a preview image now to test the configuration.

## Notes

- Configuration file uses YAML frontmatter in markdown format
- The `.local.md` suffix indicates this is a local/personal config
- Consider adding `.claude/github-social.local.md` to `.gitignore` if it contains sensitive preferences
- API keys should NEVER be stored in the config file - only environment variable names
