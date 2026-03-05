---
name: citation-cff
description: Create, validate, update, and export CITATION.cff files for academic citation of software
argument-hint: "[--init] [--sync] [--validate] [--enrich] [--export bibtex|apa|ris|codemeta] [--workflow] [--apply] [--dry-run]"
allowed-tools: ["Read", "Write", "Edit", "Glob", "Grep", "Bash", "Skill"]
---

Manage CITATION.cff files for academic citation of software repositories, supporting creation, validation, synchronization, enrichment, and export to common citation formats.

## Task

Analyze this project and manage its CITATION.cff file — creating, validating, syncing, enriching, or exporting citations as requested.

## Process

1. Use the citation-cff skill to analyze the project
2. Parse `$ARGUMENTS` for flags to determine which operation(s) to perform
3. Execute the requested operation (or default behavior if no flags)
4. Present results and next steps

## Flags

### `--init`

Create a new CITATION.cff from project metadata:
- Read package manifest (package.json, Cargo.toml, pyproject.toml, etc.), README, and git history
- Extract title, version, authors, license, repository URL, description, keywords
- Generate a CFF 1.2.0-compliant file
- Output the file for review (or write directly if `--apply` is also set)

### `--sync`

Synchronize CITATION.cff with current project sources:
- Compare version, date-released, authors, and keywords against package manifest and git history
- Report any fields that are out of date
- Update stale fields in the CITATION.cff
- Requires an existing CITATION.cff (error if not found)

### `--validate`

Validate an existing CITATION.cff against the CFF 1.2.0 schema:
- Check required fields (cff-version, message, title, authors, type)
- Validate field types, date formats, URL formats, SPDX license identifiers
- Report errors and warnings with line numbers
- Exit with success/failure status

### `--enrich`

Add academic metadata to an existing CITATION.cff:
- Look up and add ORCID identifiers for authors
- Add DOI if available (Zenodo, npm, crates.io)
- Add identifiers (DOI, URL, SWMH) and references
- Add or update preferred-citation block
- Requires an existing CITATION.cff (error if not found)

### `--export FORMAT`

Export citation to a standard format. Supported formats:
- `bibtex` — BibTeX (.bib)
- `apa` — APA 7th edition plain text
- `ris` — RIS (.ris)
- `codemeta` — CodeMeta JSON-LD (codemeta.json)
- `endnote` — EndNote XML

Output is written to stdout (or to a file if `--apply` is set).

### `--workflow`

Generate a GitHub Actions workflow for automatic CFF validation:
- Create `.github/workflows/cff-validate.yml`
- Triggered on push/PR when CITATION.cff changes
- Uses `citation-file-format/cffconvert` for validation
- Output the workflow file for review (or write directly if `--apply`)

### `--apply`

Write changes to disk without prompting for confirmation. Applies to `--init`, `--sync`, `--enrich`, `--workflow`, and `--export`.

### `--dry-run`

Show what would change without writing any files. Applies to all operations that modify or create files.

## Default Behavior

When no flags are provided:
1. Check if `CITATION.cff` exists in the repository root
2. **If it exists**: run validation (`--validate` behavior)
3. **If it does not exist**: run initialization (`--init` behavior) and present the generated file for review

## Output Format

Display results with:
- Operation performed (init, sync, validate, enrich, export, workflow)
- File path(s) created or modified
- Summary of changes or validation results
- Errors and warnings (if any), with line numbers where applicable
- Next steps (e.g., "Run `--enrich` to add DOI and ORCID metadata")
- CLI commands for applying changes manually if `--apply` was not used

## Example Usage

```bash
# Auto-detect: validate existing or create new CITATION.cff
/citation-cff

# Create new CITATION.cff from project metadata
/citation-cff --init

# Create and write immediately
/citation-cff --init --apply

# Preview what init would generate
/citation-cff --init --dry-run

# Sync version and authors from package manifest
/citation-cff --sync --apply

# Validate against CFF 1.2.0 schema
/citation-cff --validate

# Add ORCID, DOI, and preferred-citation
/citation-cff --enrich --apply

# Export as BibTeX
/citation-cff --export bibtex

# Export as CodeMeta JSON-LD and write to file
/citation-cff --export codemeta --apply

# Generate CI validation workflow
/citation-cff --workflow --apply
```

## Tips

- Run `/github-social:social-all` to run all enhancement commands in sequence
- CITATION.cff makes your software citable via GitHub's "Cite this repository" button
- Use `--validate` in CI to catch citation metadata drift
- `--sync` is useful after version bumps to keep citation metadata current
- `--enrich` adds academic credibility with ORCID and DOI identifiers
- CodeMeta export (`--export codemeta`) improves discoverability in academic search engines
