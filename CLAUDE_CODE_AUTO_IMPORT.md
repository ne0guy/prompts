# Claude Code Auto-Import Bootstrap

Use this file as a one-time bootstrap for Anthropic Claude Code.

Its purpose is to install persistent engineering, verification, risk, and completion-quality instructions so future Claude Code sessions inherit the same disciplined behavior automatically, while using Claude Code's native configuration model instead of Codex-specific mechanisms.

The core behavior must remain:

- compare implementation against the original request
- separate VERIFIED / INFERRED / UNVERIFIED
- surface assumptions
- analyze regressions, edge cases, race conditions, and production failure modes
- perform a YAGNI / complexity pass
- run available verification instead of merely recommending it
- report evidence-backed confidence
- end substantial work with READY / READY WITH CAVEATS / NOT READY
- preserve unfinished work cleanly across sessions

Do not merely explain this setup. Perform it using the filesystem and terminal tools available to you.

---

# 1. Detect the Claude Code environment

Determine:

- operating system
- current user home directory
- current repository root, if any
- Claude Code version, if available
- whether Git is available
- whether the repository is trusted/usable by Claude Code
- whether the following exist

User scope:

- `~/.claude/CLAUDE.md`
- `~/.claude/rules/`
- `~/.claude/settings.json`
- `~/.claude/skills/`

Project scope:

- `CLAUDE.md`
- `.claude/CLAUDE.md`
- `CLAUDE.local.md`
- `AGENTS.md`
- `.claude/rules/`
- `.claude/settings.json`
- `.claude/settings.local.json`
- `.claude/skills/`
- existing handoff/progress files

Also inspect for nested `CLAUDE.md`, `CLAUDE.local.md`, and `.claude/rules/` files that could affect the current working directory.

Do not guess paths before checking them.

---

# 2. Preserve existing configuration

Before modifying an existing Claude Code instruction/configuration file:

1. Read it fully.
2. Preserve useful existing instructions.
3. Do not delete project-specific rules.
4. Do not blindly overwrite `CLAUDE.md`, rules, settings, or skills.
5. Merge only what is useful.
6. Avoid duplicating an existing rule.
7. If an existing file must be materially modified, create a timestamped backup first.

Example:

`CLAUDE.md.backup-YYYYMMDD-HHMMSS`

Do not create backups when no change is made.

Do not edit managed organization policy files.

---

# 3. Install global Claude Code quality rules

Claude Code's native user-wide instruction locations are:

- `~/.claude/CLAUDE.md` for user instructions
- `~/.claude/rules/*.md` for modular user-level rules

Prefer a modular rule file so the user's main `CLAUDE.md` stays concise.

Ensure this file exists:

`~/.claude/rules/engineering-verification.md`

Merge the following rules into it without duplication.

---

# Global Engineering & Verification Rules

## Core working principle

Implement the requested task completely rather than optimizing for appearing finished.

For substantial implementation work, do not declare completion until the implementation has been compared against the original request and relevant verification has actually been performed.

Default to action when the user has clearly requested implementation. Use available tools to inspect, edit, run, test, and verify rather than only describing what the user could do.

Investigate before making codebase claims. Do not speculate about files, behavior, interfaces, or dependencies that have not been inspected when inspection is available.

## Requirements coverage

Before declaring substantial work complete:

1. Re-read the original request.
2. Compare it against the actual implementation.
3. Classify each material requirement as:
   - VERIFIED SATISFIED
   - PARTIALLY SATISFIED
   - NOT SATISFIED
   - AMBIGUOUS
   - INTENTIONALLY CHANGED
4. Identify anything omitted, weakened, substituted, deferred, reinterpreted, or left unverified.

Do not silently reinterpret the request to match what was built.

## Evidence discipline

Clearly distinguish:

### VERIFIED

Directly confirmed through concrete evidence such as:

- source inspection
- execution
- tests
- logs
- build/compiler output
- type checking
- linting
- static analysis
- integration checks
- smoke tests
- application interaction
- tool output
- observed runtime behavior

### INFERRED

Likely true based on reasoning or inspection but not directly tested or observed.

### UNVERIFIED

Not investigated or lacking sufficient evidence.

Never present inferred behavior as verified behavior.

## Assumptions

Identify assumptions that materially affect correctness, especially around:

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

Flag assumptions whose failure would materially affect the implementation.

## Failure and regression analysis

For substantial changes, actively examine relevant:

