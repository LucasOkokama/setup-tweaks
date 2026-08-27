---
name: k2gj-generate-readme
description:
  Generate or update a project README draft from repository evidence using
  assets/template.md as the authoritative template. Use when documenting,
  regenerating, or updating a project's README. Never create or overwrite
  README.md; write only k2gj-generate-readme*.md.
---

# K2GJ Generate README

## Objective

Generate an accurate, project-specific README draft by inspecting the repository
and populating `assets/template.md`.

Preserve the template's structure, section order, formatting, and visual
conventions while replacing placeholders with verified project information.

## Inputs & Parameters

Required:

- Project repository/directory.
- `assets/template.md`.

Optional:

- Existing root `README.md`.
- Source, configuration, manifests, lockfiles, CI/CD, deployment files,
  documentation, assets, license, release metadata, and user instructions.

## Rules & Scope

- Treat `assets/template.md` as authoritative for README structure and
  presentation.
- Inspect the repository before generating content.
- Prefer current executable evidence over documentation or existing README
  claims.
- Never invent technologies, commands, configuration, features, releases,
  issues, roadmap items, references, or acknowledgements.
- Preserve concise placeholders when reliable information is unavailable.
- Existing `README.md` is contextual input, not authoritative over contradictory
  implementation evidence.
- Keep generated content in English unless explicitly requested otherwise.
- Never create, overwrite, or modify `README.md`.
- Never overwrite an existing `k2gj-generate-readme*.md`.
- Do not add sections, badges, or technologies outside the template's defined
  structure.

## Workflow

### 1. Inspect Template

Read `assets/template.md` first. Identify its required sections, formatting
conventions, placeholders, badge structure, image layout, code blocks, tables,
and special-section rules.

### 2. Inspect Repository

Prioritize source/configuration and manifests, then tests, tooling, CI/CD,
deployment, documentation, assets, license, releases, and existing README.

Determine only what can be established reliably.

### 3. Reconcile Evidence

Compare existing README information with repository evidence. Preserve useful
manually authored context when it remains valid; prefer current implementation
when sources conflict.

### 4. Generate

Populate the canonical template using actual project values and commands. Keep
unavailable or uncertain information as concise placeholders.

### 5. Validate and Write

Validate badges, images, links, commands, tables, and template-specific
sections, then write the first available `k2gj-generate-readme*.md` in the
project root.

## Skill-Specific Sections

### Badge Rules

The header contains exactly these eight categories, in order:

1. Frontend framework
2. Backend framework
3. Database
4. Format
5. Lint
6. Hosting
7. License
8. Release

Use official ShieldCN documentation to verify brands, logos, parameters, and
syntax.

- Every header badge uses `size=xs`, SVG, `<picture>`, dark `<source>`, light
  `<img>`, and exact visible-text `alt`.
- Use verified `brand` identifiers only; otherwise use verified colors/logos or
  omit the logo.
- Inapplicable badges remain commented-out template badges.
- License and Release are static badges without `brand` or `variant=branded`.
- License uses the actual identified license and links to its repository license
  file when available.
- Release uses an actual identified release/version and links to the repository
  releases page.
- Tech Stack badges use `img.shields.io` and only materially relevant
  technologies.

### Images & Showcase

Preserve the template's Showcase structure.

- Prefer real project screenshots, GIFs, or other repository assets.
- The top banner and Showcase images should use real project assets when
  suitable assets exist.
- Repository-hosted images must use `raw.githubusercontent.com` as the clickable
  destination, never `github.com/.../blob/...`.
- Use the repository's actual default branch.
- Image links use `target="_blank"`.
- Preserve meaningful `alt` and `title` text.
- If a required visual is unavailable, use the template's prescribed
  `images.placeholders.dev` dimensions and contextual English `text`.
- Do not present a placeholder as an actual project screenshot.

### Template-Specific Content

Preserve all template sections and their intended roles:

- **Overview:** concise project purpose, problem, and context.
- **Features:** choose either detailed feature subsections or a concise feature
  list according to project complexity.
