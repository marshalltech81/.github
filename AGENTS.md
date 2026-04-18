# AGENTS.md

## Priority order

Apply these rules in this order:

1. Security and safety
2. Repository policy and workflow
3. Correctness, contracts, and validation
4. Scope control and simplicity
5. Maintainability and documentation
6. Efficiency and brevity

If two rules conflict, follow the higher-priority rule.

## Operating model

AI agents are implementation assistants.

Do not act as an autonomous owner of:

- repository policy
- architecture
- security posture
- release process
- legal, compliance, or licensing decisions

Do not:

- change unrelated files
- introduce speculative refactors
- weaken security, CI, branch protections, or repository safeguards
- bypass documented workflow constraints
- invent credentials, requirements, compliance claims, or operational assumptions

## Repository workflow assumptions

Assume all of the following unless the repository explicitly states otherwise:

- pull request based development
- protected default branch
- linear history on protected branches
- squash merge as the default merge strategy
- required status checks before merge
- CODEOWNERS review for sensitive areas
- signed commits may be required on protected branches

Do not assume direct pushes to the default branch are allowed.

## Shared .github repository defaults

A repository may inherit default community-health files and templates from a special account-level or
organization-level `.github` repository.

Before adding repository-local scaffolding, check whether shared defaults already exist for:

- `CODE_OF_CONDUCT.md`
- `CONTRIBUTING.md`
- `SECURITY.md`
- `SUPPORT.md`
- `FUNDING.yml`
- issue templates
- pull request templates

Required behavior:

- Prefer aligning with shared `.github` defaults when they already provide the needed behavior.
- Only add repository-local community-health files when the repository needs to override shared defaults.
- Treat repository-local files as overrides when shared defaults exist.
- Note that a repository-local `.github/ISSUE_TEMPLATE/` directory overrides shared default issue templates.

Do not:

- duplicate shared scaffolding unnecessarily
- suggest repository-local templates before checking shared defaults
- override shared defaults implicitly without calling out the divergence

## Architecture, ADRs, and roadmaps

Use architecture documentation, accepted ADRs, and roadmap documents as project context.

Authority order:

1. repository policy and workflow rules
2. documented architecture
3. accepted ADRs
4. roadmap documents

Required behavior:

- Follow documented architecture constraints, layering rules, ownership boundaries, and service boundaries.
- Treat accepted ADRs as authoritative unless the task explicitly includes revisiting them.
- Prefer solutions aligned with documented architecture and prior decisions.
- Use roadmaps to understand direction, sequencing, and scope when they appear current.
- Call out conflicts with architecture, ADRs, or roadmap direction.

Do not:

- re-open settled architectural decisions implicitly
- introduce architecture changes without identifying them
- treat roadmap documents as stronger authority than policy, architecture docs, or accepted ADRs
- assume undocumented architecture changes are acceptable because a local implementation would be simpler

Recommend a new ADR when a change:

- introduces a new architectural pattern
- changes major dependency direction
- changes service boundaries or data ownership
- changes deployment, runtime, or security posture materially
- replaces a prior accepted architectural decision

## Change discipline

Required behavior:

- Keep each change focused on one concern.
- Keep diffs narrow and reviewable.
- Prefer reversible changes.
- Use the simplest solution that fully solves the stated problem.
- Follow existing repository patterns before introducing new ones.
- Reuse existing utilities, patterns, standard library features, and approved dependencies when they
  solve the problem well.
- Follow established best practices and conventions for the language, framework, and ecosystem in use.
- Stop once the requested problem is solved cleanly.

Do not:

- perform drive-by cleanup
- mix formatting-only changes with behavior changes
- rename files or symbols unless necessary
- solve adjacent problems that were not requested
- generalize for hypothetical future requirements
- introduce abstraction, indirection, configuration, or extension points without a present need
- replace a local fix with a broad refactor unless the task explicitly requires it
- add dependencies for trivial tasks
- reinvent the wheel when an existing repository pattern, standard library feature, approved dependency,
  or established utility already solves the problem well

Use more complex solutions only when justified by:

- correctness
- security
- maintainability
- measurable reduction in duplication or risk

## DRY, abstraction, and design patterns

Use DRY principles as a maintainability tool, not as an absolute goal.

Required behavior:

- Reduce meaningful duplication when it improves correctness, consistency, or maintainability.
- Prefer reuse when duplicated logic is truly shared in behavior and likely to evolve together.
- Prefer small, local abstractions before broad shared frameworks or utility layers.
- Prefer known, established design patterns when they are already used in the repository or are idiomatic
  for the ecosystem.
- Prefer patterns that improve clarity, correctness, testability, and maintainability.

Do not:

- extract abstractions for minor, one-off, or speculative duplication
- unify code paths that only look similar but have different responsibilities or likely future changes
- trade readability for theoretical DRY compliance
- introduce helpers, base classes, factories, strategies, adapters, dependency inversion, or interface
  boundaries without a concrete present need
- force textbook object-oriented patterns onto ecosystems that favor simpler or more idiomatic approaches

Guidance:

- Prefer clear duplication over the wrong abstraction.
- Keep code straightforward and local until duplication is proven to be real and persistent.

## Commits and pull requests

Assume squash merge is used for the default branch.

Required behavior:

- Write PR titles so they can be used as squash-merge commit messages.
- Keep commit history reviewable.
- Optimize the final change for squash merge.

Conventional Commits are required for:

- PR titles
- final squash-merge commit titles
- individual commits when repository workflow, reviewer, or tooling requires them

Use only standard types unless the repository explicitly allows others:

- `feat:`
- `fix:`
- `docs:`
- `refactor:`
- `test:`
- `build:`
- `ci:`
- `chore:`

Mark breaking changes with `!` or `BREAKING CHANGE:` when applicable.

Do not invent custom commit types unless the repository explicitly allows them.

## Contracts, compatibility, and configuration

Treat public and semi-public interfaces as compatibility-sensitive contracts.

This includes:

- APIs
- CLI behavior
- config keys and defaults
- environment variables
- event and message schemas
- file formats
- database schemas
- generated artifacts
- automation behavior
- public package interfaces

Required behavior:

- Preserve backward compatibility unless the task explicitly includes a breaking change.
- Identify contract impact before changing public interfaces, configuration, schemas, file formats, or command behavior.
- Prefer additive changes over breaking changes when practical.
- Version interfaces when the repository or ecosystem expects versioned contracts.
- Document breaking changes clearly.
- Provide migration guidance when compatibility cannot be preserved.
- Keep configuration names, defaults, and behavior consistent.
- Document new configuration values and defaults.
- Prefer explicit validation of required configuration.
- Determine the canonical source for generated files, schemas, policies, docs, config, or derived
  artifacts before editing them.

Do not:

- remove or rename public interfaces silently
- change request or response semantics silently
- change default behavior without calling out impact
- introduce hidden configuration switches
- spread the same configuration value across multiple inconsistent sources without reason
- edit generated files manually when they are expected to be regenerated
- update derived artifacts without updating the source that owns them

## Feature flags and rollout controls

Use feature flags and rollout controls deliberately when the repository already uses them or when a
risky behavior change benefits from staged rollout.

Required behavior:

- Document new feature flags, their purpose, and their default state.
- Prefer explicit flag names that reflect the behavior being controlled.
- Keep rollout and rollback behavior understandable.
- Remove stale or temporary flags when they are no longer needed.

Do not:

- introduce hidden, undocumented, or permanent ad hoc flags without justification
- change default flag behavior silently
- use feature flags as a substitute for correctness or test coverage
- leave dead or fully rolled out flags in place indefinitely without reason

## Dependencies, licensing, and supply chain

Treat dependency choice as an ongoing maintenance and supply-chain decision.

Required behavior:

- Make the smallest necessary dependency change.
- Explain why a new dependency is needed.
- Prefer standard library or already-approved dependencies.
- Prefer actively maintained, widely adopted ecosystem tools and libraries when a new dependency is necessary.
- Consider maintenance health, ecosystem fit, ownership, security, license, and long-term support burden.
- Update lockfiles when dependency changes require them.
- Preserve required notices, attribution, and legal headers.
- Preserve artifact signing, provenance, SBOM, attestation, or verification steps when they are already
  part of the repository.
- Prefer pinned versions for build-critical tools, actions, and dependencies when the repository already
  follows that pattern.
- When changing Dependabot configuration, ensure referenced labels exist or update the repository’s
  label source of truth.

Dependabot labels:

- When adding or modifying Dependabot configuration, ensure appropriate Dependabot-related labels exist
  for the repository.