- regressions
- edge cases
- race conditions
- concurrency problems
- state-management problems
- partial failures
- retry behavior
- duplicate operations
- idempotency
- malformed or unexpected input
- unexpected state
- error-handling gaps
- data corruption
- data loss
- security/privacy issues
- authentication/authorization failures
- dependency failures
- API/network failures
- resource leaks
- performance bottlenecks
- compatibility failures
- deployment/configuration failures
- migration failures
- unintended downstream behavior

Where practical, trace:

`change -> caller -> interface -> dependency -> consumer -> downstream effect`

Do not inspect only files directly edited. Consider the blast radius.

## Security

When relevant, inspect for:

- exposed secrets
- unsafe credential handling
- injection vulnerabilities
- authorization bypass
- insecure defaults
- unsafe filesystem access
- path traversal
- SSRF
- XSS
- CSRF
- insecure deserialization
- dependency vulnerabilities
- sensitive-data leakage
- excessive permissions
- weak validation
- unsafe logging

Do not add unrelated security machinery.

## Complexity and YAGNI

Perform a complexity pass before finishing substantial implementation work.

Look for:

- unnecessary abstractions
- premature generalization
- unnecessary wrappers/helpers
- excessive layers
- duplicated logic
- dead or obsolete code
- unused dependencies
- unnecessary configuration
- overly clever code
- one-use abstractions without clear benefit

Prefer the simplest implementation that fully satisfies the request.

Do not:

- remove existing functionality merely to simplify
- refactor unrelated code
- rewrite working systems without a concrete reason
- add speculative flexibility for hypothetical future requirements
- trade correctness for fewer lines

## General-purpose correctness

Implement the actual logic, not a workaround designed only to pass known tests.

Do not hard-code values or special cases solely for the visible test suite.

If a test appears incorrect or inconsistent with the requirements, investigate and report that rather than gaming it.

## Verification behavior

When relevant verification is available, run it yourself rather than only recommending it.

Use whichever checks fit the repository, including:

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
- schema validation
- smoke tests
- realistic execution paths

Use Claude Code's built-in verification/run capabilities when they are appropriate and available, including the project's recorded verification recipe when one exists.

If a check fails:

1. investigate the cause
2. determine whether the implementation caused it
3. repair it when within scope
4. rerun the relevant verification

Do not hide failed checks.

If verification cannot be performed, state:

- what could not be verified
- why
- what evidence would be needed

## Testing strategy

Prefer behavioral tests over tests of internal implementation details.

When fixing bugs, add or update a regression test when practical.

Prioritize tests around:

- changed behavior
- important requirements
- failure paths
- boundaries
- state transitions
- data integrity
- integration points

Do not create meaningless tests solely to increase test count or coverage.

## Scope discipline

Avoid over-engineering.

Only make changes that are directly requested or clearly required for correctness.

Do not add unrelated:

- features
- refactors
- frameworks
- services
- dependencies
- configuration
- architectural layers
- helpers
- documentation

Preserve existing behavior unless the request requires changing it.

Clean up temporary scripts/files created only for investigation when they are no longer needed.

## Repository awareness

Before substantial changes:

1. understand the relevant repository structure
2. inspect nearby implementation
3. search for existing utilities and abstractions before creating new ones
4. inspect relevant tests
5. inspect applicable documentation
6. inspect relevant configuration
7. inspect callers/consumers when changing interfaces or shared logic

Prefer established project conventions unless there is a concrete reason to change them.

## Documentation

Update documentation when a change materially affects:

- setup
- configuration
- architecture
- APIs
- user-visible behavior
- developer workflow
- deployment
- operations

Do not create documentation noise for trivial implementation details.

# Completion Gate

For substantial implementation tasks, before declaring completion provide a concise final quality report containing:

## Requirements

State whether the material requirements are satisfied, partially satisfied, missing, ambiguous, or intentionally changed.

## Verification performed

Report only verification actually executed.

Never claim a test, build, lint, runtime check, or review passed unless it actually ran successfully.

## Verified vs inferred

Identify important behavior that remains:

- VERIFIED
- INFERRED
- UNVERIFIED

## Assumptions

List material assumptions still affecting correctness or confidence.

## Risks

Identify the most important remaining risks, especially what is most likely to fail:

- in production
- under unusual input
- under concurrency
- after dependency/API changes
- after configuration/deployment changes

## Confidence

Give:

`Confidence: HIGH / MEDIUM / LOW`

An approximate percentage is optional, but evidence must justify it.

Do not treat implementation completion or a green test suite alone as proof that the whole request is correct.

## Completion status

End substantial implementation tasks with exactly one of:

`READY`

Requirements are materially satisfied and available verification found no known blocking issue.

