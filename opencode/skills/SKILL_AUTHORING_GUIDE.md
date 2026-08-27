# SKILL.md Authoring Guide

This document defines the standard structure, authoring rules, and best
practices to use when creating a new `SKILL.md` for an Agent Skills-compatible
environment.

The structure below is intentionally opinionated: it provides a consistent
foundation while requiring each skill to include at least three meaningful
skill-specific subsections. Additional sections may be included when they
provide genuinely useful guidance for the skill's domain.

The Agent Skills specification requires YAML frontmatter with `name` and
`description`, followed by Markdown instructions. The specification also
supports optional metadata such as `license`, `compatibility`, `metadata`, and
`allowed-tools`.

## Core Principles

- Keep the skill focused on one coherent unit of work.
- Write instructions for an AI agent, not for a human reader.
- Prefer precise, actionable instructions over general explanations.
- Spend context only on information the agent would not reasonably know without
  the skill.
- Avoid repeating the same rule, instruction, or explanation in multiple
  sections.
- Avoid unnecessarily long paragraphs. Prefer concise paragraphs and focused
  bullet points.
- Do not add information merely to make the document appear more complete.
- Prefer defaults over lists of equally valid alternatives when one approach is
  clearly preferable.
- Be prescriptive when a specific sequence or behavior is important.
- Allow flexibility when multiple approaches are valid.
- Keep `SKILL.md` at or below 250 lines. Move detailed, conditional, or rarely
  needed material into `references/`, reusable utilities into `scripts/`, and
  supporting templates or static resources into `assets/` when appropriate.
- Use concrete examples when they clarify an expected behavior or output.
- Treat the `description` as a critical part of the skill because it is the
  primary mechanism used for skill discovery and activation.

Agent Skills uses progressive disclosure: metadata is available during
discovery, the `SKILL.md` body is loaded when the skill is activated, and
supporting resources are loaded only when needed. This makes unnecessary content
in `SKILL.md` particularly costly.

## Standard Template

Use the following structure as the default starting point for a new `SKILL.md`:

```markdown
---
name: [skill-name]
description: [skill-description]
---

# Skill Name

## Objective

[Explain what the skill is intended to accomplish and the problem it solves]

## Inputs & Parameters

[Define the information, files, parameters, context, or user-provided data the
skill may receive or require]

## Rules & Scope

[Define the boundaries of the skill, mandatory rules, assumptions, constraints,
and what the skill should or should not handle]

## Workflow

[Define the procedure the agent should follow]

## Skill-Specific Sections

[Define any additional sections that are specific to the skill's domain or
functionality. Add only sections that provide information not adequately covered
by the standard sections]

### [Skill-Specific Topic 1]

[Define a meaningful aspect of the skill's domain or functionality]

### [Skill-Specific Topic 2]

[Define a second meaningful aspect of the skill's domain or functionality]

### [Skill-Specific Topic 3]

[Define a third meaningful aspect of the skill's domain or functionality.]

[Add additional skill-specific subsections when they provide meaningful guidance
for the skill]

## Evidence & Analysis

[Define how evidence, sources, project artifacts, calculations, observations, or
other supporting information should be evaluated and used]

## Output Format

[Define what the final response or result should look like]

## Output File (optional)

[Define the requirements for any file that must be generated]

## Failure & Fallback

[Define how the skill should behave when information is missing, an operation
fails, an assumption cannot be validated, or the expected workflow cannot be
completed]

## Final Verification

[Define the checks the agent must perform before considering the task complete]
```

## General Guidelines

### Frontmatter

The YAML frontmatter is mandatory and must appear at the very beginning of the
file.

#### Fields

The `name` identifies the skill.

Rules:

- Must be present.
- Must be 1–64 characters.
- Use lowercase letters, numbers, and hyphens.
- Must not start or end with a hyphen.
- Must not contain consecutive hyphens.
- The skill directory name should match the `name` field.

Example:

```
name: pdf-analysis
```

The `description` is one of the most important parts of the skill because it is
used during skill discovery and activation.

It should concisely explain:

- What the skill does.
- When it should be used.
- What kind of task or user intent should trigger it.
- Optionally, important contexts or keywords that help distinguish it from other
  skills.

The description should focus on user intent and practical applicability rather
than implementation details.

Keep it concise. Do not move a large portion of the skill's instructions into
the description.

The current Agent Skills specification limits `description` to 1024 characters.

---

#### Optional Frontmatter

Optional fields such as `license`, `compatibility`, `metadata`, and
`allowed-tools` may be included when they provide meaningful information.

Do not add optional metadata simply because the specification allows it.

For example, use `compatibility` only when the skill genuinely depends on a
specific environment, product, system package, network access, or similar
requirement.

## Sections

### Skill Name

Use the skill's human-readable title.

