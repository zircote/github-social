# Example Configuration: DALL-E 3

This example shows a complete configuration for automated image generation using OpenAI's DALL-E 3.

## Configuration File

Create `.claude/github-social.local.md` in your project:

```yaml
---
provider: dalle-3
api_key_env: OPENAI_API_KEY
style: abstract
output_path: .github/social-preview.png
dimensions: 1280x640
include_text: false
colors: auto
---

# Project Brand Guidelines (Optional)

Additional context for image generation:

## Color Preferences
- Primary: Deep blue (#1e40af)
- Accent: Cyan (#06b6d4)
- Background: Dark slate (#0f172a)

## Style Notes
- Prefer geometric over organic shapes
- Modern, professional aesthetic
- No gradients with more than 3 colors
```

## Environment Setup

Ensure your OpenAI API key is available:

```bash
# Add to ~/.bashrc, ~/.zshrc, or shell profile
export OPENAI_API_KEY="sk-your-key-here"
```

## Usage

After setup, simply run:

```
/social-preview
```

Or ask Claude:

```
Generate a social preview image for this project
```

## Expected Behavior

1. Claude analyzes your project files
2. Generates an image prompt based on project purpose
3. Calls DALL-E 3 API with the prompt
4. Downloads and resizes image to 1280x640
5. Saves to `.github/social-preview.png`
6. Reports success with file details

## Cost

Each generation costs approximately $0.08 (HD quality).

## Troubleshooting

**"OPENAI_API_KEY not found"**
- Verify the environment variable is set: `echo $OPENAI_API_KEY`
- Restart your terminal after adding to shell profile

**"Content policy violation"**
- The generated prompt may contain flagged content
- Claude will retry with a modified prompt
- If persistent, try changing style to "minimalist"

**"Rate limit exceeded"**
- Wait a moment and try again
- Check your OpenAI usage limits
