---
name: social-preview
description: This skill should be used when the user asks to "generate social preview", "create social image", "github social preview", "repository preview image", "og image", "social media card", "create repo image", "github card image", or needs to create an image that captures a project's spirit and purpose for social media sharing. Analyzes project files to understand intent and generates optimized SVG graphics or AI-generated images.
---

# Social Preview Image Generator

Generate social preview images for GitHub repositories by analyzing project intent, purpose, and spirit. Create images that capture what a project does and why it matters.

## Overview

This skill analyzes a project's codebase, documentation, and configuration to understand its purpose, then generates:
1. **SVG graphics** (default) - Clean, minimal vector graphics generated directly
2. **AI-generated images** - Via DALL-E 3 or Gemini when configured
3. **Text prompts** - For manual use with Midjourney or other tools

## GitHub Image Requirements

All generated images must meet GitHub's social preview requirements:
- **Minimum**: 640x320 pixels
- **Recommended**: 1280x640 pixels (2:1 aspect ratio)
- **Maximum file size**: 1MB
- **Formats**: JPG (required for GitHub), SVG (source file)
- **Default outputs**:
  - `.github/social-preview.svg` - Editable source file
  - `.github/social-preview.jpg` - **Always generated** for GitHub upload

**IMPORTANT**: GitHub's social preview setting requires a raster image (JPG/PNG). SVG generation MUST ALWAYS also produce a JPG file.

## Workflow

### Step 1: Check for Configuration

Look for optional settings file at `.claude/github-social.local.md`.

If file exists, parse YAML frontmatter for:
- `provider`: Image generation provider (svg, dalle-3, gemini, manual) - **default: svg**
- `api_key_env`: Environment variable name containing API key (for dalle-3 or gemini)
- `svg_style`: SVG style preference (minimal, geometric, illustrated) - **default: minimal**
- `dark_mode`: Dark mode support (false, true, both) - **default: false**
- `output_path`: Where to save the image
- `dimensions`: Image dimensions (default: 1280x640)
- `include_text`: Whether to include project name in image
- `colors`: Color scheme preference (auto, dark, light, custom)
- `upload_to_repo`: Whether to upload the image to the GitHub repository

**Command-line overrides** (highest priority):
- `--provider=svg|dalle-3|gemini|manual`
- `--dark-mode` (generates dark variant or both)
- `--upload` (upload to repository)

If no config exists, use defaults: `provider: svg`, `svg_style: minimal`.

### Step 2: Analyze Project

Gather project context by reading available files:

**Primary sources** (check in order):
1. `README.md` - Project description, features, purpose
2. `package.json` / `Cargo.toml` / `pyproject.toml` / `go.mod` - Name, description, keywords
3. `CLAUDE.md` - Project context and guidelines
4. `.github/FUNDING.yml` - Project goals/sustainability info

**Secondary sources** (if primary insufficient):
- Source code structure (main entry points)
- License file (project nature: open source, commercial)
- Documentation directory
- Configuration files

**Extract these elements**:
- **Project name**: Official name
- **Purpose**: What problem does it solve?
- **Domain**: What field/industry? (DevTools, AI, Finance, etc.)
- **Technology**: Primary language/framework
- **Spirit**: What makes it unique? What's its personality?
- **Keywords**: Key concepts, features, or themes

### Step 3: Design Visual Concept

Based on analysis, design a visual concept that:

1. **Represents the domain visually**
   - DevTools: Terminal aesthetics, code patterns, geometric shapes
   - AI/ML: Neural networks, data flows, abstract intelligence
   - Web: Browser elements, connectivity, responsive layouts
   - Data: Charts, graphs, structured patterns
   - Security: Shields, locks, protective barriers
   - Infrastructure: Cloud shapes, server racks, pipelines

2. **Captures the project's spirit**
   - Fast/Performance: Motion lines, streamlined shapes
   - Reliable/Stable: Solid foundations, balanced compositions
   - Modern/Cutting-edge: Gradients, futuristic elements
   - Simple/Minimal: Clean lines, whitespace, focused elements
   - Powerful/Feature-rich: Layered elements, depth

3. **Applies style preferences**
   - **Minimal**: Clean lines, 3-5 shapes max, generous whitespace
   - **Geometric**: 8-15 shapes, grid/flow arrangements, abstract metaphors
   - **Illustrated**: Organic paths, hand-drawn aesthetic, warm colors

### Step 4: Generate Output Based on Provider

Route to appropriate generation method based on provider setting.

---

#### Step 4a: Generate SVG (Default)

When `provider: svg` or not configured, generate a clean SVG file directly.

**SVG Design Principles**:
- Simple geometric shapes (rectangles, circles, paths)
- Clean sans-serif typography (system fonts)
- Maximum 3-4 colors from domain palette
- No raster images or complex filters
- Viewbox: 0 0 1280 640
- File size target: <50KB