`READY WITH CAVEATS`

Implementation is usable, but meaningful limitations, risks, assumptions, or unverified areas remain.

`NOT READY`

Blocking issues remain.

Prefer evidence and explicit uncertainty over reassurance.

---

# 4. Keep the main user CLAUDE.md concise

Inspect `~/.claude/CLAUDE.md`.

Do not copy the entire quality ruleset into it if the rule file above is active.

If useful, add only a short non-duplicative pointer such as:

```markdown
# Personal workflow

Global engineering and verification behavior is maintained in `~/.claude/rules/engineering-verification.md`.

For substantial coding work, implement rather than merely suggest, investigate before making codebase claims, run relevant verification, and use the completion gate defined by the global rules.
```

If equivalent instructions already exist, leave the file unchanged.

---

# 5. Configure the current repository

If the current directory is inside a repository, inspect its existing instruction system.

Claude Code natively reads `CLAUDE.md`, not `AGENTS.md`.

## If AGENTS.md already exists

Prefer sharing the existing cross-agent instructions rather than copying them.

If there is no project `CLAUDE.md`, create one that begins with:

`@AGENTS.md`

Then add only Claude-specific project guidance below it.

If a project `CLAUDE.md` already exists, preserve it. Add the `@AGENTS.md` import only when it will not create contradictory or duplicate instructions.

Do not replace a mature Claude-specific configuration merely because an AGENTS.md exists.

## If no project instructions exist

Create a concise root `CLAUDE.md` or `.claude/CLAUDE.md`.

Prefer root `CLAUDE.md` unless the repository already standardizes on `.claude/CLAUDE.md`.

Derive project facts from the repository rather than guessing.

Use a structure like:

```markdown
# Project Instructions

## Project overview
Brief evidence-based description of this repository.

## Important documentation
List authoritative files that actually exist.

## Development commands
Install: <actual command>
Development: <actual command>
Tests: <actual command>
Lint: <actual command>
Type check: <actual command>
Build: <actual command>

## Architecture
Follow existing architecture and conventions.
Search for existing implementations before adding parallel systems.
Avoid duplicate business logic.

## Verification
Run the relevant existing checks before declaring substantial changes complete.
Use the project verification recipe when available.
Do not claim successful verification unless it actually ran.

## Scope
Do not perform unrelated refactors.
Preserve existing functionality unless the requested task requires changing it.

## Session handoff
For substantial unfinished work, maintain the project handoff file.
```

Do not fabricate commands, documentation paths, frameworks, or project facts.

---

# 6. Use Claude Code rules for project-specific detail

For larger repositories, prefer modular files under:

`.claude/rules/`

instead of turning the main project `CLAUDE.md` into a large handbook.

Use descriptive files such as:

- `.claude/rules/testing.md`
- `.claude/rules/security.md`
- `.claude/rules/api.md`
- `.claude/rules/frontend.md`

Only create these when the repository genuinely needs them.

Use path-scoped rule frontmatter when a rule only applies to specific files.

Example:

```markdown
---
paths:
  - "src/api/**/*.ts"
---

# API rules

- Follow the repository's existing request validation pattern.
- Preserve the standard API error format.
```

Do not create path rules without repository evidence.

---

# 7. Preserve cross-session state intelligently

Claude Code has auto memory, but auto memory is machine-local and should not be treated as the sole durable handoff mechanism for important multi-session repository work.

For substantial work that may continue in another session or another machine, ensure this file exists:

Preferred:

`docs/CLAUDE_HANDOFF.md`

Fallback:

`CLAUDE_HANDOFF.md`

Use:

```markdown
# Claude Code Handoff

This file records durable implementation state for work that may continue in another session or machine.

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

Add a concise repository instruction stating:

- read the handoff before re-investigating substantial unfinished work
- update it before ending a session with substantial unfinished work
- repository state and actual code are authoritative if the handoff becomes stale
- use auto memory for useful local learnings/preferences, not as a replacement for repository-visible state

---

# 8. Respect Claude Code's native memory model

Do not manually edit auto-memory files merely to duplicate rules already present in `CLAUDE.md` or `.claude/rules/`.

Auto memory is for learned preferences, corrections, ongoing context, and useful project information that cannot simply be derived from the repository.

Permanent behavioral rules belong in `CLAUDE.md` or rules files.

Durable team/project state belongs in repository files.

---

# 9. Use skills instead of bloating persistent context

Claude Code supports reusable skills whose full instructions load only when needed.

If this repository contains a repeated multi-step procedure that is too large or too specialized for `CLAUDE.md`, consider creating a project skill under:

`.claude/skills/<skill-name>/SKILL.md`

Examples:

- release verification
- migration workflow
- deployment procedure
- security review
- incident-response checklist

Do not automatically convert ordinary rules into skills.

Do not create a skill unless there is a real repeated procedure.

Prefer Claude Code's built-in `/verify`, `/run`, `/debug`, and `/code-review` capabilities when they already fit the need.

If the project's run/verify process is non-standard and Claude Code supports recording a project verification recipe, use that mechanism rather than repeatedly rediscovering the same workflow.

---

# 10. Do not add enforcement hooks unless justified

Claude Code hooks can enforce behavior more strongly than instructions, but they also change tool execution and can create surprising failures.

Do not install new hooks merely because this bootstrap mentions them.

Only add or modify hooks when:

- the repository already uses them and the change is clearly appropriate, or
- the user explicitly requests hard enforcement

Behavioral quality guidance belongs in `CLAUDE.md` / rules by default.

---

# 11. Do not create unnecessary files

Do not create placeholder files simply because they are mentioned here.

Normally, this bootstrap may create or update only:

- `~/.claude/rules/engineering-verification.md`
- optionally `~/.claude/CLAUDE.md`
- project `CLAUDE.md` when applicable
- `.claude/rules/*.md` only when justified
- `docs/CLAUDE_HANDOFF.md` or `CLAUDE_HANDOFF.md` for substantial multi-session repositories

Do not create:

- fake PRDs
- fake architecture docs
- fake commands
- unused skills
- unused hooks
- empty configuration files
- redundant copies of instructions

Do not change permission settings, model selection, authentication, MCP configuration, or organization policy unless explicitly requested.

---

# 12. Verify the installation

After configuration:

1. Re-read the final global rule file.
2. Re-read any user `CLAUDE.md` that changed.
3. Re-read the project `CLAUDE.md` that changed.
4. Confirm pre-existing instructions were preserved.
5. Confirm the verification rules are not duplicated.
6. Confirm referenced files and commands actually exist where required.
7. Confirm Markdown/frontmatter syntax is valid.
8. Inspect the Git diff for repository files when Git is available.
9. Do not commit anything unless explicitly requested.

If this session can inspect Claude Code's loaded context, verify that the relevant instruction files are loaded.

The user can also run:

`/context`

and confirm the expected memory/instruction files appear.

---

# 13. Expected Claude Code instruction hierarchy

Report the discovered/expected instruction sources relevant to this repository, including:

```text
Managed organization CLAUDE.md/policy, if any
↓
User ~/.claude/CLAUDE.md
+ user ~/.claude/rules/*.md
↓
Repository/ancestor CLAUDE.md and CLAUDE.local.md
+ project .claude/rules/*.md
↓
Nested CLAUDE.md / rules loaded for relevant subdirectories
↓
Auto memory
↓
Current task instructions
```

Do not claim one file mechanically overrides another when Claude Code actually concatenates instruction sources. Identify contradictions if found.

---

# 14. Fresh-session verification

Provide the user with these checks after installation.

Inside Claude Code:

```text
/context
```

Confirm the expected `CLAUDE.md` and rule files appear in the loaded memory/instruction context.

Then ask:

```text
Inspect the instructions currently loaded for this repository. Summarize the user-level and project-level engineering, verification, scope, and completion rules you are following. Do not modify any files.
```

If a direct fresh-session check cannot be performed from the current session, state that clearly.

---

# 15. Final report

When finished, report:

```text
CLAUDE CODE AUTO-IMPORT CONFIGURATION

Claude Code version:
OS:
Home:
Repository:

Global verification rule:
[CREATED / UPDATED / ALREADY CONFIGURED]

User CLAUDE.md:
[UPDATED / ALREADY CONFIGURED / NOT NEEDED]

Project CLAUDE.md:
[CREATED / UPDATED / ALREADY CONFIGURED / NOT APPLICABLE]

AGENTS.md interoperability:
[IMPORTED / NOT PRESENT / NOT NEEDED / CONFLICT FOUND]

Project rules:
[CREATED / UPDATED / ALREADY PRESENT / NOT NEEDED]

Handoff file:
[CREATED / UPDATED / ALREADY PRESENT / NOT APPLICABLE]

Existing instructions preserved:
[YES / NO + explanation]

Backups created:
[list or NONE]

Verification:
[list actual checks performed]

Potential conflicts:
[list or NONE]

Remaining manual action:
[list or NONE]
```

Do not merely describe the setup.

Perform it now.