It should normally correspond to the purpose of the skill rather than its
implementation.

Do not use this section to repeat the `description` verbatim.

---

### Objective

Describe the skill's primary objective.

This section should answer:

- What does the skill accomplish?
- What is the desired outcome?
- What problem is it solving?

Keep it focused on the purpose rather than explaining the entire workflow.
Prefer a short paragraph or a small list of objectives.

Do not repeat detailed rules or workflow steps here.

---

### Inputs & Parameters

Describe what the skill can receive or needs to operate.

Possible inputs include:

- User instructions.
- Files or directories.
- Configuration values.
- URLs.
- Existing project artifacts.
- Optional parameters.
- Required contextual information.
- Output preferences.

Use paragraphs, bullets, or both according to what best communicates the
requirements.

Clearly distinguish required inputs from optional inputs when that distinction
matters.

If no specific scope is provided, use the entire project as the default context.
Inspect the project structure and consider all relevant project files when
performing the task. Do not restrict the context to the currently open file or
to files explicitly mentioned by the user unless a narrower scope is explicitly
specified.

Do not describe the processing workflow here. Inputs should describe what the
skill receives, not what it does with them.

---

### Rules & Scope

Define the operational boundaries of the skill.

This is the appropriate place for:

- Mandatory rules.
- Constraints.
- Inclusion and exclusion criteria.
- Scope boundaries.
- Domain-specific requirements.
- Important assumptions.
- Things the skill must never do.
- Conditions under which another skill or process should be preferred.

Use paragraphs, bullets, or both according to what best communicates the
requirements.

Avoid turning every individual rule into its own heading. Additional `###`
sections should be used only when the rules naturally form distinct groups and
the extra structure significantly improves readability.

Do not repeat workflow steps here. If something describes _when or how_ an
operation is performed, it usually belongs in `Workflow`.

---

### Workflow

Define the actual procedure the agent should follow.

This section is intentionally more flexible than the others.

The workflow can be:

- A paragraph, when the process is simple.
- A bullet list, when the process is short and sequential.
- Numbered `###` subsections, when the process has several meaningful phases.

When using subsections, use `###` and number them:

```markdown
### 1. [Step Name]

[Instructions description]

### 2. [Step Name]

[Instructions description]

### 3. [Step Name]

[Instructions description]

### 4. [Step Name]

[Instructions description]
```

Use numbered subsections only when they improve navigation. Do not create a
subsection for every small action.

The workflow should describe _how the agent should perform the task_, including
important decisions and sequencing.

Prefer procedures over declarations. For example, explain what the agent should
inspect, determine, compare, validate, or produce instead of simply stating that
it should "perform a thorough analysis."

Do not duplicate rules that are already clearly defined in `Rules & Scope`.

---

### Skill-Specific Sections

This section is mandatory and must appear between `Workflow` and
`Evidence & Analysis`.

Every skill must include at least three meaningful skill-specific `###`
subsections. These subsections should contain information that is specific to
the skill and does not merely duplicate the standard sections.

Add more skill-specific subsections when they provide meaningful guidance for
the skill. Avoid creating many small sections or adding subsections merely to
satisfy the minimum count. Each subsection should make the instructions easier
to understand or execute.

---

### Evidence & Analysis

Use this section when the skill needs to reason from evidence, inspect
artifacts, evaluate sources, compare alternatives, perform calculations, or
otherwise justify conclusions.

Describe:

- What constitutes valid evidence.
- Which sources or artifacts should receive priority.
- How conflicting information should be handled.
- What assumptions may be made.
- What must be verified.
- How uncertainty should be represented.
- How conclusions should be derived from the available evidence.

For simple skills where evidence or analysis is not meaningful, this section may
be brief.

Use paragraphs, bullets, or both according to what best communicates the
requirements.

Do not repeat the `Workflow`. The `Workflow` describes the process; this section
describes the standards used when evaluating information during that process.

---

### Output Format

Define the expected final result produced by the skill.

The output can be:

- A Markdown report.
- A JSON object.
- A code change.
- A textual answer.
- A structured analysis.
- A set of recommendations.
- A generated document.
- Another explicitly defined format.

This section describes the _content and structure of the result_, not
necessarily where it is stored.

For example:

- The result must contain a summary.
- Findings must be grouped by severity.
- Recommendations must follow each finding.
- The response must use Markdown.
- Unverified assumptions must be explicitly identified.

If the output is a Markdown report, specify the required sections only when they
are important.

The output may either be returned directly in the AI response or written to a
file, depending on the skill requirements.

Avoid duplicating the same structure in `Output File`.

---

### Output File

Use this section only when the skill is expected to generate or modify a file.

Define the file-specific requirements, such as:

- File name or naming convention.
- File extension.
- Destination directory.
- Required directory structure.
- Encoding.
- Required content.
- Whether existing files may be overwritten.
- Whether the file must be created, updated, or both.