- If the repository manages labels as code, update the label source of truth.
- If labels are managed manually, clearly list the labels that must be created.
- Do not reference non-existent labels in Dependabot configuration without calling out the required setup.
- Prefer labels such as `dependencies`, `dependabot`, `security`, and ecosystem-specific labels when
  useful, such as `github-actions`, `docker`, `python`, or package-manager-specific labels.
- For Dependabot security updates, prefer labels that make security relevance clear.
- Do not create excessive label granularity unless the repository already uses that labeling strategy.

Do not:

- add niche or weakly maintained dependencies for trivial problems
- add dependencies with unclear, incompatible, or unreviewed licenses when license constraints matter
- remove attribution, notices, or license-related files casually
- omit required lockfile updates
- bypass signing, provenance, verification, or attestation steps without explicit justification
- weaken release integrity controls to make a workflow simpler

Escalate when dependency, licensing, provenance, or artifact trust cannot be confidently assessed.

## Validation

Before proposing a final change:

1. Install dependencies using the documented package manager.
2. Run formatting checks if configured.
3. Run linting if configured.
4. Run type checks if configured.
5. Run tests relevant to the changed code.
6. Run the full test suite if the change affects shared infrastructure, build tooling, authentication,
   data access, workflows, or releases.

If a command cannot be run:

- say so explicitly
- explain why
- do not imply validation was completed when it was not

## Testing and code coverage

Prefer the repository’s existing testing strategy when one is established.

Required behavior:

- Add or update tests when behavior changes.
- Prefer unit tests for isolated logic and deterministic behavior.
- Prefer integration tests when validating interaction between components, services, persistence,
  containers, workflows, or external boundaries.
- Add regression tests for bug fixes when practical.
- Keep tests focused on observable behavior and expected outcomes.
- Run the relevant test suite before proposing a final change.

Coverage rules:

- Treat code coverage as a signal, not a goal by itself.
- Prefer meaningful coverage of changed and risky code over broad but shallow coverage increases.
- Follow repository coverage thresholds when they exist.
- Do not lower coverage thresholds or exclude code from coverage without explicit justification.

Do not:

- add shallow tests only to increase coverage numbers
- rewrite the repository’s test strategy as part of an unrelated change
- replace meaningful integration coverage with narrow unit tests when the risk is in component interaction
- claim tests were added or run if they were not

## Local validation before push

Before pushing changes, prefer to run the repository’s local validation path when it exists.

Required behavior:

- Run relevant formatting, linting, type-checking, and fast test commands before push when available.
- Review the final diff before push.
- Review sensitive changes a second time before push, especially workflows, security files,
  infrastructure, release automation, policy files, and generated artifacts.

Preferred local enforcement:

- Use local Git hooks when the repository provides them.
- Prefer `pre-commit` or equivalent hook frameworks when they are already part of the repository workflow.
- Use local hooks to enforce fast, high-signal checks before commit or push.
- Use `pre-push` hooks for checks that are fast enough to run before every push.

Do not:

- claim checks were run if they were not
- skip local validation without saying so
- rely on local hooks as the only enforcement layer

Server-side CI and required branch checks remain the source of truth for merge protection.

## Linting, formatting, and file defaults

Prefer the repository’s existing linting and formatting toolchain when configured.

Required behavior:

- Run configured formatting checks before proposing a final change.
- Run configured linting checks before proposing a final change.
- Follow established formatter output rather than hand-formatting around it.
- Follow established linting rules unless the task explicitly includes changing those rules.
- Keep formatting-only changes separate from behavior changes when practical.
- Follow `.editorconfig` and `.gitattributes` when present.

Default file formatting unless the repository states otherwise:

- LF line endings
- spaces instead of tabs
- final newline at end of text files
- trailing whitespace trimmed

Exceptions are allowed when the file format or toolchain requires them, such as tabs in `Makefile`.

Do not:

- introduce a second formatter when one formatter is already standard
- mix competing lint or format tools without a clear reason
- replace an established linting or formatting toolchain as part of an unrelated change
- disable or weaken linting or formatting rules just to make a change pass

## Secrets and sensitive data

Handle secrets and sensitive data conservatively.

Required behavior:

- Keep secrets out of source code, logs, tests, fixtures, screenshots, generated artifacts, and documentation.
- Use placeholder values in examples and sample configuration.
- Minimize exposure of personal, customer, or operationally sensitive data.
- Prefer secret leak prevention over post-commit cleanup.
- Preserve and strengthen GitHub secret scanning, push protection, custom patterns, and
  repository-approved scanners such as `gitleaks` when present.
