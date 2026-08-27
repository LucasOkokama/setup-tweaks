---
name: k2gj-dead-code
description:
  Identify and report potentially unused or unreachable project code using
  repository evidence. Use when asked to find, analyze, audit, or report dead
  code. Produces a Markdown report with locations, evidence, caveats, and
  confidence levels.
---

# K2GJ Dead Code

## Objective

Analyze authored project code to identify constructs with no established
references or execution paths.

Account for static references, entry points, framework conventions, dynamic
invocation, registrations, configuration, public APIs, and external consumers
before classifying code as dead.

## Inputs & Parameters

- Project repository or directory.
- Optional analysis scope such as `src`, a package, or specific files.
- Optional context about frameworks, generated code, entry points, or runtime
  behavior.

If no scope is provided, analyze authored project source and exclude generated,
vendored, and dependency-managed code.

## Rules & Scope

- Inspect the actual project before reporting dead code.
- Distinguish **unused code** (no identified consumers) from **unreachable
  code** (no identified execution path).
- Consider imports, exports, re-exports, inheritance, overrides, callbacks,
  registrations, routes, dependency injection, decorators, reflection,
  configuration, and framework conventions.
- Treat public APIs, exports, plugins, CLI interfaces, and framework-discovered
  code conservatively.
- Treat unresolved dynamic or external usage as uncertainty, not proof of dead
  code.
- Do not report comments, documentation examples, fixtures, generated artifacts,
  vendored code, or dependencies unless explicitly in scope.
- Never modify, delete, refactor, or automatically fix code.
- Prefer false negatives over unsupported dead-code claims.
- Report the smallest meaningful source construct.
- Never fabricate source, references, paths, execution paths, or evidence.

Confidence:

- **High:** strong evidence after relevant dynamic/framework/external mechanisms
  are reasonably ruled out.
- **Medium:** substantial evidence exists, but a known limitation remains.
- **Low:** code appears unused/unreachable, but important usage mechanisms
  cannot be ruled out.

## Workflow

### 1. Establish Scope

Identify the project root, requested scope, source directories, package
boundaries, entry points, module system, framework configuration, and
generated/vendored areas.

### 2. Identify Candidates

Find relevant executable constructs such as functions, methods, variables,
classes, exports, modules, routes, handlers, callbacks, components, branches,
and unreachable statements.

### 3. Trace Usage

Build reference and reachability evidence using structural analysis when
available. Consider direct references, imports/exports, inheritance,
registrations, configuration, framework discovery, dynamic lookup, tests, and
public boundaries.

### 4. Validate Findings

Classify only candidates supported by evidence. Assign a confidence level and
record meaningful unresolved usage mechanisms.

### 5. Generate Report

For each validated finding, record the repository-relative path, smallest useful
source range, actual source code, evidence, confidence, and caveat when
necessary.

Calculate summary totals only from final validated findings.

## Skill-Specific Sections

### Reference Analysis

Treat references as relationships rather than simple text matches. Trace
imports, exports, call sites, inheritance, overrides, registrations, callbacks,
configuration mappings, and relevant transitive paths.

Prefer structural/language-aware evidence when it conflicts with textual search.

### Reachability Analysis

Establish application, package, test, CLI, and framework entry points before
judging reachability.

For branches and statements, require evidence that no valid execution path
reaches the reported code. Do not confuse rarely executed code with unreachable
code.

### Dynamic & Framework Usage

Inspect dependency injection, decorators, reflection, route discovery, plugin
registration, generated registries, configuration lookups, event systems,
serialization hooks, and convention-based loading.

If behavior cannot be established, lower confidence or omit the finding.

### External Boundaries

Assume public exports, package entry points, libraries, plugins, CLI interfaces,
and framework-discovered surfaces may have external consumers.

Do not call them definitively dead merely because no internal reference exists.

### Finding Precision

Report the smallest meaningful unit supported by evidence, with accurate
repository-relative paths and line ranges.

## Evidence & Analysis

Prioritize:

1. Language-aware/structural analysis.
2. Project-wide references and imports.
3. Entry-point and execution-path analysis.
4. Framework/configuration inspection.
5. Textual search.

Absence of references is meaningful only after relevant entry points, exports,
registrations, dynamic behavior, and external boundaries have been considered.

When evidence conflicts, investigate it; if unresolved, lower confidence and
explain the limitation.

## Output Format

Generate:

````markdown
# Dead Code Report

## Summary

- **Analysis scope:** `<scope>`
- **Total findings:** <number>
- **Files affected:** <number>
- **High confidence:** <number>
- **Medium confidence:** <number>
- **Low confidence:** <number>

## Dead Code

### 1. `<file path>`

**Type:** <finding type>

**Confidence:** <High|Medium|Low>

**Source:** [<filename> (L<start>-L<end>)](<relative path>#L<start>-L<end>)

```<language>
<actual source code>
```

**Evidence:** <specific supporting evidence.>

**Caveat:** <meaningful limitation, when applicable.>
````

Rules:

- Number findings sequentially.
- Use accurate repository-relative paths and line numbers.
- Include actual source code, never invented or paraphrased code.
- Include evidence for every finding.
- Include caveats only when meaningful.
- Do not recommend deletion unless explicitly requested.
- If no supported findings exist, report zero findings.
- Summary counts must exactly match the findings.

## Output File

Create the report in the project root using the first unused filename:

```text
k2gj-dead-code.md
k2gj-dead-code-2.md
k2gj-dead-code-3.md
...
```

Never overwrite an existing report or create additional files.

If file writing is unavailable, return the complete Markdown report directly.

## Failure & Fallback

- Missing project root or required scope: request/identify the missing location
  rather than analyzing an unrelated directory.
- Unreadable files: continue only if sufficient evidence remains and disclose
  the limitation.
- Missing static-analysis tools: use available repository/reference analysis.
- Unknown framework/runtime behavior: lower confidence rather than assuming dead
  code.
- Incomplete analysis: clearly identify the affected scope.
- No reliable findings: produce a valid zero-finding report.
- Never fabricate source, references, locations, or evidence.

## Final Verification

Confirm:

- The analyzed scope matches the requested scope.
- Every finding has an accurate path, source range, and source snippet.
- Evidence and confidence reflect actual analysis limitations.
- Relevant entry points, exports, registrations, framework mechanisms, dynamic
  usage, and external boundaries were considered.
- Generated, vendored, and dependency code was excluded unless requested.
- Summary counts exactly match the findings.
- No unsupported dead-code claim is presented as certain.
- The report follows the required structure.
- The output file was created without overwriting an existing report, or the
  complete report was returned when writing was unavailable.
