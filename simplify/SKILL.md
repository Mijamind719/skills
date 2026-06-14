---
name: simplify
description: Simplify code while preserving behavior. Use when asked to simplify code, clean up code, reduce complexity, refactor for clarity, improve readability, remove unnecessary abstraction, or review recently changed code for maintainability. Focus on small, project-convention-aware edits with relevant validation.
---

# Simplify

## Overview

Improve code clarity, consistency, and maintainability without changing what the code does. Prefer conservative, local edits that make the existing design easier to read and maintain.

## Operating Rules

### Preserve Behavior

- Keep public APIs, outputs, side effects, data formats, error behavior, and ordering semantics unchanged unless the user explicitly asks otherwise.
- Treat tests, snapshots, migrations, protocol schemas, and generated files as behavior contracts.
- Stop and explain the risk before editing when a simplification might change behavior.

### Keep Scope Local

- Start from the user-mentioned files or the current working tree diff.
- If the user did not specify a target, inspect recently changed code first.
- Avoid broad architecture rewrites, dependency swaps, file moves, public API redesigns, or style churn outside the simplification target.

### Follow The Project

- Read nearby code before editing.
- Follow local instructions such as `AGENTS.md`, existing naming, lint rules, tests, and framework idioms.
- Do not import conventions from another project or language ecosystem unless the repository already uses them.

### Simplify For Clarity

- Reduce unnecessary nesting, branching, indirection, and duplication.
- Remove abstractions that only rename one expression or obscure control flow.
- Inline tiny helpers when the call site becomes clearer.
- Extract helpers only when they remove real duplication, isolate a meaningful concept, or make the main flow easier to scan.
- Improve names when the better name is obvious from domain context.
- Remove comments that merely repeat the code; keep comments that explain non-obvious constraints.
- Prefer explicit readable code over clever compact code. Avoid dense one-liners and nested ternaries when they make code harder to debug.

### Do Not Over-Simplify

- Do not collapse distinct responsibilities into one function.
- Do not remove abstractions that encode domain concepts, preserve invariants, or provide useful extension points.
- Do not optimize for fewer lines at the expense of readability.
- Do not introduce new dependencies, frameworks, or formatting systems just to simplify.

## Workflow

1. Identify the target code from the prompt, current diff, or recent edits.
2. Read surrounding code, tests, and project guidance relevant to the target.
3. List low-risk simplification opportunities mentally; edit only the ones with clear benefit.
4. Make small, focused changes.
5. Re-read the diff and remove accidental churn.
6. Run the most relevant available validation, such as targeted tests, type checks, linters, or build commands.
7. Summarize what changed, why it is behavior-preserving, and what validation ran.

## Output Expectations

- Lead with the concrete simplification result.
- Mention files changed and validation run.
- If no safe simplification is available, say so and explain the limiting risk.
- If validation cannot run, state the reason and any residual risk.

## Good Simplification Targets

- Repeated conditionals that can share a clear helper.
- Branches that can return early to reduce indentation.
- Names that hide domain intent.
- Dead code, redundant wrappers, duplicate constants, or repeated literals.
- Overly compact expressions whose intermediate values deserve names.

## High-Risk Changes To Avoid By Default

- Changing concurrency, caching, retry, transaction, or authorization logic.
- Reordering side-effectful operations.
- Rewriting parsing, serialization, rounding, time zone, or locale behavior.
- Altering error messages that tests, users, logs, or integrations may rely on.
- Replacing established project patterns with personal preference.
