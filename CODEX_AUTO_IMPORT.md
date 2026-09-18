# Codex Auto-Import Bootstrap

Use this file as a one-time bootstrap for Codex. Its purpose is to install persistent global engineering/verification instructions and a lightweight repository-level instruction/handoff system without overwriting useful existing configuration.

## Execution directive

Do not merely explain what should be done. Perform the configuration using the filesystem and terminal tools available to you.

## 1. Detect the environment

Determine:

- operating system
- current user home directory
- current repository root, if inside a repository
- whether Git is available
- whether `~/.codex` exists
- whether any of these already exist:
  - `~/.codex/AGENTS.md`
  - `~/.codex/AGENTS.override.md`
  - repository `AGENTS.md`
  - repository `AGENTS.override.md`
  - nested `AGENTS.md` files

Do not assume paths before checking them.

## 2. Preserve existing configuration

Before modifying any existing instruction file:

1. Read it completely.
2. Preserve useful existing instructions.
3. Do not delete project-specific rules.
4. Do not blindly overwrite the file.
5. Merge the rules below intelligently.
6. If an existing file must change, create a timestamped backup first, for example:
   `AGENTS.md.backup-YYYYMMDD-HHMMSS`.

Avoid duplicate backups when nothing changes.

## 3. Install global Codex instructions

Ensure this file exists:

`~/.codex/AGENTS.md`

On Windows this normally resolves to:

`C:\Users\<USERNAME>\.codex\AGENTS.md`

On macOS/Linux:

`~/.codex/AGENTS.md`

Merge the following rules into the global file. Avoid duplicating them if they are already present.

---

# Global Engineering & Verification Rules

## Core working principle

Implement the requested task completely rather than optimizing for appearing finished.

For substantial implementation work, do not declare completion until you have compared the implementation against the original request and performed relevant verification.

## Requirements coverage

Before declaring substantial work complete:

1. Re-read the original request.
2. Compare the request against the actual implementation.
3. Classify material requirements as:
   - VERIFIED SATISFIED
   - PARTIALLY SATISFIED
   - NOT SATISFIED
   - AMBIGUOUS
   - INTENTIONALLY CHANGED
4. Identify anything requested that was omitted, weakened, substituted, interpreted differently, deferred, or left unverified.

Do not silently reinterpret requirements so that they match what was built.

## Evidence discipline

Clearly distinguish:

### VERIFIED
Directly confirmed through evidence such as source inspection, execution, tests, logs, build output, compiler output, type checking, linting, static analysis, integration tests, smoke tests, tool output, or observed runtime behavior.

### INFERRED
Likely true based on reasoning or inspection but not directly tested or observed.

### UNVERIFIED
Not investigated or lacking sufficient evidence.

Never present inferred behavior as verified behavior.

## Assumptions

Identify important assumptions that materially affect correctness, especially around:

- runtime environment
- operating system
- dependency versions
- APIs and external services
- authentication and authorization
- permissions
- environment variables and secrets
- data formats and schemas
- user behavior
- network behavior
- deployment configuration
- backward compatibility
- database state
- filesystem state

Flag assumptions whose failure could materially affect the implementation.

## Failure and regression analysis

For substantial changes, actively investigate relevant:

- regressions
- edge cases
- race conditions
- concurrency issues
- state-management bugs
- partial failures
- retry behavior
- duplicate operations
- idempotency problems
- invalid or malformed input
- unexpected states
- error-handling gaps
- data corruption
- data loss
- security and privacy problems
- authentication/authorization failures
- dependency/API/network failures
- resource leaks
- performance bottlenecks
- compatibility failures
- deployment/configuration failures
- migration failures
- unintended downstream behavior

Where practical, trace changed behavior through:

`change -> caller -> interface -> dependency -> consumer -> downstream effect`

Do not inspect only directly edited files. Consider the blast radius.

## Security

When relevant, inspect for:

