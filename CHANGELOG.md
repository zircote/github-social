# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- **[social-preview]**: Mandatory JPG generation with SVG provider
  - SVG generation now ALWAYS produces both `.svg` and `.jpg` files
  - JPG is required for GitHub social preview (GitHub doesn't accept SVG)
  - JPG specs: 1280x640, quality 90, < 1MB
  - Both files uploaded when using `--upload` flag
- **[citation-cff]**: New skill for CITATION.cff file management
  - Create valid CITATION.cff from project metadata (any language ecosystem)
  - Validate against CFF 1.2.0 schema with error codes and fix suggestions
  - Sync version, date-released, authors, and keywords from project sources
  - Export citations as BibTeX, APA, RIS, CodeMeta, or EndNote
  - Enrich with ORCID iDs, DOIs, and preferred-citation blocks
  - Generate GitHub Actions workflow for CI validation via `dieghernan/cff-validator`
- **[/citation-cff]**: New command with `--init`, `--sync`, `--validate`, `--enrich`, `--export`, `--workflow` flags

### Fixed

- **[social-setup]**: Updated setup wizard with SVG-first provider options
  - SVG now shown as first (recommended) option
  - Added Gemini provider option
  - Removed deprecated stable-diffusion option
  - Added dark_mode and upload_to_repo configuration
- **[commands]**: Added explicit `name` fields to all command files
  - `github-social:social-all` and `github-social:social-setup` use prefixed names
  - Prevents conflicts with other plugins
- **[examples]**: Removed undocumented `style` field from AI provider examples
  - Only `svg_style` is a valid config option (for SVG provider)

## [0.5.0] - 2025-01-18

### Added

- **[social-preview]**: SVG generation as default provider (free, instant, editable)
  - Claude generates clean SVG graphics directly - no external API needed
  - Three SVG styles: `minimal` (default), `geometric`, `illustrated`
  - Domain-specific color palettes (DevTools, AI/ML, Web, Data, Security, Infrastructure, Plugin)
  - New reference file: `references/svg-templates.md` with complete templates by domain
  - Example configuration: `examples/config-svg.md`
- **[social-preview]**: Google Gemini (Nano Banana) image generation support
  - Models: `gemini-2.5-flash-image` (fast), `gemini-3-pro-image-preview` (quality)
  - Lower cost than DALL-E (~$0.039/image vs ~$0.08/image)
  - Example configuration: `examples/config-gemini.md`
- **[social-preview]**: Dark mode support with `<picture>` element for theme switching
  - Config option: `dark_mode: false | true | both`
  - Generates light and dark SVG variants when set to `both`
- **[readme-enhance]**: Collapsible prompt display pattern
  - Shows generation prompt in `<details>` section below infographic
  - Follows NotebookLM-style documentation pattern
- **[readme-enhance]**: SVG infographic generation as default
  - New reference file: `references/svg-infographic-patterns.md` with layout templates

### Changed

- **[social-preview]**: Default provider changed from DALL-E to SVG
- **[commands]**: Added `--provider` flag to override provider (svg, dalle-3, gemini, manual)
- **[commands]**: Added `--dark-mode` flag for dark mode variant generation
- Updated plugin description to reflect SVG-first approach
- Updated plugin version to 0.5.0

## [0.4.0] - 2025-01-18

### Added

- **[readme-enhance]**: New skill for README enhancement with marketing badges and infographics
  - Generates shields.io badges based on project analysis (version, build, license, downloads, language, etc.)
  - Creates NotebookLM-style infographic prompts capturing architecture and features
  - Hybrid infographic style adapts to project type (CLI, library, framework, app, infrastructure)
  - Direct README.md modification with intelligent badge section management
  - Comprehensive reference documentation for badge patterns and infographic prompts
- **[/readme-enhance]**: New command to enhance README with badges and infographic
  - Supports `--badges-only`, `--infographic-only`, and `--dry-run` flags
- **[/github-social:social-all]**: New command to run all skills in sequence
  - Executes repo-metadata, social-preview, and readme-enhance in order
  - Supports `--apply`, `--dry-run`, `--skip-badges`, and `--skip-infographic` flags
  - Shared project analysis for efficiency

### Changed

- Updated plugin version to 0.4.0
- Enhanced README documentation with new features and commands

## [0.3.0] - 2025-01-18

### Added

- **[social-preview]**: Added `upload_to_repo` option to automatically upload generated images to GitHub repository
  - Uses GitHub MCP tool for seamless integration
  - Configurable via `.claude/github-social.local.md`
- **[CI/CD]**: Added GitHub Actions workflow and templates
  - Pull request template
  - Copilot instructions

### Changed

- Improved social preview skill workflow documentation

## [0.2.0] - 2025-01-18

### Added

- **[repo-metadata]**: New skill for generating optimized repository descriptions and topics
  - SEO-friendly descriptions (max 350 chars)
  - Topic suggestions for discoverability (max 20 topics)
  - Apply changes via `gh` CLI or output for manual review
- **[/repo-metadata]**: New command with `--apply` flag for direct GitHub updates
- GitHub activity badges in README (stars, issues, last commit, PRs welcome)

### Changed

- Updated badge URLs for renamed repository

## [0.1.0] - 2025-01-18

### Added

- **[social-preview]**: Initial skill for generating GitHub social preview images
  - Analyzes project files (README, package.json, CLAUDE.md) to understand purpose
  - Generates detailed image prompts capturing project spirit
  - Supports multiple providers (DALL-E, Stable Diffusion, Midjourney, manual)
  - Outputs images meeting GitHub requirements (1280x640px, <1MB)
- **[/social-preview]**: Command to generate social preview images
- **[/github-social:social-setup]**: Interactive setup wizard for configuration
- Configuration via `.claude/github-social.local.md` with YAML frontmatter
- MIT License

[Unreleased]: https://github.com/zircote/github-social/compare/v0.5.0...HEAD
[0.5.0]: https://github.com/zircote/github-social/compare/v0.4.0...v0.5.0
[0.4.0]: https://github.com/zircote/github-social/compare/v0.3.0...v0.4.0
[0.3.0]: https://github.com/zircote/github-social/compare/v0.2.0...v0.3.0
[0.2.0]: https://github.com/zircote/github-social/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/zircote/github-social/releases/tag/v0.1.0