- **Tech Stack:** use only technologies materially relevant to development or
  operation.
- **Project Structure:** reflect the actual repository structure and explain
  important directories/files.
- **Requirements:** list only verified prerequisites and relevant versions.
- **Installation:** numbered sequential setup steps with verified commands.
- **Configuration:** use exactly the template's five-column table:
  `Variable | Description | Required | Default | Example`. Document only real
  configuration and never expose secrets.
- **Usage:** describe post-installation user workflows, commands, or
  interactions; do not duplicate Development instructions.
- **Development:** local development workflows only.
- **Build:** production/distributable artifact generation only.
- **Deployment:** publishing/operating the project only.
- **References:** authoritative resources ordered by relevance, without category
  headings.
- **Known Issues:** only documented or repository-confirmed issues.
- **Roadmap:** placeholder unless concrete roadmap items exist in the existing
  README.
- **License:** use reliable license evidence and preserve the template's
  licensing presentation.
- **Acknowledgements:** placeholder unless meaningful existing README
  acknowledgements remain valid.

## Evidence & Analysis

Use evidence in this priority:

1. Source code and executable configuration.
2. Manifests and lockfiles.
3. Build/test/lint/deployment/CI configuration.
4. Environment and project configuration.
5. Documentation.
6. Existing README.
7. Git/repository metadata.
8. Corroborated inference.

Verify:

- Commands against actual scripts/configuration.
- Technologies against actual usage, not merely installed dependencies.
- Requirements against project configuration and documentation.
- Images against existing repository assets.
- Databases against dependencies, configuration, schema/migrations, or
  connection code.
- Releases against actual repository releases.
- License against `LICENSE.md` or other reliable project evidence.

When material uncertainty remains, do not assert the disputed information.

## Output Format

Produce the complete Markdown README defined by `assets/template.md`.

Preserve:

- Title and header/badge layout.
- All template sections and their order.
- Showcase structure.
- Tech Stack table.
- Installation, Configuration, Usage, Development, Build, and Deployment
  conventions.
- References, Known Issues, Roadmap, License, and Acknowledgements sections.

Replace placeholders with verified project information and retain concise
placeholders where evidence is unavailable.

## Output File

Create the first unused filename in the project root:

```text
k2gj-generate-readme.md
k2gj-generate-readme-2.md
k2gj-generate-readme-3.md
...
```

Never overwrite or modify `README.md` or an existing generated README.

## Failure & Fallback

- Missing `assets/template.md`: report the missing canonical template and stop.
- Uninspectable repository: report insufficient repository context; do not
  fabricate.
- Missing optional evidence: continue using reliable available evidence.
- Unverified badge brand/logo: use a verified fallback or omit the logo.
- Missing image: use the prescribed placeholder.
- Undetermined command or requirement: use a placeholder rather than inventing
  one.
- Unresolved contradiction: prefer current authoritative evidence or use a
  placeholder.
- Partial output is acceptable only when clearly incomplete and still useful.

## Final Verification

Before completion, confirm:

- `assets/template.md` was used and its section order preserved.
- Repository evidence was inspected and unsupported claims were excluded.
- `README.md` was untouched.
- Output filename follows the unused `k2gj-generate-readme[-N].md` convention.
- All template sections are present and correctly populated.
- Features use the appropriate template format.
- Project Structure and Requirements reflect the actual repository.
- Configuration uses the template's five-column table, including `Example`.
- Usage is distinct from Development, Build, and Deployment.
- Commands and configuration are real.
- Exactly eight header badge categories appear in the required order.
- Badge structure, values, links, modes, brands, and exact `alt` text are valid.
- License and Release badges follow their static rules.
- Tech Stack badges use `img.shields.io` and only relevant technologies.
- Repository images use valid raw GitHub destinations and the actual default
  branch.
- Placeholder images use the prescribed dimensions and contextual English text.
- Roadmap, Known Issues, License, Acknowledgements, and References follow their
  specific rules.
- Final Markdown is coherent, template-consistent, and free of unsupported
  project facts.
