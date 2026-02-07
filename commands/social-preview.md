---
name: github-social:social-preview
description: Generate a social preview image for the current GitHub repository
argument-hint: "[--provider svg|dalle-3|gemini|manual] [--style minimal|geometric|illustrated] [--dark-mode] [--upload]"
allowed-tools: Read, Glob, Grep, Bash, Write, mcp__github__create_or_update_file, mcp__github__get_file_contents, Skill
---

## Memory

Capture after: `/mnemonic:capture patterns "{REPO} social preview {PROVIDER} style"`

Generate a social preview image for this GitHub repository that captures the project's spirit and purpose.

## Task

Analyze this project and generate a social preview image suitable for GitHub's repository settings.

## Requirements

The image must meet GitHub's social preview requirements:
- Minimum: 640x320 pixels
- Recommended: 1280x640 pixels (2:1 aspect ratio)
- Maximum file size: 1MB
- Output locations:
  - `.github/social-preview.svg` - Editable source file
  - `.github/social-preview.jpg` - **Always generated** (required for GitHub upload)

**IMPORTANT**: GitHub requires a raster image (JPG/PNG) for social preview. SVG generation ALWAYS produces both SVG and JPG files.

## Process

### 1. Check Configuration

Look for `.claude/github-social.local.md` configuration file. If it exists, parse the YAML frontmatter for settings:
- `provider`: svg (default), dalle-3, gemini, manual
- `svg_style`: minimal (default), geometric, illustrated
- `dark_mode`: false (default), true, both
- `output_path`, `dimensions`, `include_text`, `colors`

Command-line overrides (if provided via $ARGUMENTS) - **highest priority**:
- `--provider [value]` overrides configured provider (svg, dalle-3, gemini, manual)
- `--style [value]` overrides svg_style (minimal, geometric, illustrated)
- `--dark-mode` generates dark mode variant (or both if provider supports)
- `--upload` uploads the generated image to the GitHub repository

### 2. Analyze Project

Read available project files to understand purpose:
- README.md (primary)
- package.json / Cargo.toml / pyproject.toml / go.mod
- CLAUDE.md
- Source code structure

Extract:
- Project name
- Purpose/description
- Domain (DevTools, AI, Web, Data, Security, Infrastructure, Plugin)
- Technology stack
- Project "spirit" (what makes it unique)

### 3. Generate Image

Use the social-preview skill based on provider setting:

**For SVG (default)**:
1. Design visual concept based on project domain
2. Generate clean SVG following domain templates
3. Apply svg_style (minimal, geometric, illustrated)
4. Save SVG to output path (`.github/social-preview.svg`)
5. **MANDATORY: Convert SVG to JPG** (`.github/social-preview.jpg`)
   - Dimensions: 1280x640
   - Quality: 90
   - File size: < 1MB
6. If `dark_mode: both`, generate dark variants (SVG and JPG)

**For DALL-E 3** (`--provider=dalle-3`):
1. Verify `OPENAI_API_KEY` is available
2. Generate optimized image prompt
3. Call DALL-E 3 API
4. Resize to 1280x640 and save as PNG

**For Gemini** (`--provider=gemini`):
1. Verify `GEMINI_API_KEY` is available
2. Generate optimized image prompt
3. Call Gemini API (gemini-2.5-flash-image)
4. Save to output path

**For Manual** (`--provider=manual`):
1. Generate optimized prompt for DALL-E/Midjourney/etc.
2. Output prompt with usage instructions

### 4. Upload to Repository (if --upload flag or upload_to_repo: true)

If upload is requested:
1. Detect repository owner and name from git remote
2. Read the generated image and encode as base64
3. Check if file already exists (get SHA if updating)
4. Use `mcp__github__create_or_update_file` to upload the image
5. Report upload success with commit URL

### 5. Report Results

**If SVG generated** (always includes JPG):
- Confirm SVG file location and size
- **Confirm JPG file location and size** (for GitHub upload)
- Report dimensions (1280x640)
- If `dark_mode: both`, report all variants (SVG + JPG for each)

**If AI-generated image (DALL-E/Gemini)**:
- Confirm file location
- Report dimensions and file size
- If uploaded: provide commit URL

**If prompt-only**:
- Display the generated prompt
- Show negative prompt
- Provide usage instructions for different tools

## Provider Comparison

| Provider | Output | Cost | Speed | Quality |
|----------|--------|------|-------|---------|
| **svg** (default) | SVG + JPG | Free | Instant | Clean, predictable |
| **dalle-3** | PNG image | ~$0.08 | 5-15s | Artistic, varied |
| **gemini** | PNG image | ~$0.039 | 3-10s | Good quality |
| **manual** | Text prompt | Free | Instant | N/A |

**Note**: SVG provider always generates both `.svg` (editable source) and `.jpg` (for GitHub upload).

## Example Usage

```bash
# Default: Generate SVG (recommended)
/social-preview

# Generate SVG with geometric style
/social-preview --style geometric

# Generate with dark mode variant
/social-preview --dark-mode

# Use DALL-E 3 for artistic image
/social-preview --provider=dalle-3

# Use Gemini for image generation
/social-preview --provider=gemini

# Generate and upload to repository
/social-preview --upload

# Get prompt only for manual generation
/social-preview --provider=manual
```

## GitHub Upload Instructions

If `--upload` was used:
- Both SVG and JPG have been committed to the repository
- Direct user to repository Settings → General → Social preview to select the **JPG** image

If manual upload needed:
1. Go to repository Settings
2. Under "Social preview", click "Edit"
3. Upload the **JPG** file (`.github/social-preview.jpg`)
4. Save changes

**Important**: GitHub requires a raster image (JPG/PNG) for social preview. Upload the `.jpg` file, not the `.svg`.

The image will appear when the repository is shared on social media platforms.

