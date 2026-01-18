---
description: Generate a social preview image for the current GitHub repository
argument-hint: [--style abstract|illustrated|minimalist] [--provider dalle-3|manual]
allowed-tools: Read, Glob, Grep, Bash, Write
---

Generate a social preview image for this GitHub repository that captures the project's spirit and purpose.

## Task

Analyze this project and generate a social preview image suitable for GitHub's repository settings.

## Requirements

The image must meet GitHub's social preview requirements:
- Minimum: 640x320 pixels
- Recommended: 1280x640 pixels (2:1 aspect ratio)
- Maximum file size: 1MB
- Output location: `.github/social-preview.png` (or configured path)

## Process

### 1. Check Configuration

Look for `.claude/github-social.local.md` configuration file. If it exists, parse the YAML frontmatter for settings:
- provider, style, output_path, dimensions, include_text, colors

Command-line overrides (if provided via $ARGUMENTS):
- `--style [value]` overrides configured style
- `--provider [value]` overrides configured provider

### 2. Analyze Project

Read available project files to understand purpose:
- README.md (primary)
- package.json / Cargo.toml / pyproject.toml / go.mod
- CLAUDE.md
- Source code structure

Extract:
- Project name
- Purpose/description
- Domain (DevTools, AI, Web, Data, Security, etc.)
- Technology stack
- Project "spirit" (what makes it unique)

### 3. Generate Image

Use the social-preview skill to:
1. Design a visual concept based on analysis
2. Generate an optimized image prompt
3. If provider is configured with valid API key:
   - Call the image generation API
   - Download and resize to 1280x640
   - Save to output path
4. If no provider or manual mode:
   - Output the detailed prompt
   - Provide instructions for manual generation

### 4. Report Results

If image generated:
- Confirm file location
- Report dimensions and file size
- Provide instructions for uploading to GitHub

If prompt-only:
- Display the generated prompt
- Show negative prompt
- Provide usage instructions for different tools

## GitHub Upload Instructions

After image is generated, remind user:
1. Go to repository Settings
2. Under "Social preview", click "Edit"
3. Upload the generated image
4. Save changes

The image will appear when the repository is shared on social media platforms.
