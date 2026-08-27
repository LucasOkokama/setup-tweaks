---
name: k2gj-commit-message
description:
  Generate, review, and correct Conventional Commits messages from diffs,
  modified files, change descriptions, or existing commit messages. Use when the
  user needs to create, review, classify, or format a commit message, including
  determining its type, optional scope, concise description, and breaking-change
  notation.
---

# K2GJ Commit Message

## Objective

Produce a clear, concise, and semantically accurate commit message that follows
the Conventional Commits specification and faithfully represents the primary
change.

## Inputs & Parameters

The skill may receive:

- A diff or patch.
- A list of modified files.
- A description of the changes.
- Context about the purpose or intent of the change.
- An existing commit message to review or correct.
- Optional information about scope, breaking changes, or user intent.
- A project or repository whose conventions can provide contextual evidence.

If no specific files, diff, change description, or scope are provided, inspect
the available project context when possible rather than assuming a scope.

## Rules & Scope

- Follow the Conventional Commits header structure: `type(scope): description`.
- Treat `scope` as optional and omit it when no clear scope is supported.
- Use a semantic type that represents the primary purpose of the change.
- Keep the description concise, specific, and in the imperative mood.
- Represent the change accurately rather than optimizing for a preferred commit
  type.
- Do not invent intent, scope, behavior, compatibility impact, or implementation
  details.
- Treat a change as breaking only when it introduces meaningful incompatibility
  for existing consumers, users, or integrations.
- Indicate a confirmed breaking change with `!` in the header and add a
  `BREAKING CHANGE:` footer when additional explanation is necessary.
- Do not classify a change as breaking merely because it is large, complex, or
  internally refactored.
- Do not include issue, ticket, or external references unless explicitly
  requested or required by the available project convention.
- Do not add a body or footer unless it materially improves the commit message.
- If classification remains uncertain but sufficient evidence exists to produce
  a reasonable result, choose the most defensible classification and preserve
  the uncertainty only when it materially affects the result.

## Workflow

### 1. Identify the Primary Change

Inspect the available evidence and determine what actually changed and what
outcome the change primarily produces.

Prioritize behavioral and functional intent over the number or names of modified
files.

### 2. Classify the Commit Type

Select the type that best represents the primary intent.

Use these standard types when applicable:

- `feat`: introduces a new feature or capability.
- `fix`: corrects incorrect or unintended behavior.
- `docs`: changes documentation without changing product behavior.
- `style`: changes formatting or style without changing behavior.
- `refactor`: restructures code without adding a feature or fixing a bug.
- `perf`: improves performance.
- `test`: adds or modifies tests without a corresponding functional change.
- `build`: changes the build system or build-related dependencies.
- `ci`: changes continuous-integration configuration or processes.
- `chore`: maintenance work that does not better fit another type.

Choose exactly one primary type. When multiple types apply, classify the change
according to its dominant purpose.

### 3. Determine the Scope

Identify a scope only when the change clearly belongs to a recognizable
component, module, package, layer, or domain.

Prefer the project's established terminology when it is observable. Keep the
scope short and omit it when the change spans areas without a clear primary
domain.

### 4. Write the Header

Write a concise imperative description that communicates the intended result of
the change.

Prefer:

`fix(auth): handle expired access tokens`

over a generic description such as:

`fix(auth): update authentication code`

Use `type: description` when no scope is justified.

### 5. Assess Compatibility

Determine whether the change introduces meaningful incompatibility for existing
consumers, users, or integrations.

Consider:

- Incompatible changes to public APIs.
- Removal or incompatible modification of existing functionality.
- Changes to contracts consumed by other components.
- Changes that require mandatory modifications from consumers.

For a confirmed breaking change, use `!` after the type or scope, for example:

`feat(api)!: remove legacy authentication endpoint`

Add a `BREAKING CHANGE:` footer only when the impact requires explanation.

### 6. Validate the Result

Check the complete message against the Conventional Commits structure and the
available evidence. Remove unnecessary body, footer, or metadata before
returning the result.

## Skill-Specific Sections

### Commit Type Semantics

Classify commits according to the primary semantic effect rather than the amount
of code changed.

A refactor that changes no externally observable behavior remains `refactor`; a
bug correction remains `fix` even when it requires substantial restructuring.
Use `chore` only when a more specific applicable type does not describe the
change better.

### Scope Selection

Treat scope as a useful qualifier, not a required field.

Derive scopes from recognizable project concepts such as packages, modules,
subsystems, domains, or layers. Prefer existing project terminology over
inventing a new label. Omit the scope when evidence does not establish a clear
and meaningful one.

### Breaking-Change Notation

Use breaking-change notation only for actual compatibility impact.

Place `!` immediately before the colon:

`feat(api)!: remove legacy endpoint`

When consumers need to know what must change, provide a concise
`BREAKING CHANGE:` footer that describes the compatibility impact. Do not use
breaking notation merely to emphasize a significant change.

### Commit Header Style

Keep the header focused on the change's outcome.

Use imperative wording, avoid unnecessary punctuation, and prefer specific verbs
over vague implementation-oriented phrases. Do not encode details in the header
that are not necessary to distinguish or understand the primary change.

## Evidence & Analysis

Evaluate evidence in the following order:

1. Actual diffs or implemented changes.
2. Explicit user-provided context about the change.
3. Modified-file names and project structure.
4. Observable project conventions.
5. Reasonable inferences supported by the available evidence.

When user-provided intent conflicts with observable implementation, investigate
the inconsistency before deciding. Do not treat branch names, file names, or
previous commit messages as sufficient evidence of intent by themselves.

When evidence is insufficient for a confident classification, prefer the most
conservative defensible interpretation. Never fabricate missing context.

## Output Format

Return the final commit message without Markdown code fences or explanatory
commentary unless the user explicitly requests analysis or alternatives.

Use one of these header forms:

- `type(scope): description`
- `type: description`
- `type(scope)!: description`
- `type!: description`

When a body or footer is necessary, use the Conventional Commits structure:

```text
type(scope): description

body

BREAKING CHANGE: explanation
```

Do not add a body or footer by default.

## Output File

Use this section only when the execution environment explicitly supports file
creation and the user has requested or authorized file output.

Generate a plain-text file named `k2gj-commit-message.txt` in the project root.
Do not overwrite an existing file. If the name is already occupied, use
`k2gj-commit-message-2.txt` and increment the numeric suffix until an unused
filename is found.

The file must contain only the final commit message. Do not create additional
files.

## Failure & Fallback

- If essential information is missing and no reasonable classification can be
  supported, request the diff, change description, or other necessary context.
- If sufficient evidence exists for a reasonable classification, proceed without
  blocking on optional information.
- If two classifications are genuinely indistinguishable and the distinction
  would materially change the message, ask for clarification.
- If breaking-change evidence is conflicting, do not declare the change breaking
  without sufficient support.
- If a user-provided commit message violates Conventional Commits, correct its
  structure while preserving the meaning of the underlying change.
- Never invent details to populate a type, scope, description, body, or footer.

## Final Verification

Before considering the task complete, verify that:

- The selected type accurately represents the primary change.
- The scope, when present, is supported by the available evidence.
- The description is concise, specific, and imperative.
- The message accurately represents the implemented change.
- Breaking-change notation is used only when justified.
- No unsupported information has been introduced.
- No unnecessary body, footer, or references have been added.
- The final message conforms to the Conventional Commits structure.
- Any generated output file contains exactly the final commit message and does
  not overwrite an existing file.