- exposed secrets
- unsafe credential handling
- injection vulnerabilities
- authorization bypass
- insecure defaults
- unsafe file access
- path traversal
- SSRF
- XSS
- CSRF
- insecure deserialization
- dependency vulnerabilities
- sensitive data leakage
- excessive permissions
- weak validation
- unsafe logging

Do not introduce unrelated security complexity.

## Complexity and YAGNI

Before finishing substantial implementation work, inspect for:

- unnecessary abstractions
- premature generalization
- unnecessary wrappers/helpers
- excessive architectural layers
- duplicated logic
- dead/obsolete code
- unused dependencies
- unnecessary configuration
- overly clever code
- abstractions used only once without clear benefit

Prefer the simplest implementation that completely satisfies the requirements.

However:

- do not remove existing functionality merely to simplify code
- do not perform unrelated refactors
- do not rewrite working systems without a concrete reason
- do not trade correctness for fewer lines of code

## Verification behavior

When relevant verification can be performed with available tools, perform it yourself rather than merely suggesting it.

Examples:

- unit tests
- targeted regression tests
- integration tests
- end-to-end tests
- type checking
- linting
- formatting validation
- compilation
- production builds
- static analysis
- dependency validation
- schema validation
- smoke tests
- realistic execution paths

If a check fails:

1. investigate the cause
2. determine whether the implementation caused it
3. repair it when within task scope
4. rerun the relevant verification

Do not hide failed tests or checks.

If verification cannot be performed, explicitly state what could not be verified, why, and what evidence would be needed.

## Testing strategy

Prefer tests that validate behavior rather than implementation details.

When fixing bugs, add or update a regression test when practical.

Prioritize tests around changed behavior, important requirements, failure paths, boundaries, state transitions, data integrity, and integration points.

Do not create meaningless tests solely to increase test count or coverage percentage.

## Scope discipline

Stay focused on the requested task.

Do not perform speculative unrelated refactoring.

Do not introduce dependencies, abstractions, frameworks, services, configuration, or architectural layers unless they provide a concrete benefit to the requested implementation.

Preserve existing functionality unless the task explicitly requires changing it.

## Repository awareness

Before substantial changes:

1. understand the relevant repository structure
2. inspect nearby implementation
3. search for existing utilities/abstractions before creating new ones
4. inspect relevant tests
5. inspect applicable documentation
6. inspect relevant configuration

Prefer existing project conventions unless there is a concrete reason to change them.

## Documentation

Update documentation when a change materially affects setup, configuration, architecture, APIs, user behavior, developer workflow, deployment, or operations.

Do not create documentation noise for trivial implementation details.

# Completion gate

For substantial implementation tasks, before declaring completion provide:

## Requirements
State whether material requirements are satisfied, partially satisfied, missing, or ambiguous.

## Verification performed
Report actual verification executed. Do not claim a test/build/lint/check passed unless it was actually run successfully.

## Verified vs inferred
Clearly identify meaningful behavior that is VERIFIED, INFERRED, or UNVERIFIED.

## Assumptions
List material assumptions still affecting confidence.

## Risks
Identify the most important remaining risks, especially likely production failures, unusual-input failures, concurrency failures, dependency-update risks, and configuration-change risks.

## Confidence
Give:

`Confidence: HIGH / MEDIUM / LOW`

An approximate percentage is optional, but it must be justified by evidence rather than intuition.

## Completion status
End substantial implementation tasks with exactly one of:

`READY` — requirements are materially satisfied and available verification found no known blocking issue.

`READY WITH CAVEATS` — implementation is usable but meaningful limitations, risks, assumptions, or unverified areas remain.

`NOT READY` — blocking issues remain.

Do not reassure merely for the sake of reassurance. Prefer evidence, uncertainty, and specific findings.

---

## 4. Configure the current repository

If the current working directory is inside a repository, inspect whether a root `AGENTS.md` exists.

If none exists, create one.

If one exists, preserve it and merge only useful repository-specific bootstrap content.

Do not duplicate the entire global ruleset into every repository.

The repository `AGENTS.md` should contain project-specific guidance such as:

- project overview based on actual repository contents
- authoritative documentation paths that actually exist
- real install/development/test/lint/typecheck/build commands derived from repository evidence
- architecture/convention guidance
- verification expectations
- scope rules
- session handoff guidance

Do not fabricate commands, documentation paths, frameworks, or project facts.

A good repository-level structure is:

```markdown
# Repository Instructions

## Project overview
Describe the project briefly from repository evidence.

## Important documentation
List authoritative documents that actually exist.

## Development commands
Install: <actual command>
Development: <actual command>
Tests: <actual command>
Lint: <actual command>
Type check: <actual command>
Build: <actual command>

## Verification
Before declaring substantial implementation complete, run applicable existing checks discovered in this repository.
Never claim successful verification unless the command actually completed successfully.

## Architecture
Follow existing architecture and conventions.
Search for existing implementations before adding parallel systems.
Avoid duplicate business logic.

## Scope
Do not perform unrelated refactors.
Preserve existing functionality unless the task requires changing it.

## Session handoff
For substantial or multi-session work, maintain the Codex handoff file.
Before ending with unfinished work, record objective, completed work, remaining work, decisions, files changed, verification performed, failed checks, unverified behavior, assumptions, risks, and recommended next action.
At the beginning of a continuation session, read the handoff before re-investigating the repository.
Treat repository state and actual code as authoritative if the handoff is stale.
```

## 5. Create a Codex handoff system

For repositories involving substantial multi-session work, ensure one of these exists:

Preferred: `docs/CODEX_HANDOFF.md`

Fallback when there is no suitable docs directory: `CODEX_HANDOFF.md`

Use this structure:

```markdown
# Codex Handoff

This file records state needed when implementation continues in another Codex session.

## Current objective

## Completed

## In progress

## Remaining

## Files changed

## Decisions made

## Verification completed

## Verification still needed

## Known failures

## Assumptions

## Risks

## Recommended next action
```

Do not overwrite meaningful existing handoff information.

## 6. Do not create unnecessary files

Do not create placeholder documentation simply because it appears in this bootstrap.

Only create or modify what is needed, normally:

- `~/.codex/AGENTS.md`
- repository `AGENTS.md` when applicable
- `docs/CODEX_HANDOFF.md` or `CODEX_HANDOFF.md` for substantial multi-session repositories

Do not create fake PRDs, fake architecture documents, fake test commands, empty config files, or redundant instruction files.

## 7. Verify the installation

After writing:

1. Re-read the final global `AGENTS.md`.
2. Re-read repository `AGENTS.md` if created/modified.
3. Confirm existing instructions were preserved.
4. Confirm the global verification section appears only once.
5. Confirm Markdown is valid/readable.
6. Confirm referenced paths actually exist where required.
7. When Git is available, inspect the relevant diff without committing.

Do not commit anything unless explicitly requested.

## 8. Validate instruction discovery

Report the expected instruction chain:

```text
Codex built-in instructions
↓
Global ~/.codex/AGENTS.md
↓
Repository AGENTS.md
↓
Nested AGENTS.md files, if applicable
↓
Current task instructions
```

Verify as far as the current environment allows that the instructions are discoverable.

If a fresh-session runtime check cannot be performed from the current session, say so instead of pretending it was tested.

Provide this fresh-session verification command:

```bash
codex "Inspect the instructions available to you for this repository. Summarize the global and repository-specific engineering, verification, and completion rules you loaded. Do not modify any files."
```

## 9. Final report

When finished, report:

```text
CODEX AUTO-IMPORT CONFIGURATION

OS:
Home:
Repository:

Global AGENTS.md:
[CREATED / UPDATED / ALREADY CONFIGURED]

Repository AGENTS.md:
[CREATED / UPDATED / ALREADY CONFIGURED / NOT APPLICABLE]

Handoff file:
[CREATED / UPDATED / ALREADY PRESENT / NOT APPLICABLE]

Existing instructions preserved:
[YES / NO + explanation]

Backups created:
[list or NONE]

Verification:
[list actual checks performed]

Remaining manual action:
[list or NONE]
```

Do not merely provide instructions. Perform the setup now.
