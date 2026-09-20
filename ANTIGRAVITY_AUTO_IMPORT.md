# Antigravity Auto-Import Bootstrap

Use this file as a one-time bootstrap for Google Antigravity 2.0, Antigravity IDE, and Antigravity CLI.

Its purpose is to install persistent engineering, verification, risk, and completion-quality instructions so future Antigravity sessions inherit the same disciplined behavior automatically.

The core behavior must remain identical across Antigravity surfaces and Gemini models used within Antigravity:

- compare implementation against the original request
- separate VERIFIED / INFERRED / UNVERIFIED
- surface important assumptions
- analyze regressions, edge cases, race conditions, failure modes, and production risks
- perform a YAGNI / complexity pass
- run available verification instead of merely recommending it
- report evidence-backed confidence
- end substantial work with READY / READY WITH CAVEATS / NOT READY
- preserve unfinished work cleanly across sessions and machines
- avoid duplicating rules across global, workspace, skill, and hook layers

Do not merely explain this setup. Perform it using the filesystem, terminal, repository, and Antigravity tools available to you.

---

# 1. Detect the Antigravity environment

Determine:

- operating system
- current user home directory
- current repository/workspace root, if any
- whether Git is available
- Antigravity surface currently in use:
  - Antigravity 2.0
  - Antigravity IDE
  - Antigravity CLI
- Antigravity version, if available
- whether the workspace is trusted/usable
- whether any of the following already exist

Global:

- `~/.gemini/GEMINI.md`
- `~/.gemini/config/`
- `~/.gemini/config/agents/`
- `~/.gemini/config/hooks.json`
- `~/.gemini/antigravity-cli/`
- `~/.gemini/antigravity-cli/rules/`
- `~/.gemini/antigravity-cli/settings.json`
- globally installed Antigravity skills/plugins/rules

Workspace/repository:

- `.agents/rules/`
- `.agent/rules/` legacy compatibility
- `.agents/skills/`
- `.agent/skills/` legacy compatibility
- `.agents/agents/`
- `.agents/hooks.json`
- `AGENTS.md`
- `GEMINI.md`
- existing handoff/progress files
- existing project-specific rule files
- existing skills and custom agents

Do not guess paths before checking them.

---

# 2. Preserve existing configuration

Before modifying any existing instruction, rule, hook, skill, or agent definition:

1. Read it completely.
2. Preserve useful existing instructions.
3. Do not delete project-specific rules.
4. Do not blindly overwrite `GEMINI.md`, `.agents/rules`, skills, agents, hooks, or settings.
5. Merge only what is useful.
6. Avoid duplicating equivalent rules in multiple layers.
7. If an existing file must be materially modified, create a timestamped backup first.

Example:

`GEMINI.md.backup-YYYYMMDD-HHMMSS`

or

`engineering-verification.md.backup-YYYYMMDD-HHMMSS`

Do not create backups when no change is made.

Do not edit organization-managed policy or centrally managed configuration.

---

# 3. Install the global Antigravity quality layer

Antigravity uses:

`~/.gemini/GEMINI.md`

for global rules that apply across workspaces.

Ensure this file exists.

Keep it concise enough to remain useful as always-loaded context.

If it already contains meaningful instructions, preserve and merge rather than replacing them.

Add the following global engineering and verification behavior unless equivalent rules already exist.

---

# Global Engineering & Verification Rules

## Core working principle

Implement the requested task completely rather than optimizing for appearing finished.

For substantial implementation work, do not declare completion until the implementation has been compared against the original request and relevant verification has actually been performed.

When the user has clearly requested implementation, use available tools to inspect, edit, execute, test, and verify rather than merely describing what could be done.

Investigate before making codebase claims. Do not speculate about files, behavior, interfaces, dependencies, or configuration when inspection is available.

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

Do not silently reinterpret the request so that it matches what was built.

## Evidence discipline

Clearly distinguish:

### VERIFIED

Directly confirmed through concrete evidence such as:

- source inspection
- execution
- tests
- logs
- build output
- compiler output
- type checking
- linting
- static analysis
- integration checks
- smoke tests
- application interaction
- browser verification
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
- idempotency problems
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

Do not inspect only directly edited files. Consider blast radius.

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
- perform unrelated refactors
- rewrite working systems without a concrete reason
- add speculative flexibility for hypothetical future requirements
- trade correctness for fewer lines of code

## General-purpose correctness

Implement the actual logic, not a workaround designed only to satisfy known tests.

Do not hard-code values or special cases solely for the visible test suite.

If a test appears incorrect or inconsistent with requirements, investigate and report it rather than gaming it.

## Verification behavior

