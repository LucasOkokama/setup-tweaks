# Global Development Instructions

These instructions apply to all projects and repositories unless a
project-specific AGENTS.md provides more specific requirements.

## Language and Communication

- All code, variable names, function names, class names, comments, commit
  messages, and technical documentation must be written in English unless a
  specific language is explicitly requested.
- Do not introduce non-English identifiers or comments unless the user
  explicitly requests them.
- Keep explanations clear, concise, and technically accurate.
- When explaining changes, focus on the reasoning, impact, and relevant
  trade-offs.

## Code Style and Readability

- Prioritize readable, maintainable, and self-explanatory code.
- Add blank lines between logical sections of code to improve readability and
  make the structure easier to understand.
- Avoid overly compressed code when separating logic improves comprehension.
- Prefer clear naming over excessive comments.
- Write comments only when the intent or reasoning cannot be easily understood
  from the code itself.
- Avoid unnecessary abstractions. Introduce complexity only when it provides
  clear value.

## Code Changes

- Before modifying code, understand the existing architecture, conventions, and
  patterns used in the project.
- Prefer minimal and focused changes over broad refactors unless a refactor is
  explicitly requested.
- Preserve existing behavior unless a behavior change is intended.
- Do not introduce new dependencies without explaining why they are necessary.
- Follow the project's existing tooling, frameworks, and conventions whenever
  possible.
- Avoid changing unrelated files or areas of the codebase.

## Logging and Debugging

- Do not add logs without a clear purpose.
- Logs should only be introduced when:
  - They provide important context for future troubleshooting.
  - They are required for debugging a specific issue.
  - Logging has been explicitly requested.
- Avoid leaving temporary debugging logs in the final implementation.
- Do not use logs as a replacement for proper error handling.
- Keep logs meaningful by including relevant context while avoiding sensitive
  information.

## Output and Formatting

- Do not use emojis in code, documentation, comments, commit messages, or
  general responses.
- The only exceptions are:
  - Markdown files where emojis are explicitly part of the desired style.
  - Cases where the user explicitly requests emojis.
- Keep generated content professional and consistent with the project's style.

## Error Handling and Reliability

- Handle errors intentionally instead of silently ignoring failures.
- Do not hide errors unless there is a clear reason to do so.
- Prefer actionable error messages that help identify the problem.
- Consider edge cases when modifying existing behavior.
- Avoid introducing fragile solutions that only work for the immediate case.

## Testing and Verification

- When modifying code, consider whether tests should be added or updated.
- Run relevant tests, type checks, linters, or build commands when available.
- If verification cannot be performed, clearly state what was not verified.
- Do not claim that something was tested if it was not actually executed.

## Security

- Never expose secrets, credentials, tokens, API keys, or sensitive information.
- Do not read or modify sensitive files unless explicitly required.
- Avoid insecure patterns even if they appear in existing code.
- Treat user input and external data as untrusted by default.

## Git and Repository Practices

- Do not create commits, push changes, or perform destructive Git operations
  unless explicitly requested.
- Before large changes, understand the current repository state.
- Avoid modifying generated files unless required.
- Keep changes easy to review.

## Decision Making

- Prefer simple and reliable solutions over clever implementations.
- When multiple approaches are possible, explain the trade-offs before making
  significant architectural decisions.
- Ask for clarification when requirements are ambiguous and the choice could
  significantly affect the project.
- Do not assume requirements that could lead to breaking changes.

## Final Review Before Completing Tasks

Before considering a task complete:

- Ensure the implementation follows the project's existing conventions.
- Check for unnecessary complexity.
- Remove temporary code, debugging artifacts, and unused imports.
- Verify that formatting and readability standards are respected.
- Confirm that no unnecessary logs, comments, or dependencies were introduced.