---
name: implicit-state-machines
description: Find implicit state machines (groups of booleans, nullables or strings that together encode one state), check them for bugs with concrete failure scenarios, and triage each one (fix now, open an issue, or leave). Reports first; issues and fixes only on request. Use when the user asks for implicit state machines, a state audit, flag soup, boolean explosion, impossible or invalid state combinations, "make illegal states unrepresentable", or replacing booleans with enums or sealed types.
---

# Implicit state machines

An implicit state machine is a group of fields that together encode one
state. For example, `loading` + `error` + `data` encode "idle, loading,
ready, failed". Some combinations are invalid, many places write the
fields, and the compiler cannot check them.

This skill finds these groups, checks them for bugs, and sorts each one
into "fix now", "open an issue" or "leave". It changes nothing until the
user decides.

## Arguments

`/implicit-state-machines [path ...]`

- With paths: scan only those files or folders. Read callers and tests
  outside them only to check a bug or a hazard.
- With no path: scan the whole repository. First rank the source files
  by state density: mutable fields, state writes (for example
  `setState`, assignments to fields), and boolean or nullable fields.
  Scan the densest files first. Skip generated, vendor and build folders.

Say in the report which scope you scanned and what you skipped.

## Phases

1. **Scan** (read-only). Change no files.
2. **Report.** Write the report. Stop and wait for the user.
3. **Decide.** Resolve the questions with the user.
4. **Record** (only on request). Create issues and comments.
5. **Apply** (only on request). Fix and refactor in separate PRs.

Do not start a phase that needs a request until the user asks for it.

## Phase 1: Scan

### Before you scan

Read the project rules (AGENTS.md, CLAUDE.md, CONTRIBUTING.md, or
similar). Follow their writing, ask-first and comment rules. When a
proposal conflicts with a rule, turn the conflict into a question.

Read `references/detection.md` for the signals to look for.
Read `references/targets.md` for the target type in this language.

### Find the candidates

For each candidate, record: `file:line`, the fields, the states they
encode, the writers (count and lines), the readers (count), and the
invalid combinations.

Also list the state holders that are already explicit (enums, sealed
types, state classes). The report names them in one line, so the reader
knows you checked them.

### Large scopes

If the scope has more than about 30 source files or 10,000 lines, split
the scan by module. Give each subagent one module, the signal list from
`references/detection.md`, and this output format for each candidate:

```text
file:line | fields | states it encodes | writers (count, lines) |
readers (count) | invalid combinations
```

Subagents only find candidates. Run the bug check, the hazard check and
the triage yourself, on the merged list. Read the code for every
candidate that you report as a bug.

Below the threshold, do the whole scan yourself.

### Bug check (required)

For each state machine, list the invalid combinations and the writers
without a guard. For each one, try to build a failure scenario:
input or event sequence → wrong state → what the user sees.

Report a bug only with a scenario. Give each bug a confidence:

- **Confirmed**: the code path is clear from reading.
- **Likely**: it depends on library or platform behavior that you did
  not check. Name what to check.
- **Rare**: confirmed, but it needs an uncommon trigger. Name the
  trigger.

For each bug, name the regression test that reproduces it, and the
existing test helper it can use. Do not fix it in this phase.

List the rejected candidates in one line each: "checked, cannot happen
because ...".

Look for shared causes. Two bugs often come from one unclear meaning,
for example two different checks for "who owns the player".

### Hazard check (required)

Find what makes a rename or a type change unsafe:

- dynamic, reflective or string-keyed access to state (tests included);
- persisted or serialized names (stored keys, enum names or indexes);
- tests that CI does not run (device, integration, manual);
- public API or native bridge names.

For each hazard, give the count and the files. Name the Now items it
affects. An item that a hazard affects without a check is not Now.

### Triage

Give each finding one verdict and a one-line reason:

- **Now**: no behavior change, existing tests cover it, no device or
  integration run is necessary, and no product decision is open.
- **Issue**: it needs a product or UX decision, touches a product rule,
  needs a device or integration run, or is part of a larger refactor.
  Name the target issue (existing or new).
- **Leave**: small and readable, the change costs more than it gains,
  or strong tests cover it. Name the reason. You can propose
  documentation, for example an ASCII state diagram.

Give each Now item a risk: low, medium or high.

## Phase 2: Report

Write the report in the chat. Use `references/report-template.md`.

Then offer in one line to save it as a file. Write a file only when the
user asks, or when the project rules name a place for audit reports.
Do not commit it unless the user asks.

End the report with the questions. Then stop.

## Phase 3: Decide

Resolve each question with the user. Record each decision with its date.

A decision can move a verdict, for example from Issue to Now. When it
does, update the report and say what moved.

## Phase 4: Record (only on request)

Before you write, read the repository's labels and one or two recent
issues. Match their title prefix, sections and labels. Ask for any
label or priority that the user did not name.

1. **Main issue**: use `references/issue-template.md`.
2. **Follow-up issues**: one for each Issue verdict with no existing
   target.
3. **Umbrella comment**: when an existing issue covers a group of
   findings, comment there instead of opening a new issue.
4. **Links**: update the main issue with links to the new issues. Tick
   the items that are done.

Each step is an external action. Get approval for each one. Before you
post, check the counts and the lists in the draft against the code
again.

## Phase 5: Apply (only on request)

Work in separate PRs, in this order:

1. **Bug fixes without open decisions.** For each bug: write the
   regression test, run it and see it fail, fix the code, see it pass.
2. **Now items.** No behavior change. Existing tests must pass with no
   changes to their expectations. Update the test references that a
   hazard affects.
3. **Bug fixes that needed a decision in phase 3.** Test-first, as in
   step 1.

Run the project checks for each PR. Report each command and its result.
Name each check that you could not run, and the reason (for example,
no device).

Do not touch Issue or Leave items. Do not commit, push or open a PR
without the user's request.

## Writing

Follow the project's writing rules for the report, issues, comments and
commit messages. If the project has none, write short, literal
sentences in the active voice. Introduce each project term before you
use it. Prefer counts and `file:line` over adjectives.