**SVG Template Structure**:
```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1280 640">
  <defs>
    <!-- Optional: gradients, patterns -->
    <linearGradient id="bg-gradient" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:[color1]"/>
      <stop offset="100%" style="stop-color:[color2]"/>
    </linearGradient>
  </defs>

  <!-- Background -->
  <rect width="1280" height="640" fill="url(#bg-gradient)"/>

  <!-- Domain-specific visual elements (3-15 shapes based on svg_style) -->

  <!-- Project name (if include_text: true) -->
  <text x="640" y="340"
        font-family="-apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', sans-serif"
        font-size="72" font-weight="700"
        text-anchor="middle" fill="[text-color]">
    [PROJECT-NAME]
  </text>

  <!-- Optional tagline -->
  <text x="640" y="400"
        font-family="-apple-system, BlinkMacSystemFont, 'Inter', sans-serif"
        font-size="24" font-weight="400"
        text-anchor="middle" fill="[secondary-text-color]" opacity="0.8">
    [tagline]
  </text>
</svg>
```

**SVG Style Guidelines**:

**Minimal** (default):
- 3-5 simple shapes maximum
- Single gradient or solid background
- Project name as focal point
- Generous whitespace (60%+ empty space)
- Subtle accent shapes

**Geometric**:
- 8-15 geometric shapes
- Grid or flow-based arrangement
- Represents domain metaphor abstractly
- More visual complexity, balanced composition

**Illustrated**:
- Organic SVG paths with curves
- Hand-drawn aesthetic using stroke variations
- Warm, approachable color palette
- Textured appearance via path styling

**Domain-Specific SVG Patterns**:

See `references/svg-templates.md` for complete templates by domain.

**Dark Mode Support**:

If `dark_mode: true` or `dark_mode: both`:
- Invert background: light (#f8fafc) ↔ dark (#0f172a)
- Adjust text colors for contrast
- Adjust accent colors for dark backgrounds
- If `both`: save as `[name].svg` and `[name]-dark.svg`

**Save SVG and Convert to JPG**:
1. Write SVG content to `output_path` (default: `.github/social-preview.svg`)
2. Verify SVG file size < 50KB (warn if larger)
3. **MANDATORY: Convert SVG to JPG**:
   ```bash
   # Convert SVG to JPG at 1280x640, quality 90
   convert .github/social-preview.svg -resize 1280x640! -quality 90 -background white -flatten .github/social-preview.jpg
   ```
4. Verify JPG meets requirements:
   - Dimensions: 1280x640
   - File size: < 1MB (compress if needed)
5. If `dark_mode: both`, generate dark variant SVG and JPG:
   - `.github/social-preview-dark.svg`
   - `.github/social-preview-dark.jpg`

**Note**: The JPG is the file to upload to GitHub's social preview setting. The SVG is kept as an editable source.

---

#### Step 4b: Generate with DALL-E 3

When `provider: dalle-3`:

1. **Verify API key**: Check `$OPENAI_API_KEY` or configured `api_key_env`

2. **Craft optimized prompt** (see Step 4d for prompt structure)

3. **Call DALL-E API**:
```bash
curl -s https://api.openai.com/v1/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "dall-e-3",
    "prompt": "[generated prompt]",
    "n": 1,
    "size": "1792x1024",
    "quality": "hd",
    "response_format": "b64_json"
  }'
```

4. **Process response**:
```bash
# Decode base64 and resize to 1280x640
echo "[base64_data]" | base64 -d > temp_image.png
convert temp_image.png -resize 1280x640! -quality 90 .github/social-preview.png
```

---

#### Step 4c: Generate with Gemini (Nano Banana)

When `provider: gemini`:

1. **Verify API key**: Check `$GEMINI_API_KEY` or configured `api_key_env`

2. **Select model**:
   - `gemini-2.5-flash-image` - Fast, efficient (default)
   - `gemini-3-pro-image-preview` - Higher quality with reasoning, better text rendering

3. **Craft optimized prompt** (see Step 4d for prompt structure)

4. **Call Gemini API**:
```bash
curl -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-image:generateContent" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{
      "parts": [{"text": "[generated prompt]"}]
    }],
    "generationConfig": {
      "responseModalities": ["IMAGE"],
      "imageConfig": {
        "aspectRatio": "16:9"
      }
    }
  }'
```

5. **Process response**:
   - Extract image data from response
   - Save to output path
   - Resize if needed to 1280x640

---

#### Step 4d: Generate Text Prompt (Manual)

When `provider: manual` or API generation fails:

**Prompt structure**:
```
[Style description], [Main subject/concept], [Visual elements representing project],
[Color palette], [Composition notes], [Technical specs]

Negative prompt: [Elements to avoid]
```

**Example prompt for a CLI tool**:
```
Abstract geometric composition, terminal-inspired design with command line aesthetics,
interconnected nodes representing automation workflows, dark background with cyan and
magenta accent gradients, centered composition with depth layers, clean professional
tech aesthetic, 1280x640 pixels, high contrast, modern minimalist

Negative prompt: text, watermarks, realistic photos, cluttered, busy patterns
```

**Include project name in prompt if `include_text: true`**:
```
... with stylized text "PROJECT-NAME" integrated into design ...
```

### Step 5: Verify Output

**For SVG output** (always accompanied by JPG):
1. Verify valid XML structure
2. Check SVG file size < 50KB (warn if 50-100KB, error if >100KB)
3. Verify viewBox is 1280x640
4. **Verify JPG was created**:
   - File exists at `.github/social-preview.jpg`
   - Dimensions are exactly 1280x640
   - File size is under 1MB
   - If exceeds 1MB, recompress with lower quality

**For AI-generated image output (PNG/JPG from DALL-E/Gemini)**:
1. Verify file exists at output path
2. Check dimensions meet requirements (≥640x320, recommended 1280x640)
3. Check file size is under 1MB
4. If exceeds 1MB, compress progressively

**Report success with**:
- SVG file location and size
- **JPG file location and size** (for GitHub upload)
- Dimensions (1280x640)
- Provider used

### Step 6: Upload to Repository (Optional)

If `upload_to_repo: true` is configured (or `--upload` flag provided):

1. **Detect repository info**:
   - Parse `git remote get-url origin` to extract owner and repo name
   - Determine current branch with `git branch --show-current`

2. **Prepare upload** (upload BOTH files):
   - Read the SVG source file and JPG output file
   - Encode content as base64 for GitHub API

3. **Check for existing files**:
   - Use GitHub API to check if files exist
   - If exist, retrieve the SHA for update operation

4. **Upload via GitHub MCP tool** (both SVG and JPG):
   ```yaml
   # Upload JPG (for GitHub social preview)
   owner: [detected owner]
   repo: [detected repo]
   path: .github/social-preview.jpg
   content: [base64 encoded JPG]
   message: "chore: update social preview image"
   branch: [current or default branch]
   sha: [existing file SHA if updating]
   ```
   ```yaml
   # Upload SVG (editable source)
   owner: [detected owner]
   repo: [detected repo]
   path: .github/social-preview.svg
   content: [base64 encoded SVG]
   message: "chore: update social preview source SVG"
   branch: [current or default branch]
   sha: [existing file SHA if updating]
   ```

5. **Report upload status**:
   - Confirm successful upload with commit URLs
   - **Remind user**: The JPG file is what should be selected in GitHub settings
   - Provide GitHub settings link for activating the social preview

**Note**: To set as actual social preview, users must:
1. Go to repository Settings → General
2. Scroll to "Social preview"
3. Click "Edit" and select the **JPG** image (`.github/social-preview.jpg`)

## Color Palettes by Domain

**DevTools/CLI**:
- Background: #0f172a (slate-900), #1e293b (slate-800)
- Accent: #06b6d4 (cyan-500), #22d3ee (cyan-400)
- Text: #f8fafc (slate-50)

**AI/ML**:
- Background: #1e1b4b (indigo-950), #312e81 (indigo-900)
- Accent: #8b5cf6 (violet-500), #a78bfa (violet-400)
- Text: #f8fafc (slate-50)

**Web/Frontend**:
- Background: #0c4a6e (sky-900), #075985 (sky-800)
- Accent: #3b82f6 (blue-500), #60a5fa (blue-400)
- Text: #f8fafc (slate-50)

**Data/Analytics**:
- Background: #1e3a5f, #0f172a
- Accent: #f59e0b (amber-500), #3b82f6 (blue-500)
- Text: #f8fafc (slate-50)

**Security**:
- Background: #0f172a (slate-900), #1e293b (slate-800)
- Accent: #22c55e (green-500), #eab308 (yellow-500)
- Text: #f8fafc (slate-50)

**Infrastructure**:
- Background: #1e293b (slate-800), #334155 (slate-700)
- Accent: #06b6d4 (cyan-500), #64748b (slate-500)
- Text: #f8fafc (slate-50)

## Error Handling

**No README or project files found**:
Ask user to describe the project purpose, then proceed with that information.

**API key not found** (for dalle-3 or gemini):
Fall back to SVG generation or prompt-only output with instructions.

**Image generation fails**:
1. Report the error
2. Fall back to SVG generation
3. Or output the prompt for manual use

**SVG file too large** (>100KB):
1. Simplify shapes (reduce complexity)
2. Remove unnecessary elements
3. Warn user and suggest switching to minimal style

## Additional Resources

### Reference Files
- **`references/svg-templates.md`** - Complete SVG templates by domain
- **`references/prompt-patterns.md`** - Detailed prompt patterns by domain
- **`references/provider-apis.md`** - API documentation for supported providers

### Example Files
- **`examples/config-svg.md`** - Example SVG configuration (default)
- **`examples/config-dalle.md`** - Example DALL-E configuration
- **`examples/config-gemini.md`** - Example Gemini configuration
