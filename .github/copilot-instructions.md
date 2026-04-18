# Copilot instructions

## Change discipline

- Keep changes focused on one concern. Do not clean up unrelated code.
- Prefer the simplest solution that fully solves the stated problem.
- Do not introduce abstractions, helpers, or extension points without a present need.
- Do not generalize for hypothetical future requirements.
- Stop once the problem is solved cleanly.

## Code review focus

Prioritize in this order: correctness, security, reliability, testability, maintainability.

Call out:

- missing tests for changed or important paths
- error-handling gaps or silent failures
- hardcoded secrets, tokens, or credentials
- unsafe defaults or permission issues
- breaking changes to public APIs, CLI behavior, config keys, or schemas
- risky workflow or deployment changes
- retry or idempotency mistakes in handlers, jobs, or webhooks

Do not spend review effort on trivial formatting or subjective naming preferences.

## Commits and pull requests

- PR titles must be usable as squash-merge commit messages.
- Use Conventional Commits: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `build:`, `ci:`, `chore:`.
- Mark breaking changes with `!` or `BREAKING CHANGE:`.

## Secrets and sensitive data

- Never suggest hardcoded credentials, tokens, API keys, or private keys.
- Use placeholder values in examples and sample configuration.

## File defaults

- LF line endings, spaces not tabs, final newline, no trailing whitespace.
- Follow `.editorconfig` and `.gitattributes` when present.

## Testing

- Add or update tests when behavior changes.
- Add regression tests for bug fixes when practical.
- Do not add shallow tests only to increase coverage numbers.

## Human review required

Flag for human review before suggesting changes to:

- branch protection, CODEOWNERS, or required checks
- authentication, authorization, or credential handling
- production deployment workflows or release automation
- public API contracts, config keys, or file format schemas
- security scanning, signing, or provenance controls
