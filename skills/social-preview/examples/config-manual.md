# Example Configuration: Manual/Prompt-Only

This example shows configuration for prompt-only output, where you manually use the generated prompt with your preferred image tool.

## Configuration File

Create `.claude/github-social.local.md` in your project:

```yaml
---
provider: manual
style: illustrated
output_path: .github/social-preview.png
dimensions: 1280x640
include_text: true
text_content: auto
colors: light
---

# Style Preferences

## Visual Direction
- Hand-drawn, artistic feel
- Warm and approachable
- Not too corporate or sterile

## Avoid
- Stock photo aesthetics
- Generic tech imagery
- Overly complex compositions
```

## Usage

Run the command:

```
/social-preview
```

Or ask Claude:

```
Create a social preview image prompt for this project
```

## Expected Output

Claude will output:

1. **Analysis Summary** - What was learned about your project
2. **Image Prompt** - Detailed prompt optimized for image generation
3. **Negative Prompt** - Elements to avoid
4. **Instructions** - How to use with different tools

### Example Output

```
## Project Analysis

- **Name**: awesome-cli
- **Purpose**: Command-line productivity toolkit
- **Domain**: DevTools
- **Spirit**: Fast, minimal, developer-focused

## Generated Image Prompt

Hand-illustrated style artwork depicting a streamlined command-line interface,
warm organic lines suggesting efficient workflows and productivity,
light cream background (#faf7f2) with terracotta (#c2410c) and sage (#84cc16) accents,
centered composition with "awesome-cli" text integrated naturally,
approachable developer aesthetic with artistic warmth,
1280x640, textured feel, friendly professional

## Negative Prompt

Photorealistic, cold, corporate, cluttered, dark, intimidating, generic stock

## Using This Prompt

### DALL-E 3
Paste the prompt directly into ChatGPT or DALL-E interface.

### Midjourney
Use: /imagine [prompt] --ar 2:1 --q 2 --stylize 500

### Stable Diffusion
Use prompt as positive, negative prompt as negative.
Set dimensions to 1280x640 or 1344x704.

### Adobe Firefly
Paste prompt, select "Art" style category.
```

## When to Use Manual Mode

- You have a preferred image generation tool
- You want to iterate on the prompt before generating
- You don't have API keys set up
- You want to use Midjourney (no API available)
- You want more control over the final output

## Tips

1. **Save good prompts** - If you like a generated prompt, save it for future use
2. **Iterate** - Modify the prompt based on initial results
3. **Combine tools** - Generate with one tool, refine with another
4. **Upscale** - Use AI upscalers if initial output is lower resolution
