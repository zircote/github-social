---
name: Social Preview Generator
description: This skill should be used when the user asks to "generate social preview", "create social image", "github social preview", "repository preview image", "og image", "social media card", "create repo image", "github card image", or needs to create an image that captures a project's spirit and purpose for social media sharing. Analyzes project files to understand intent and generates optimized image prompts or images.
---

# Social Preview Image Generator

Generate social preview images for GitHub repositories by analyzing project intent, purpose, and spirit. Create images that capture what a project does and why it matters.

## Overview

This skill analyzes a project's codebase, documentation, and configuration to understand its purpose, then generates either:
1. A detailed image prompt (default, no configuration needed)
2. An actual image via configured provider (when settings exist)

## GitHub Image Requirements

All generated images must meet GitHub's social preview requirements:
- **Minimum**: 640x320 pixels
- **Recommended**: 1280x640 pixels (2:1 aspect ratio)
- **Maximum file size**: 1MB
- **Formats**: PNG (preferred), JPG, GIF
- **Default output**: `.github/social-preview.png`

## Workflow

### Step 1: Check for Configuration

Look for optional settings file at `.claude/github-social.local.md`.

If file exists, parse YAML frontmatter for:
- `provider`: Image generation provider (dalle-3, stable-diffusion, midjourney, manual)
- `api_key_env`: Environment variable name containing API key
- `style`: Visual style preference (abstract, illustrated, minimalist, custom)
- `output_path`: Where to save the image
- `dimensions`: Image dimensions (default: 1280x640)
- `include_text`: Whether to include project name in image
- `custom_style`: Custom style description if style is "custom"
- `colors`: Color scheme preference (auto, dark, light, custom)

If no config exists, proceed with defaults (prompt-only output).

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
   - **Abstract**: Geometric shapes, gradients, no literal representations
   - **Illustrated**: Hand-drawn feel, character, warmth
   - **Minimalist**: Project name prominent, subtle background elements
   - **Custom**: Follow user's custom_style description

### Step 4: Generate Image Prompt

Craft a detailed prompt optimized for image generation:

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

### Step 5: Generate or Output

**If provider is configured and API key available**:

For **DALL-E 3**:
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

Then decode and resize to 1280x640:
```bash
echo "[base64_data]" | base64 -d > temp_image.png
# Use ImageMagick or similar to resize
convert temp_image.png -resize 1280x640! -quality 90 .github/social-preview.png
```

For **Stable Diffusion** (local or API):
Construct appropriate API call based on user's SD setup.

For **manual/midjourney**:
Output the optimized prompt for user to copy.

**If no provider configured (default)**:
Output the detailed prompt with instructions:
1. Display the generated prompt
2. Explain it's optimized for DALL-E 3 but works with other tools
3. Suggest using the `/social-preview:setup` command for automated generation
4. Note the GitHub requirements (1280x640, <1MB)

### Step 6: Verify Output

If image was generated:
1. Verify file exists at output path
2. Check dimensions meet requirements (≥640x320)
3. Check file size is under 1MB
4. Report success with file location

If image exceeds 1MB:
- Compress with quality reduction
- Convert to optimized PNG
- Report final size

## Style Guide for Prompts

### Abstract Style
- Geometric shapes: circles, hexagons, flowing lines
- Gradients: smooth color transitions
- No literal objects or people
- Tech-inspired patterns: grids, circuits, nodes
- Example: "Abstract geometric composition with flowing gradient shapes..."

### Illustrated Style
- Hand-drawn aesthetic
- Warm, approachable feel
- Can include metaphorical imagery
- Textured, organic lines
- Example: "Hand-illustrated style artwork depicting..."

### Minimalist Style
- Maximum whitespace
- Single focal element
- Project name as primary element
- Subtle background texture or gradient
- Example: "Minimalist design with centered typography..."

## Color Palettes by Domain

**DevTools/CLI**: Dark backgrounds, terminal green, cyan accents
**AI/ML**: Purple gradients, neural blue, data visualization colors
**Web/Frontend**: Modern gradients, brand-friendly blues and purples
**Data/Analytics**: Chart colors, professional blues, accent oranges
**Security**: Deep blues, warning yellows, trust greens
**Infrastructure**: Cloud whites, server grays, network blues

## Error Handling

**No README or project files found**:
Ask user to describe the project purpose, then proceed with that information.

**API key not found**:
Fall back to prompt-only output with instructions.

**Image generation fails**:
1. Report the error
2. Output the prompt for manual use
3. Suggest checking API key or trying different provider

**File too large after generation**:
Apply progressive compression until under 1MB.

## Examples

### Example 1: CLI Tool Project

**Project analysis**:
- Name: "quicksort"
- Purpose: Fast file sorting utility
- Domain: DevTools/CLI
- Spirit: Speed, efficiency, simplicity

**Generated prompt**:
```
Abstract geometric composition with terminal-inspired aesthetics, flowing lines
suggesting rapid data movement, interconnected sorting nodes in a dynamic arrangement,
dark slate background with electric blue and cyan gradient accents, modern minimalist
tech style, centered composition with subtle depth, 1280x640, high contrast

Negative: text, watermarks, realistic elements, cluttered design
```

### Example 2: Machine Learning Library

**Project analysis**:
- Name: "neuralkit"
- Purpose: Neural network toolkit for researchers
- Domain: AI/ML
- Spirit: Powerful, accessible, innovative

**Generated prompt**:
```
Futuristic abstract visualization of neural network architecture, interconnected
nodes with glowing synaptic connections, deep purple to blue gradient background
with bright accent nodes, layered depth creating sense of complexity and power,
scientific yet approachable aesthetic, 1280x640, ethereal glow effects

Negative: human faces, realistic brains, cluttered, dated graphics
```

## Additional Resources

### Reference Files
- **`references/prompt-patterns.md`** - Detailed prompt patterns by domain
- **`references/provider-apis.md`** - API documentation for supported providers

### Example Files
- **`examples/config-dalle.md`** - Example DALL-E configuration
- **`examples/config-manual.md`** - Example manual/prompt-only configuration
