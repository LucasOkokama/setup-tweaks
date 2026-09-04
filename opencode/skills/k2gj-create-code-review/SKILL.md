---
name: k2gj-create-code-review
description:
  Perform evidence-based code reviews for a worktree, file, pull request, or
  entire project. Use when the user asks to review code or assess concerns such
  as correctness, bugs, security, performance, maintainability, architecture,
  tests, API compatibility, concurrency, or other explicit review criteria.
---

# K2GJ Create Code Review

## Objective

Perform a focused, evidence-based code review according to the requested target
and scope, producing an actionable Markdown report.

## Inputs & Parameters

The skill accepts:

- **Target:** Worktree, File, Pull Request, or Project.
- **Scope:** One or more concerns to analyze.
- Additional context, requirements, or constraints provided by the user.

If the target is not specified and cannot be inferred, ask the user to choose:

- Worktree
- File
- Pull Request
- Project

If the user explicitly requests the entire project, use `Project`.

If no scope is specified, use the default scope defined in `Rules & Scope`,
adapting it to the target.

## Rules

- **Default scope:** Correctness and bugs, Security, Performance,
  Maintainability, Architecture, Tests, API compatibility, Error handling,
  Concurrency and race conditions.
- Base findings on concrete evidence from the repository, changes, project
  context, or available external sources.
- Never fabricate behavior, evidence, or project conventions.
- Do not report speculative concerns as confirmed findings.
- Report concrete problems as Findings and optional improvements as Suggestions.
- Do not report stylistic preferences as findings unless they create a material
  problem.
- User-provided scope takes precedence over the default scope.
- Do not force irrelevant review categories onto the analysis.
- Always generate the review and all review-related content in
  English. Never generate the review report in another language, even if the
  user, repository, code, comments, commits, issues, or Pull Request use a
  different language.

## Workflow

### 1. Determine Target and Scope

Identify the target and requested scope. If the target cannot be inferred, ask
the user to choose one. If no scope is provided, use the default scope.

### 2. Establish Review Set

Determine the code that must be reviewed and, for change-based targets,
establish the appropriate baseline and changes before analyzing the code.

### 3. Gather Context

Inspect the target and relevant surrounding code needed to understand its
behavior, dependencies, contracts, and assumptions.

### 4. Analyze and Validate

Analyze the review set according to the selected scope. Validate each potential
finding against the available code and context before reporting it.

### 5. Generate Report

Generate the report using `assets/template.md` and save it according to the
`Output File` rules.

## Skill-Specific Sections

### Worktree Target

Review the changes currently present in the working tree, including:

- Staged changes.
- Unstaged changes.
- New files.
- Relevant context around modified files.

Focus on the current worktree changes rather than treating the entire repository
as newly introduced code.

### File Target

Review the requested file together with the context required to evaluate it.

Inspect relevant:

- Callers and callees.
- Interfaces and types.
- Tests.
- Related or equivalent implementations.
- Configuration.
- Nearby dependencies.

Do not limit the analysis to the file itself when additional context is
required.

### Pull Request Target

Review the changes introduced by the Pull Request or Merge Request, not merely
the final branch state.

For GitHub Pull Requests, use the **GitHub MCP Server** to retrieve the
repository and Pull Request information required for the review, including:

- Pull Request metadata.
- Base branch.
- All commits introduced by the PR.
- Changed files and their diff.

When reviewing the PR:

- Identify the repository and Pull Request.
- Establish the appropriate base.
- Review all commits and the complete diff.
- Inspect relevant surrounding context.
- Consider interactions between commits.
- Do not report pre-existing issues unless the PR materially affects them.

If the requested PR cannot be accessed through the GitHub MCP Server, report the
problem directly in the conversation instead of generating an incomplete review
report.

### Project Target

Review the project as a whole according to the selected scope.

Inspect the project structure and relevant components rather than restricting
the analysis to the currently open or explicitly mentioned files.

Prioritize relevant areas instead of treating every file as equally important.

### Review Scope

Treat Scope as the set of concerns the review should investigate.

Examples include:

- Correctness
- Bugs
- Security
- Performance
- Maintainability
- Architecture
- Tests
- API compatibility
- Concurrency and race conditions
- Error handling
- Observability

Do not report unrelated concerns merely because they are observable.

### Findings and Suggestions

A Finding is a concrete problem supported by sufficient evidence.

If no findings are identified, keep the `Findings` section and explicitly state
that no findings were identified.

Severity:

- `CRITICAL` — catastrophic security, data integrity, or system impact.
- `HIGH` — serious correctness, security, data, or operational impact.
- `MEDIUM` — meaningful problem with moderate impact or scope.
- `LOW` — minor but concrete problem worth addressing.

A Suggestion is a useful, non-blocking improvement that does not constitute a
concrete defect.

Every finding must have a unique sequential identifier such as `CR-001`.

## Evidence & Analysis

For every potential finding, verify:

1. What is wrong.
2. Where it occurs.
3. Why it matters.
4. What evidence supports it.
5. How it can reasonably be addressed.

For change-based reviews, distinguish newly introduced or materially affected
behavior from pre-existing behavior.

If a conclusion depends on an assumption, identify it in the report's
`Assumptions` section.

If the available evidence is insufficient to establish a concrete problem, do
not report it as a finding.

## Output Format

Generate the complete Markdown report using `assets/template.md` as the required
structure.

The report must:

- Include the target type and, when available, a clickable target reference.
- Use relative links for repository file references when appropriate.
- Distinguish Findings from Suggestions.
- Include accurate Review Coverage.
- Include assumptions and limitations when applicable.

## Output File

Generate the complete review report in the project root.

Use `k2gj-create-code-review.md` as the default filename.

Never overwrite an existing generated report. If the filename already exists,
append an incrementing numeric suffix:

- `k2gj-create-code-review-2.md`
- `k2gj-create-code-review-3.md`
- Continue until an unused filename is found.

Use `assets/template.md` as the report template. Do not modify the template
itself.

## Failure & Fallback

If the review cannot be reliably performed because required information, files,
repository state, external access, or an operation fails, explain the problem
directly in the conversation and do not generate a report.

Ask for clarification when essential information is missing.

Continue with optional missing context when a reliable review remains possible,
and document relevant assumptions or limitations in the report.

Never fabricate evidence to complete a report.

## Final Verification

Before completing the task:

- Confirm the target and scope were correctly identified.
- Confirm the appropriate baseline was used for change-based reviews.
- Confirm relevant context was inspected.
- Confirm every finding has concrete evidence and an accurate severity.
- Confirm file and line references are accurate.
- Confirm Review Coverage is accurate.
- Confirm the report follows `assets/template.md`.
- Confirm the output filename does not overwrite an existing report.