- Check for hardcoded secrets in changed files and, when appropriate, repository history.
- Treat discovered secrets as incidents: remove exposure, rotate credentials, and document impact
  when required by repository policy.

Do not:

- commit credentials, tokens, API keys, certificates, private keys, or other secrets
- suppress or bypass secret scanning controls without explicit justification
- assume deleting a secret from the latest commit is sufficient if it may exist in git history
- leave exposed secrets in issues, pull requests, logs, test data, screenshots, or generated artifacts
- add sample values that look like real secrets

## Privacy and data retention

Handle personal, customer, and operationally sensitive data conservatively.

Required behavior:

- Minimize collection, storage, retention, and exposure of sensitive data.
- Call out new data flows, retention changes, or new categories of stored sensitive data.
- Prefer least-data and least-retention designs when practical.

Do not:

- expand collection or retention of personal or sensitive data silently
- log or expose personal or sensitive data unnecessarily
- treat privacy-sensitive changes as ordinary refactors

Escalate when a change materially affects data collection, retention, or exposure risk.

## Operational reliability

Design for clear failure behavior, observability, safe rollout, and reversibility.

Required behavior:

- Prefer explicit error handling over silent failure.
- Fail fast for invalid configuration and unrecoverable startup conditions.
- Use retries, timeouts, and fallback behavior only when appropriate for the system boundary.
- Keep error messages actionable and safe.
- Add or update logs, metrics, tracing, or health checks when they materially improve validation or
  production visibility.
- Keep logs useful for debugging without leaking secrets or sensitive data.
- Consider startup, shutdown, retries, timeouts, and failure visibility.
- Consider rollback strategy for risky changes.
- Prefer incremental rollout paths when changes affect production behavior, infrastructure, or data.
- Call out irreversible steps explicitly.
- Update runbooks, on-call guidance, operational notes, or recovery steps when behavior materially changes.

Do not:

- swallow exceptions or ignore command failures silently
- retry indefinitely without bounds
- expose sensitive details in user-facing or external error messages
- add noisy logs without clear value
- log secrets, tokens, credentials, or personal data
- remove useful operational signals without replacement
- make high-blast-radius changes without explaining recovery options
- assume rollback is trivial when data or schema changes are involved

## Concurrency, idempotency, and retries

Design for repeated execution, partial failure, and concurrent activity when the system boundary requires it.

Required behavior:

- Consider concurrency and duplicate execution for handlers, jobs, webhooks, background tasks, and APIs
  that may be retried.
- Prefer idempotent operations when retries, duplicate delivery, or repeated requests are realistic.
- Use bounded retries with clear stop conditions.
- Prefer exponential backoff for retrying remote API calls when retries are appropriate.
- Prefer exponential backoff with jitter for remote API retries when the repository already uses retry
  helpers or middleware.
- Use timeouts for network and remote boundary operations when the repository pattern supports them.
- Consider locking, deduplication, sequencing, or optimistic concurrency when shared state is involved.
- Keep retry behavior visible in logs, metrics, or tracing when it materially affects operability.

Do not:

- assume operations run exactly once
- retry indefinitely without limits
- retry non-idempotent side effects blindly
- ignore duplicate-delivery risk for queues, webhooks, cron jobs, or external APIs
- add retry logic without considering timeout, cancellation, observability, and total retry duration

## Performance and caching

Preserve or improve performance characteristics for the changed path.

Required behavior:

- Consider latency, memory, CPU, I/O, network cost, and startup cost for hot or frequently used paths.
- Call out obvious performance risks introduced by the change.
- Preserve existing performance budgets, limits, or SLO-related behavior when documented.
- Avoid unnecessary repeated work, excess allocations, avoidable round trips, and obvious N+1 behavior.
- Identify the source of truth before changing cache behavior or derived state.
- Consider invalidation, staleness, and consistency impact when changing cache keys, TTLs, or refresh behavior.
- Document cache behavior changes when they materially affect correctness or operability.

Do not:

- add expensive work to hot paths without justification
- move expensive work into startup, request handling, or critical loops without calling it out
- introduce cache layers without a clear correctness and invalidation story
- treat cached or derived state as authoritative without justification
- change cache semantics silently when downstream behavior may depend on them

Guidance:

- Avoid premature optimization, but do not ignore obvious regressions.
- Prefer correctness over caching sophistication.
- Keep cache behavior simple, predictable, and observable.

## Database and migration safety

Treat schema and data migrations as high-risk changes.

Required behavior:

- Prefer backward-compatible schema changes when practical.
- Separate schema expansion from cleanup or removal when possible.
- Consider rollout order across application code, jobs, and migrations.
- Call out locking, data backfill, downtime, and rollback risk.

Do not:

- combine risky schema, data, and behavior changes without justification
- assume destructive migrations are safe without an explicit migration plan
- drop or rename columns, tables, or constraints without assessing compatibility impact

## Time and clock handling

Treat time, time zones, and clock behavior as correctness concerns.

Required behavior:

- Prefer explicit, unambiguous time handling.
- Prefer UTC for internal storage, comparison, and service-to-service communication unless the
  repository clearly requires a different model.
- Consider time zones, DST, locale, skew, and precision when changing time-related logic.
- Document behavior changes for scheduling, retention, TTLs, billing windows, or expiration logic.

Do not:

- rely on ambiguous timestamp parsing or formatting
- assume local time behavior is safe for distributed or server-side logic
- ignore clock skew or duplicate execution risk for time-based workflows

## Python projects

For Python repositories, prefer the existing repository toolchain when one is already configured.

Default Python tooling for modern repositories:

- `ruff` for linting
- `ruff format` for formatting
- `pytest` for tests
- `pyright` for static type checking
- `pre-commit` for local hook enforcement

Required behavior:

- Follow the repository’s existing configuration unless the task explicitly includes toolchain migration
  or modernization.
- Do not introduce a second formatter when one formatter is already standard.
- Do not mix competing lint or format tools without a clear reason.
- Do not replace the repository’s Python toolchain as part of an unrelated change.

When configured, run relevant commands:

- `ruff check`
- `ruff format --check`
- `pytest`
- `pyright`

Do not add tools only to satisfy this policy unless the task explicitly includes tooling setup or modernization.

## Shell and Bash scripts

For shell scripts, prefer the repository’s existing shell conventions when established.

Default shell practices:

- explicit shebangs that match the shell actually used
- `shellcheck` for shell script linting
- small, focused shell scripts for automation, glue code, and wrappers
- Bash-specific features only when the script is explicitly Bash
- simpler, more structured languages when the script grows large or complex

Required behavior:

- Match the script implementation to its shebang.
- If the script uses Bash features, use a Bash shebang rather than a generic POSIX shell shebang.
- Follow shell best practices for quoting, variable expansion, error handling, and command substitution.
- Prefer safe quoting and avoid word-splitting bugs.
- Keep shell scripts readable, predictable, and easy to validate.
- Run `shellcheck` when configured.

Command option style:

- Prefer long-form options when clearly supported and materially more readable.
- Prefer short-form or POSIX-style options when portability matters.
- Be consistent within the script.
- Avoid GNU-only long options when portability is important unless the repository explicitly targets GNU tooling.
- Prefer explicit commands over dense one-liners when readability would otherwise suffer.

Do not:

- write Bash-specific code under a generic `sh` shebang
- add large amounts of complex logic to shell when a structured language is a better fit
- disable or ignore `shellcheck` warnings without clear justification
- rely on unsafe unquoted expansions or fragile command substitution patterns

## Web services and 12-factor principles

For web services, prefer service-design practices aligned with the Twelve-Factor App methodology
when compatible with repository requirements.

Prefer:

- one codebase per service with clear deploy targets
- explicit, isolated dependencies
- configuration through environment-specific configuration mechanisms
- backing services treated as replaceable attached resources
- clear separation of build, release, and run stages
- stateless service processes where practical
- explicit network bindings
- horizontal scaling through process replication when appropriate
- fast startup and graceful shutdown
- strong development, staging, and production parity
- logs emitted as structured event streams to stdout or stderr when the platform supports it
- admin and maintenance tasks run as one-off processes

Do not:

- couple services tightly to one machine, container, or local filesystem without reason
- hard-code deploy-specific configuration into the codebase
- introduce avoidable dev/prod drift
- treat local convenience patterns as production architecture defaults
- force a 12-factor interpretation when the repository clearly uses a different justified operational model

## Docker and container builds

For Docker-based repositories, prefer existing repository container patterns when established.

Default Docker practices:

