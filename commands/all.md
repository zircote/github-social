---
name: all
description: Run all github-social skills in sequence - metadata, social preview, and README enhancement
argument-hint: "[--apply] [--dry-run] [--provider svg|dalle-3|gemini|manual] [--skip-badges] [--skip-infographic] [--dark-mode]"
allowed-tools: ["Read", "Write", "Edit", "Glob", "Grep", "Bash", "MCPSearch", "mcp__github__create_or_update_file", "Skill"]
---

## Memory

Capture after: `/mnemonic:capture patterns "{REPO} GitHub Social full enhancement"`

# Complete GitHub Social Enhancement

Run all github-social skills in sequence to fully optimize a repository's presentation:

1. **repo-metadata**: Generate optimized description and topics
2. **social-preview**: Generate social preview image (SVG by default)
3. **readme-enhance**: Add badges and infographic to README

## Execution Steps

### 1. Parse Arguments

Handle optional flags:
- `--apply`: Apply all changes (update GitHub metadata, modify README, upload images)
- `--dry-run`: Preview all changes without applying any
- `--provider [value]`: Override image provider for both social preview and infographic (svg, dalle-3, gemini)
- `--skip-badges`: Skip badge generation in README enhancement
- `--skip-infographic`: Skip infographic generation
- `--dark-mode`: Generate dark mode variants for images

Default behavior: Generate SVG images, preview changes, prompt before applying.

### 2. Load Configuration

Read `.claude/github-social.local.md` for all skill settings:
- `provider`: svg (default), dalle-3, gemini, manual
- `svg_style`: minimal (default), geometric, illustrated
- `dark_mode`: false (default), true, both
- Badge style preferences
- Infographic style preferences
- Upload settings

Command-line flags override configuration.

### 3. Analyze Project (Shared)

Perform comprehensive project analysis once, reuse across all skills:

1. Read README.md, package manifest, CLAUDE.md
2. Extract from git remote: owner, repo name, default branch
3. Identify:
   - Project name and version
   - Purpose and key features
   - Domain (DevTools, AI, Web, Data, Security, Infrastructure, Plugin)
   - Primary language and frameworks
   - CI/CD configuration
   - License type
   - Documentation sites
   - Community links

### 4. Execute Skills in Sequence

#### Step 4a: Repository Metadata

Generate optimized GitHub metadata:
- Description (max 350 chars, engaging, SEO-friendly)
- Topics (max 20, lowercase, hyphenated)

Output preview of changes.

#### Step 4b: Social Preview Image

Generate social preview based on provider:

**SVG (default)**:
- Generate clean SVG using domain templates
- Save SVG to `.github/social-preview.svg`
- **MANDATORY: Convert to JPG** (`.github/social-preview.jpg`)
  - Dimensions: 1280x640, Quality: 90, Size: < 1MB
- If `--dark-mode`, generate dark variants (SVG + JPG)

**DALL-E 3 / Gemini**:
- Generate optimized image prompt
- Call API and save PNG
- Save to `.github/social-preview.png`

Output the generated file locations (SVG + JPG for SVG provider, or prompt if manual).

#### Step 4c: README Enhancement

Unless `--skip-badges` and `--skip-infographic` both set:
- Generate shields.io badges (unless `--skip-badges`)
- Generate infographic SVG/image (unless `--skip-infographic`)
- Add collapsible prompt display section
- Prepare README.md updates

Output badge set and infographic location.

### 5. Present Summary

Display complete summary:

```
## GitHub Social Enhancement Summary

### Repository Metadata
- Description: "..." (X chars)
- Topics: topic-1, topic-2, topic-3, ...

### Social Preview
- Provider: svg (default)
- SVG Output: .github/social-preview.svg (X KB)
- JPG Output: .github/social-preview.jpg (X KB) - **For GitHub upload**
- Dimensions: 1280x640
- [Generated/Pending]

### README Enhancement
- Badges: X badges generated
- Infographic: .github/readme-infographic.svg
- Dark mode: [yes/no]
- README changes: [Preview/Applied]

### Next Steps
[Instructions based on --apply flag]
```

### 6. Apply Changes (if --apply or confirmed)

If `--apply` flag or user confirms:

1. **Update GitHub metadata** (via gh CLI):
   ```bash
   gh repo edit --description "..."
   gh repo edit --add-topic topic-1 --add-topic topic-2 ...
   ```

2. **Save/upload social preview**:
   - Write SVG/PNG to output path
   - Upload to repository if `upload_to_repo: true`

3. **Update README.md**:
   - Insert/update badge section
   - Add infographic with prompt display section
   - If dark mode, use `<picture>` element for theme switching

4. **Report results**:
   - Confirm each step completed
   - Provide GitHub settings links for manual steps

## Provider Comparison

| Provider | Social Preview | Infographic | Cost | Speed |
|----------|---------------|-------------|------|-------|
| **svg** (default) | SVG + JPG | SVG file | Free | Instant |
| **dalle-3** | PNG image | PNG image | ~$0.16 | 10-30s |
| **gemini** | PNG image | PNG image | ~$0.08 | 6-20s |

**Note**: SVG provider always generates both `.svg` (editable) and `.jpg` (for GitHub upload).

## Example Usage

```bash
# Preview all enhancements (SVG default)
/github-social:all

# Apply all changes automatically
/github-social:all --apply

# Dry run to see what would change
/github-social:all --dry-run

# Use DALL-E for artistic images
/github-social:all --provider=dalle-3 --apply

# Use Gemini for image generation
/github-social:all --provider=gemini --apply

# Generate with dark mode support
/github-social:all --dark-mode --apply

# Skip certain features
/github-social:all --apply --skip-infographic
/github-social:all --apply --skip-badges
```

## Tips

- Run without `--apply` first to review changes
- SVG generation is free and instant (recommended default)
- SVG always generates JPG for GitHub compatibility
- Ensure `gh` CLI is authenticated for metadata updates
- Configure `.claude/github-social.local.md` for custom settings
- Social preview requires selecting the **JPG** in GitHub settings
- Use `--dark-mode` for repositories viewed in both themes

## Related Commands

- `/social-preview` - Generate social preview image only
- `/repo-metadata` - Generate metadata only
- `/readme-enhance` - Enhance README only
- `/github-social:setup` - Configure plugin settings