When relevant verification is available, perform it rather than merely recommending it.

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
- dependency validation
- schema validation
- smoke tests
- browser checks
- realistic execution paths

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

Prefer behavioral tests over tests of implementation details.

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

Only make changes directly requested or clearly required for correctness.

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

Never claim a test, build, lint, runtime check, browser check, or review passed unless it actually ran successfully.

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

# 4. Use workspace rules for project-specific behavior

Antigravity workspace rules belong in:

`.agents/rules/`

Use the modern `.agents/rules/` path by default.

Only retain or modify `.agent/rules/` when the repository already uses the legacy path and changing it would be disruptive.

Do not duplicate the entire global ruleset in the repository.

Instead, create or update a concise project rule such as:

`.agents/rules/project-engineering.md`

with project-specific facts derived from repository evidence.

Include only what is relevant, such as:

- project overview
- authoritative documentation
- actual install command
- actual development command
- actual test command
- actual lint command
- actual type-check command
- actual build command
- architectural constraints
- project-specific security requirements
- test strategy
- completion checks
- deployment-specific checks

Never invent commands, paths, frameworks, services, or project facts.

If the repository already has appropriate rules, preserve and extend them rather than creating a redundant file.

Prefer an Always On project engineering rule only when its content truly applies to all work in the repository.

Use Model Decision or Glob activation for specialized rules when appropriate.

---

# 5. Interoperate with AGENTS.md and GEMINI.md

If the repository already contains `AGENTS.md`, inspect it before creating overlapping Antigravity rules.

Do not blindly copy it.

Where useful, reference shared repository instructions using Antigravity rule file references such as:

`@AGENTS.md`

when supported by the active rule file and path resolution.

If the repository contains a project `GEMINI.md`, preserve it.

Avoid maintaining conflicting copies of the same policy in:

- `AGENTS.md`
- `GEMINI.md`
- `.agents/rules/`

Prefer one canonical source for shared project facts and lightweight references from Antigravity rules where practical.

---

# 6. Preserve cross-session state

For substantial work that may continue across Antigravity sessions, machines, models, or subagents, maintain a repository-visible handoff file.

Preferred:

`docs/AGENT_HANDOFF.md`

Fallback:

`AGENT_HANDOFF.md`

Use:

```markdown
# Agent Handoff

This file records durable implementation state for work that may continue in another Antigravity session, model, agent, or machine.

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

Add a concise project rule stating:

- read the handoff before re-investigating substantial unfinished work
- update it before ending substantial unfinished work
- actual repository state and code are authoritative if the handoff becomes stale
- never use the handoff as a substitute for inspecting current code

---

# 7. Use Antigravity Skills for reusable procedures

Antigravity skills belong in:

`.agents/skills/<skill-name>/SKILL.md`

Use a skill when a repeated multi-step procedure is too specialized or too large to keep permanently loaded as a rule.

Examples:

- release verification
- deployment validation
- security review
- migration procedure
- incident investigation
- repository health audit
- browser-based acceptance testing

Do not automatically convert ordinary rules into skills.

Do not create a skill unless there is a real reusable workflow.

A skill should contain:

- clear trigger/usage description
- inputs
- ordered procedure
- required verification
- failure handling
- expected output
- scripts/resources only when useful

Keep always-on behavioral principles in Rules, not Skills.

---

# 8. Use custom subagents only when they provide real leverage

Antigravity custom agents can be defined at workspace level under:

`.agents/agents/<agent-name>/agent.md`

Use custom subagents when work benefits from isolated or parallel specialization.

Examples:

- code reviewer
- security auditor
- test investigator
- migration verifier
- dependency researcher

Do not create custom agents merely to make the configuration appear sophisticated.

For complex tasks, Antigravity may delegate parallel work to subagents.

When using subagents:

- give them a narrow objective
- ensure they inspect current repository state
- avoid duplicated work
- require evidence for findings
- reconcile results in the parent agent
- do not let a subagent's conclusion substitute for final parent verification
- use isolated branches/worktrees when concurrent edits could conflict

Do not assume a subagent inherits the parent conversation history.

Provide required task context explicitly.

---

# 9. Use hooks only for deterministic enforcement

Antigravity hooks can be configured at workspace level in:

`.agents/hooks.json`

and globally in supported Antigravity configuration.

Do not install new hooks merely because this bootstrap mentions them.

Use hooks only when deterministic enforcement provides a clear benefit, such as:

- mandatory formatting after edits
- a required linter after file changes
- a pre-command policy check
- structured diagnostics capture

Do not use hooks to encode nuanced reasoning instructions that belong in Rules.

Do not add expensive hooks that run large test suites after every trivial edit.

Do not modify existing hooks without understanding their blast radius.

---

# 10. Prefer rules over duplicated prompt text

Do not paste the complete engineering checklist into every task prompt.

The intended hierarchy is:

```text
Global ~/.gemini/GEMINI.md
↓
Workspace .agents/rules/*.md
↓
Relevant .agents/skills/*/SKILL.md
↓
Relevant custom agents/subagents
↓
Current user task
```

Use the task prompt for the actual objective and task-specific constraints.

Use Rules for persistent behavior.

Use Skills for reusable procedures.

Use custom agents for specialized or parallel execution.

Use Hooks for deterministic enforcement.

---

# 11. Do not create unnecessary files

Do not create placeholder files simply because they are mentioned here.

Normally this bootstrap may create or update only:

- `~/.gemini/GEMINI.md`
- `.agents/rules/project-engineering.md` when applicable
- `docs/AGENT_HANDOFF.md` or `AGENT_HANDOFF.md` for substantial multi-session work
- `.agents/skills/.../SKILL.md` only when a genuine repeated workflow exists
- `.agents/agents/.../agent.md` only when a genuine specialist agent is useful
- `.agents/hooks.json` only when deterministic enforcement is justified

Do not create:

- fake PRDs
- fake architecture docs
- fake test commands
- unused skills
- unused subagents
- unused hooks
- empty configuration files
- redundant rule files
- duplicate instructions

Do not change authentication, account configuration, model selection, MCP servers, permissions, sandbox policy, plugin configuration, or organization policy unless explicitly requested.

---

# 12. Verify the installation

After configuration:

1. Re-read the final global `~/.gemini/GEMINI.md`.
2. Re-read any workspace rules created or modified.
3. Re-read the handoff file if created.
4. Confirm pre-existing instructions were preserved.
5. Confirm rules are not duplicated unnecessarily.
6. Confirm all referenced files and commands actually exist where required.
7. Confirm Markdown and any frontmatter/activation metadata are valid.
8. Inspect Git diff for repository files when Git is available.
9. Do not commit repository changes unless explicitly requested.

If the Antigravity surface provides rule/customization inspection, verify that the relevant rules are loaded or discoverable.

For Antigravity CLI, inspect available rules/skills/agents through its native interfaces when possible.

---

# 13. Verify Antigravity-specific capabilities rather than assuming them

When configuring this environment, detect which of the following are actually available in the installed Antigravity version:

- Rules
- Skills
- custom agents
- subagents
- hooks
- plugins
- MCP
- browser tooling
- planning
- task manager
- multi-agent/teamwork features

Use only features supported by the installed version and account.

Do not fabricate or silently depend on unavailable slash commands or capabilities.

If an optional feature is unavailable, preserve the core behavior using Rules + repository files.

---

# 14. Fresh-session verification

After setup, provide an exact fresh-session verification prompt:

```text
Inspect the persistent Antigravity instructions currently available for this workspace.

Summarize:
1. the global engineering and verification rules you loaded,
2. the workspace-specific rules you loaded,
3. any relevant skills or custom agents available,
4. the completion gate you are expected to follow.

Do not modify any files.

Clearly distinguish instructions you actually discovered from instructions you infer should exist.
```

If Antigravity CLI is available, also suggest inspecting:

- `/agents` for custom/subagents
- `/skills` for loaded skills
- `/config` or `/settings` for relevant configuration
- applicable rules/customization surfaces supported by the current release

Do not claim a fresh-session test was performed if the current session cannot actually start one.

---

# 15. Final report

When finished, report:

```text
ANTIGRAVITY AUTO-IMPORT CONFIGURATION

Antigravity surface:
Antigravity version:
OS:
Home:
Repository:

Global GEMINI.md:
[CREATED / UPDATED / ALREADY CONFIGURED]

Workspace rules:
[CREATED / UPDATED / ALREADY CONFIGURED / NOT APPLICABLE]

AGENTS.md interoperability:
[REFERENCED / PRESERVED / NOT PRESENT / NOT NEEDED / CONFLICT FOUND]

Project GEMINI.md:
[PRESERVED / UPDATED / NOT PRESENT / NOT NEEDED]

Handoff file:
[CREATED / UPDATED / ALREADY PRESENT / NOT APPLICABLE]

Skills:
[CREATED / EXISTING / NOT NEEDED]

Custom agents:
[CREATED / EXISTING / NOT NEEDED]

Hooks:
[UPDATED / EXISTING / NOT NEEDED]

Existing instructions preserved:
[YES / NO + explanation]

Backups created:
[list or NONE]

Verification:
[list actual checks performed]

Potential conflicts:
[list or NONE]

Unavailable optional features:
[list or NONE]

Remaining manual action:
[list or NONE]
```

Do not merely describe the setup.

Perform it now.