- multi-stage builds
- small runtime images with only required runtime artifacts
- minimal runtime images, including distroless-style images when compatible with runtime and debugging needs
- pinned base images, preferably with digest references when repository policy or release rigor requires it
- cache-friendly layer ordering
- `.dockerignore` to reduce build context size
- explicit, minimal runtime dependencies
- non-root execution
- read-only root filesystems when supported
- tmpfs mounts for ephemeral writable paths when scratch space is needed
- least-privilege container defaults
- Docker secrets for sensitive values in Docker Compose when supported
- `pre-commit` for local hook enforcement when already used
- Dockerfile linting, such as `hadolint`, when Dockerfiles are part of the workflow

Required behavior:

- Follow existing Dockerfile and image-building conventions unless the task explicitly includes modernization.
- Prefer multi-stage builds when build-time and runtime dependencies differ.
- Prefer reproducible builds over floating-image behavior.
- Keep final images as small and minimal as practical.
- Keep build context small and avoid copying unnecessary files into images.
- Reuse existing base images, build stages, and container patterns when they already solve the problem well.
- Prefer dedicated service users over root execution when the application can run without privileges.
- Treat container hardening as a default goal, but do not break runtime compatibility just to satisfy
  a hardening preference.
- When using Docker Compose, prefer Docker secrets over environment variables for sensitive values when practical.
- Keep non-sensitive configuration separate from secret material.

Do not:

- replace an established container strategy as part of an unrelated change
- add unnecessary packages, tools, or shells to runtime images
- use broad `COPY . .` patterns when narrower copy patterns are practical
- introduce multiple competing image build patterns without reason
- switch base images without explaining compatibility, security, or maintenance impact
- force distroless, read-only roots, or tmpfs usage when the repository clearly requires different runtime behavior
- store sensitive credentials in plain environment variables in Compose when Docker secrets are already supported

When Docker tooling is configured, run relevant commands:

- documented image build command
- documented container test or smoke-test command
- configured Dockerfile linting or policy checks
- configured `pre-commit` hooks relevant to Docker or container files
- documented Compose validation or config-rendering command when Compose files change

Preserve multi-platform build behavior when already part of the workflow.

## Service configuration hardening

When configuring services, especially services deployed in containers, prefer secure and hardened
defaults compatible with the operational model.

Prefer:

- TLS 1.3 for inbound and outbound encryption
- TLS 1.2 only when compatibility requires it
- strong authenticated transport for sensitive or administrative traffic
- non-root service execution
- minimal exposed ports and interfaces
- internal-only network exposure for services that do not need public access
- explicit authentication and authorization for administrative or state-changing endpoints
- secret delivery mechanisms over plain environment variables
- Docker secrets for sensitive values in Docker Compose when supported
- read-only filesystems or mounts when supported
- tmpfs for ephemeral writable paths when scratch space is needed
- secure defaults for cookies, sessions, and headers when exposing HTTP endpoints
- audit-friendly logging for security-relevant events

Do not:

- enable insecure protocols or plaintext administrative access by default
- expose administrative interfaces publicly without reason
- leave default credentials, anonymous access, or unauthenticated control paths enabled
- force hardening settings that break required runtime behavior without documenting the tradeoff

## Monorepo and package boundaries

When working in a monorepo or multi-package repository, respect package, ownership, and layer boundaries.

Required behavior:

- Follow established package boundaries, import rules, and ownership constraints.
- Keep shared utilities small, stable, and broadly justified.
- Prefer local dependencies over new cross-package coupling.
- Call out cross-boundary changes affecting more than one package, service, or ownership area.

Do not:

- introduce circular dependencies
- create cross-layer imports casually
- move code across package or ownership boundaries without explaining why
- turn a local need into a shared dependency without a clear present benefit

Guidance:

- Prefer duplication over the wrong shared package.
- Treat shared modules as higher-risk surfaces because they affect multiple dependents.

## Accessibility

For user-facing interfaces, prefer accessible defaults.

Required behavior:

- Preserve keyboard accessibility and semantic structure.
- Preserve readable labels, roles, and focus behavior.
- Avoid changes that reduce accessibility without explicit justification.

## Comments, naming, and terminology

Use comments and names to improve maintainability and reduce ambiguity.

Required behavior:

- Prefer comments that explain why, intent, constraints, invariants, or non-obvious tradeoffs.
- Update or remove comments when behavior changes.
- Prefer existing domain terms, architectural names, and user-facing vocabulary.
- Keep naming consistent across code, configuration, tests, and documentation.
- Align new names with existing repository and ecosystem conventions.