For example:

```markdown
### Output File

Generate a Markdown file named `analysis-report.md` in the requested output
directory.
```

The file must contain the complete report defined in `Output Format`.

If the skill does not generate or modify a file, omit this section entirely.

Do not use `Output File` to repeat the complete output structure already defined
in `Output Format`.

If the file cannot be created or modified for any reason, return the complete
output directly in the conversation instead of silently omitting the result.

When returning file content directly in the conversation:

- Preserve the content exactly as it should appear in the file.
- If the output contains code, Markdown, configuration, structured text, or any
  other content that would normally be represented using a code block, wrap the
  entire output in a Markdown code block using **10 backticks** as the fence.
- Do not use fewer than 10 backticks for this fallback code block.
- This 10-backtick fence is intended to prevent conflicts with code blocks that
  may already exist inside the generated content.

---

### Failure & Fallback

Define how the agent should behave when the expected workflow cannot be
completed normally.

Consider cases such as:

- Required input is missing.
- A referenced file does not exist.
- A source cannot be accessed.
- Evidence is contradictory.
- A tool fails.
- A required dependency is unavailable.
- An operation produces an invalid result.
- The requested task is outside the skill's scope.

Prefer explicit and useful fallback behavior.

For example:

- Ask the user for missing information when it is essential.
- Continue with available information when the missing information is optional.
- Clearly identify assumptions.
- Never fabricate unavailable evidence.
- Stop when continuing would produce an unreliable result.
- Provide a partial result when it remains useful and clearly label what is
  incomplete.
- If a required output file cannot be created or modified, return the complete
  output directly in the conversation using the file-output fallback defined in
  `Output File`.

Avoid repeating general rules from `Rules & Scope`. Focus specifically on what
should happen when the normal path breaks.

---

### Final Verification

Define the checks that must be performed before the skill considers its work
complete.

Verification can include:

- Confirming required inputs were processed.
- Checking that required rules were followed.
- Validating calculations or transformations.
- Checking output structure.
- Confirming that required files exist.
- Confirming that generated files contain the expected content.
- Ensuring unsupported assumptions are not presented as facts.
- Ensuring no required step was skipped.
- If a file cannot be created or modified, confirming that the complete output
  was returned in the conversation using the required 10-backtick code fence
  when applicable.

Keep verification practical and directly related to the skill.

Do not restate the entire skill as a checklist. Verify only the conditions that
materially determine whether the result is correct.

## Writing Style & Structure Rules

### Avoid Repetition

Avoid repeating the same information across sections.

Each instruction should ideally have one authoritative location.

For example:

- Put scope boundaries in `Rules & Scope`.
- Put procedures in `Workflow`.
- Put evaluation standards in `Evidence & Analysis`.
- Put response structure in `Output Format`.
- Put file-specific requirements in `Output File`.
- Put exceptional behavior in `Failure & Fallback`.
- Put completion checks in `Final Verification`.

If an instruction naturally belongs to one section, do not copy it into several
sections merely for emphasis.

Important rules may be referenced elsewhere when necessary, but should not be
rewritten repeatedly.

---

### Keep Content Concise

Avoid unnecessarily long paragraphs.

Prefer:

- Short paragraphs.
- Focused bullet points.
- Clear instructions.
- Concrete terminology.
- Direct statements.

Do not turn every sentence into a bullet point. Use the format that best
communicates the information.

A section containing three concise bullets is often preferable to a large
paragraph. A simple concept may be better expressed as one paragraph.

The goal is **high information density without unnecessary verbosity**.

---

### Avoid Structural Pollution

Do not create a new heading for every concept.

The standard sections should remain the primary structure.

Use numbered `###` subsections inside `Workflow` when they improve navigation or
organize meaningful sequential phases.

The `Skill-Specific Sections` section is an explicit exception: it must contain
at least three meaningful `###` subsections specific to the skill. These
subsections should be used to organize distinct skill-specific guidance, not to
artificially increase the document's structure.

For other sections, prefer paragraphs and bullet lists. Create additional
subsections only when the content is genuinely complex enough to justify them.

A heavily fragmented `SKILL.md` is harder to scan and increases cognitive and
context overhead.

---

### Avoid Redundant Examples

Examples are useful when they clarify behavior that would otherwise be
ambiguous.

Do not add examples merely to make the skill longer.

Prefer one strong example over several nearly identical examples.

When examples are included, ensure they demonstrate something that the agent
needs to learn.

---

### Prefer Actionable Instructions

Use instructions that tell the agent what to do.

Prefer:

```
Inspect the repository structure before deciding which files are relevant.
```

Over:

```
The repository structure is important.
```

Prefer:

```
Use the most authoritative available source when conflicting values are found.
```

