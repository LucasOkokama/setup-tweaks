---
name: k2gj-create-pull-request
description:
  Create a GitHub pull request from the current repository changes. Use when the
  user asks to create, prepare, or submit a pull request, including determining
  the appropriate base branch and generating a concise title and structured body
  from the available repository context, changes, and related issues.
---

# K2GJ Create Pull Request

## Objective

Create a GitHub pull request that accurately represents the current changes,
using a consistent title and body structure while avoiding unsupported or
fabricated information.

## Inputs & Parameters

The skill may use:

- The current repository and its Git state.
- The current branch.
- The appropriate base branch.
- User-provided context, requirements, or issue references.
- Relevant commits and repository changes.
- Existing pull request information when updating an existing PR.

## Rules & Scope

- Create or update pull requests only when explicitly requested or clearly
  implied by the user's request.
- When an existing pull request for the current branch is identified, update it
  only when the user's request refers to that pull request or clearly requests
  an update. Do not modify an existing pull request merely because one exists.
- By default, create the pull request from the current branch to its appropriate
  base branch, including all committed changes relative to that base.
- Do not include uncommitted working-tree changes or modify them in any way.
- Do not create commits or push changes automatically.
- Before creating the pull request, verify that the current branch and its
  commits are available on the remote. If commits have not been pushed, stop and
  inform the user.
- Base the PR content on repository evidence and user-provided context. Never
  invent repository facts, test results, issue references, or breaking changes.
- Omit optional sections when they are not applicable.
- All pull request content must be written in English, including the title,
  body, section headings, bullet points, and issue references when applicable.

## Workflow

### 1. Inspect Repository Context

Inspect the current repository, branch, committed history, and GitHub context
needed to create or update the pull request.

Determine the appropriate base branch using available repository and GitHub
context, such as the repository's default branch, existing pull requests, branch
relationships, and repository conventions. Do not assume a base branch when the
available evidence is ambiguous.

### 2. Analyze the Changes

Analyze the committed changes in the current branch relative to its base branch.

Identify:

- The primary outcome of the PR.
- The motivation or problem being addressed.
- The meaningful changes introduced.
- Any breaking changes.
- Any related issues or tickets supported by the available context.

### 3. Generate PR Content

Generate and validate the PR title and body according to the conventions defined
in the skill-specific sections.

All generated pull request content must be written in English.

Repository content, code, commit messages, issue titles, or other source
material may be in another language, but the resulting pull request content must
always be in English.

### 4. Create or Update the PR

Use the appropriate GitHub MCP operation (`create_pull_request` or
`update_pull_request`) with the repository, branches, title, and body determined
above.

## Skill-Specific Sections

### PR Title

Use a concise title following the Conventional Commits style:

```text
<type>(<scope>): <description>
```

The title must describe the overall purpose and outcome of the pull request, not
reproduce the title of any individual commit.

When the PR contains multiple commits, derive the title from the combined
changes and their primary objective.

Use an appropriate type such as `feat`, `fix`, `refactor`, `docs`, `test`,
`perf`, `build`, `ci`, `chore`, or `revert`.

The scope is optional. Use `!` when the PR introduces a breaking change.

Keep the title concise and focused on the meaningful outcome of the PR.

### PR Body

Use the following structure:

```markdown
## Summary

[Briefly describe what this PR does and the main outcome]

## Motivation (optional)

[Explain why this change is needed and what problem or goal it addresses]

## Changes

[Summarize the main changes made in this PR, focusing on meaningful changes
rather than files or implementation details]

- Change description 1
- Change description 2
- Change description 3

## Breaking Changes (optional)

[Describe any breaking changes, their impact, and what consumers need to do to
adapt]

- Breaking Change description 1
- Breaking Change description 2
- Breaking Change description 3

## Related Issues (optional)

[Reference related issues, tickets, or discussions using GitHub issue references
when possible]

- Closes #123
- Related to #456
```

`Summary` should be a short paragraph explaining what the PR does and its main
outcome.

`Motivation` should be a short paragraph explaining why the change is needed.
Omit it when the motivation is not available or adds no meaningful context.

`Changes` should be a simple bullet list of meaningful changes. Do not create a
heading for each individual change.

`Breaking Changes` should only be included when the change affects existing
consumers, public interfaces, APIs, configurations, or other externally
relied-upon behavior. Describe the impact and what consumers need to do to
adapt.

`Related Issues` should use GitHub issue references such as `Closes #123`,
`Fixes #123`, or `Related to #123` when the relationship is known. Omit the
section when there are no relevant references.

### PR Content Accuracy

Base the PR title and body on repository evidence, user-provided context, and
relevant GitHub information.

- Describe what the changes demonstrably do.
- Include motivation only when supported by available context.
- Report testing only when there is evidence that the tests were actually run.
- Identify breaking changes based on observable impact or externally relied-upon
  behavior.
- Include issue references only when their relationship to the PR is known.
- When information is unavailable, omit it rather than fabricate it.

### GitHub MCP Operations

Use the GitHub MCP Server for pull request operations:

- `create_pull_request` for new PRs.
- `update_pull_request` for existing PRs.
- Use the repository, branches, title, and body determined from the repository
  context, following the tool schema for required parameters.

## Evidence & Analysis

Prioritize evidence in this order:

1. User-provided requirements and issue context.
2. Committed changes in the current branch relative to its base branch.
3. Relevant repository and GitHub context, including existing PRs and issues.
4. Relevant commits and commit history.
5. Existing repository conventions and configuration.

Use the available evidence to infer the purpose and scope of the changes, but do
not present an inference as a confirmed fact when the evidence is insufficient.

A breaking change must be supported by an observable change to behavior or an
externally relied-upon interface.

Only include testing information when there is evidence that the tests were
actually executed.

## Output Format

After successfully creating or updating the pull request, report:

- The pull request title.
- A concise summary of the created or updated PR.
- The pull request URL, when provided by the GitHub tool.

Do not reproduce the entire PR body unless the user asks for it.

If the pull request cannot be created or updated, clearly explain the failure
and identify what prevented completion.

## Failure & Fallback

If required repository or branch information cannot be determined, inspect the
repository and available GitHub context before asking the user.

If the intended base branch cannot be determined reliably, ask the user instead
of guessing.

If the changes cannot be understood sufficiently to produce an accurate PR, ask
for the missing context or clearly explain what could not be determined.

If the GitHub MCP operation fails, report the failure without claiming that the
PR was created or updated.

If optional information, such as related issues or motivation, is unavailable,
omit the corresponding optional section rather than inventing content.

## Final Verification

Before considering the task complete:

- Confirm the PR title follows the required naming convention.
- Confirm the PR body follows the defined structure.
- Confirm `Summary` accurately describes the main outcome.
- Confirm `Changes` describe meaningful changes rather than merely listing
  files.
- Confirm optional sections are included only when applicable and contain no
  invented information.
- Confirm all issue references are supported by available repository or GitHub
  context.
- Confirm testing claims are supported by actual evidence.
- Confirm the GitHub tool explicitly reported a successful PR creation or
  update.
- If the PR URL is available, return it in the final response.