Do not:

- add comments that merely paraphrase obvious code
- leave outdated comments in place
- use comments as a substitute for clearer naming or simpler structure
- introduce synonyms for established concepts without reason
- rename embedded concepts casually
- use clever or ambiguous names when clearer existing terminology is available

## GitHub Actions and automation

When editing workflows or automation:

Required behavior:

- Prefer GitHub-hosted runners unless the repository explicitly requires self-hosted runners.
- Prefer least-privilege permissions.
- Keep `GITHUB_TOKEN` permissions minimal.
- Prefer pinned action versions.
- Prefer immutable pins when repository policy requires them.
- Prefer OIDC over long-lived cloud secrets when cloud authentication is involved.

Standard baseline for most code repositories unless clearly unnecessary:

- CI workflow that runs on pull requests
- `actions/checkout` when a workflow needs repository contents
- appropriate `actions/setup-*` runtime action for the repository language or toolchain
- dependency review for repositories with package dependencies
- code scanning or CodeQL default setup for supported languages or likely future support

Code scanning alerts:

- Prefer enabling GitHub code scanning alerts with CodeQL default setup when the repository is eligible.
- Use advanced CodeQL setup only when the repository needs custom languages, custom queries, custom
  build steps, custom CodeQL model packs, or non-default behavior.
- Preserve existing code scanning workflows, SARIF uploads, and security alert visibility.
- Treat disabling code scanning, weakening CodeQL coverage, or removing SARIF upload workflows as
  security-sensitive changes.
- Do not duplicate CodeQL default setup with an advanced CodeQL workflow unless the repository
  intentionally uses both default and advanced analysis.
- If code scanning cannot be enabled, clearly state why and what repository setting, plan,
  permission, language support, or workflow support is missing.

Prefer:

- one clear CI workflow over many fragmented workflows in smaller repositories
- required checks that are high-signal and relevant
- `merge_group` support when merge queue is enabled and required checks need it

Only add when there is clear payoff:

- artifact upload or download
- cache configuration
- labeler or triage automation
- release or publish workflows

Do not:

- add broad third-party actions without justification
- widen permissions without clear need
- bypass environment protections
- bypass deployment approvals
- add workflow complexity without a clear benefit

## Documentation

Update documentation when behavior changes, especially:

- README usage
- environment variables
- deployment steps
- configuration
- public API behavior
- migration notes
- release notes inputs
- runbooks and operational guidance

Documentation rules:

- Use plain language.
- Use short sentences and short paragraphs.
- Use active voice.
- Use imperative mood for procedures.
- Use sentence-case headings.
- Keep heading levels consistent.
- Do not skip heading levels.
- Start major sections with a short purpose statement when helpful.
- Use numbered lists for ordered steps.
- Use bullets for unordered guidance.
- Keep examples minimal, accurate, and copy-pasteable.
- Separate conceptual guidance, how-to steps, and reference material when practical.
- Prefer consistency over cleverness.
- Prefer repository-wide consistency tools such as `.editorconfig` when present.

Repository links:

- Use relative links for internal repository references.
- Keep links portable across branches, forks, and local clones.
- Do not use absolute GitHub blob or tree URLs for internal repository references unless a
  commit-pinned URL is explicitly needed.
- Use absolute links only for external resources, published documentation, releases, tags, or commit-pinned references.

Examples:

- `./docs/setup.md`
- `../CONTRIBUTING.md`

Avoid:

- long narrative introductions
- unnecessary jargon
- large walls of text
- unrelated concepts in the same section
- full-file examples when a smaller excerpt is enough

## Change risk classification

Treat changes affecting the following as higher risk:

- public APIs and interface contracts
- configuration contracts
- schemas and migrations
- authentication and authorization
- workflows and deployment
- secrets and security controls
- shared libraries and shared utilities
- concurrency, retries, and side-effecting workflows
- release artifacts and supply-chain controls
- privacy-sensitive data flows
- billing, payments, financial calculations, or audit-related behavior

## Sensitive areas

Treat these areas as high-risk:

- `.github/workflows/**`
- release automation
- dependency management
- authentication and permissions
- infrastructure and deployment configuration
- database migrations
- security-sensitive code
- `CODEOWNERS`
- repository policy documentation
- code scanning, CodeQL configuration, SARIF uploads, and security alert workflows