Over:

```
Authoritative sources are preferred.
```

The skill should help the agent act, not merely describe concepts.

---

### Avoid Over-Specification

Do not prescribe implementation details when multiple valid approaches can
achieve the same result.

Give the agent freedom when the choice does not materially affect correctness.

Be explicit when a particular behavior is required because an operation is
fragile, safety-sensitive, deterministic, or dependent on a project convention.

Good skills balance flexibility with control.

---

### Progressive Disclosure

A skill should contain only the information that needs to be available when the
skill is activated.

The main `SKILL.md` must remain at or below 250 lines. When additional detail is
required, use supporting resources instead of expanding `SKILL.md`.

Use supporting directories when appropriate:

- `scripts/` for executable utilities or deterministic processing logic.
- `references/` for detailed documentation, domain knowledge, conditional
  instructions, or material that is only needed in specific scenarios.
- `assets/` for templates, static resources, examples, or other supporting
  artifacts.

When a supporting resource is required for a specific scenario, explicitly tell
the agent when to load or use it from `SKILL.md`.

Do not move essential instructions into supporting files solely to reduce the
line count. The main skill must still contain enough information for the agent
to understand its purpose, scope, workflow, and required behavior.

Avoid deeply nested reference chains. Supporting files should be directly
discoverable from `SKILL.md` whenever possible.

---

### Recommended Skill Directory

A minimal skill may contain only:

```
skill-name/
└── SKILL.md
```

A skill that requires supporting material may contain:

```
skill-name/
├── SKILL.md
├── scripts/
├── references/
└── assets/
```

Additional files should exist because they provide useful supporting resources,
not simply because the specification permits them.

If the required instructions or supporting knowledge would cause `SKILL.md` to
exceed 250 lines, use the supporting directories instead of increasing the size
of `SKILL.md`.

---

### Size and Context Guidance

The final `SKILL.md` must contain no more than **250 lines**.

Treat the 250-line limit as a hard constraint for the main skill file, not
merely as a recommendation or target. Do not exceed this limit by compressing
unrelated content into dense paragraphs or removing required structure.

When the skill requires more information than can reasonably fit within 250
lines:

- Keep `SKILL.md` focused on the instructions required during normal skill
  execution.
- Preserve the standard `SKILL.md` structure and required sections.
- Move detailed, conditional, rarely needed, or reference-oriented material into
  `references/`.
- Move reusable executable logic or utilities into `scripts/`.
- Move templates, static resources, examples, or other supporting files into
  `assets/` when appropriate.
- Add clear instructions in `SKILL.md` specifying when the supporting resource
  must be loaded or used.
- Do not move essential workflow rules into supporting files merely to satisfy
  the line limit.

The 250-line limit applies only to `SKILL.md`. Supporting files may contain
additional detail when that detail is necessary and appropriately scoped.

Prefer a shorter, precise `SKILL.md` over a longer file containing duplicated,
low-value, or rarely needed information.

---

### Final Authoring Checklist

Before considering a new `SKILL.md` complete, verify:

- The YAML frontmatter is valid.
- `name` is present and follows the naming constraints.
- `description` is present, concise, and clearly explains both capability and
  activation context.
- The skill represents one coherent unit of work.
- The objective is clear.
- Inputs and parameters are defined when relevant.
- Scope and mandatory rules are explicit.
- The workflow is actionable.
- Workflow subsections are used only when they improve clarity.
- The `Skill-Specific Sections` section is present.
- It contains at least three meaningful `###` subsections specific to the skill.
- Each skill-specific subsection provides distinct, actionable information.
- Additional skill-specific subsections are included when they provide
  meaningful guidance, without introducing unnecessary fragmentation.
- Evidence and analysis requirements are defined when relevant.
- The output format is unambiguous.
- `Output File` exists only when a file is actually generated or modified.
- Failure and fallback behavior is defined for meaningful failure cases.
- Final verification checks correctness and completeness.
- Information is not unnecessarily repeated across sections.
- Paragraphs are not excessively long.
- The document does not contain unnecessary headings or structural
  fragmentation.
- Detailed conditional knowledge has been moved to supporting resources when
  appropriate.
- The skill contains actionable instructions rather than generic explanations.
- The final `SKILL.md` contains only information that materially improves the
  agent's ability to perform the task.
- The final `SKILL.md` contains no more than 250 lines.
- Content that cannot reasonably fit within the 250-line limit has been moved to
  appropriate supporting files.
- Supporting files are referenced from `SKILL.md` when they are required for
  specific workflows or scenarios.
- If the skill generates or modifies files, a fallback is defined for cases
  where the file cannot be created or modified.
- The file-output fallback returns the complete result in the conversation.
- Code, Markdown, configuration, or other structured file content returned
  through the fallback uses a 10-backtick code fence.