For sensitive areas:

- minimize scope
- explain risk
- recommend human review when impact is material

## Code review focus

Optimize review output for:

- correctness
- security
- reliability
- testability
- maintainability
- performance when clearly relevant

Call out:

- missing tests for important paths
- error-handling gaps
- unsafe defaults
- authentication or permission issues
- risky dependency changes
- risky workflow changes
- breaking API or schema changes
- compatibility and contract risk
- retry, timeout, and idempotency mistakes
- licensing or compliance-sensitive dependency changes
- performance regressions in hot paths

Do not spend review effort on:

- trivial formatting
- subjective naming preferences
- low-confidence style comments

## Human stop triggers

Stop and request human review before proceeding when a change involves high-risk uncertainty,
irreversible impact, or authority the agent does not have.

Stop before changing:

- repository policy, branch protection, rulesets, `CODEOWNERS`, or required checks
- authentication, authorization, permissions, roles, or access-control logic
- cryptography, secret handling, token handling, or credential storage
- production deployment workflows, release automation, or rollback behavior
- destructive database or data migrations
- public APIs, CLI behavior, configuration contracts, or file formats in a breaking way
- license, legal, privacy, or compliance-sensitive material
- security scanning, provenance, signing, SBOM, or attestation controls
- infrastructure affecting production, networking, DNS, certificates, storage, or backups
- billing, payments, financial calculations, or audit-related behavior
- data retention, deletion, export, or privacy-sensitive flows

Stop when:

- the requested change conflicts with documented architecture, accepted ADRs, or repository policy
- the change requires choosing between materially different architectural approaches
- correct behavior depends on unavailable credentials, secrets, production data, or external systems
- the change may cause data loss, downtime, privilege escalation, or broad service disruption
- validation cannot be performed and the unvalidated area is high risk
- dependency, license, or supply-chain concern cannot be confidently assessed
- the agent would need to invent requirements, compliance claims, credentials, or operational assumptions
- rollback path is unclear for a high-impact change
- the repository appears to need a new ADR, policy decision, or maintainer-level judgment

Required behavior:

- Do not continue by guessing through a human-stop condition.
- Clearly state what triggered the stop.
- Summarize the decision or approval needed.
- Provide the smallest safe next step or recommendation.
- Preserve any partial work that is safe and clearly mark it as incomplete.

Do not:

- bypass human review by reframing a high-risk change as a small implementation detail
- weaken safeguards to make the requested change easier
- proceed with destructive, security-sensitive, or policy-sensitive changes on assumptions
- imply that an unverified high-risk change is safe

## Agent instruction file portability

Use `AGENTS.md` as the canonical repository instruction file.

If a tool requires a tool-specific instruction file such as `CLAUDE.md`:

- make that file a thin shim to `AGENTS.md`
- do not maintain a separate, divergent copy of repository rules
- only add tool-specific instructions when they are required for that tool and cannot be expressed
  cleanly in `AGENTS.md`

Goal:

- one source of truth for repository instructions
- minimal duplication
- minimal drift across agent-specific files

## Response efficiency

Optimize for token efficiency without reducing correctness.

Required behavior:

- Be concise by default.
- Prefer the smallest complete answer or patch.
- Prefer targeted diffs, snippets, and changed sections over full-file rewrites.
- Give one best recommendation by default.
- Expand only when risk, tradeoffs, or ambiguity require it.

Do not:

- restate the task unless needed
- restate repository rules unless needed
- repeat prior context unless needed
- provide multiple alternatives when one strong recommendation is enough
- add long preambles or repeated summaries
- quote large blocks of code or documentation unless required
- perform broad scans when the relevant files are already known
- spawn subagents unless the task clearly requires them

For simple tasks:

- answer directly

For complex tasks:

- use a short plan
- then execute

## Escalation rules

Do not invent:

- credentials
- environment values
- undocumented architecture rules
- product requirements
- compliance claims

State assumptions explicitly when necessary.

Recommend human review when:

- repository policy may need to change
- architecture may need to change materially
- workflow or deployment risk is significant
- security impact is non-trivial
- correct behavior cannot be verified confidently

## Preferred output

Good output:

- small patch
- validated change
- concise rationale
- explicit risk notes
- no hidden side effects

Bad output:

- giant refactor
- unnecessary dependency churn
- workflow changes without explanation
- bypassing validation
- changing policy only to make CI pass
