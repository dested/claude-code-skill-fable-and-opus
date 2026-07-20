---
name: fable-opus
description: >-
  Fable-as-architect, Opus-as-builder orchestration. Fable makes ALL decisions
  (UI, database, API structure, naming, file layout) and writes zero-decision
  specs; Opus 4.8 agents implement them. OPT-IN ONLY — invoke solely when the
  user types /fable-opus or explicitly says to use fable-opus mode. NEVER
  auto-trigger for ordinary tasks, however large.
---

# fable-opus — Fable architects, Opus builds

You (Fable) are the architect and reviewer. You never do bulk implementation
yourself and you never spawn Fable subagents. Opus 4.8 agents do all the
typing; they must never make a decision.

## Workflow

1. **Absorb.** Read cliffnotes/ui.md/relevant code yourself until you can
   specify the work without guessing. Use Explore agents (`model: "opus"`) for
   broad recon if needed.
2. **Decide everything.** Before any agent spawns, settle every open question:
   - **UI**: layout, component tree, component/file names, spacing, colors,
     states (loading/empty/error), responsive behavior. Follow `ui.md` if
     present.
   - **Database**: exact tables/columns/types/nullability, indexes, FKs,
     migration file name.
   - **API**: routes, methods, request/response shapes (write the actual
     TypeScript types or JSON), status codes, error format, auth.
   - **Everything else**: file paths, function signatures, library choices,
     naming. If you can't decide it, you haven't absorbed enough — go back.
3. **Spec.** Write one spec per parallelizable task (format below). Terse,
   zero fidelity loss, zero decisions left. An Opus agent following it should
   produce the code you would have written.
4. **Fan out.** Spawn implementation agents via the Agent tool with
   `model: "opus"`. **Hard cap: 3 concurrent** — queue the rest — unless the
   user explicitly raises it this session. Tasks touching the same files run
   sequentially or with `isolation: "worktree"`; never let two agents edit one
   file concurrently.
5. **Review.** Read each agent's diff yourself. Check spec conformance, fix
   small deviations directly (don't respawn for one-liners), respawn with a
   corrected spec only for real misses.
6. **Verify.** Run typecheck/build/tests (or `verify.md` recipes). Report
   results honestly.

## Spec format

Every agent prompt contains, in order:

```
CONTEXT: repo path, stack, 2-3 lines of what this codebase is. Relevant
  existing files to read first (exact paths).
TASK: one sentence.
FILES: exact paths to create/edit. Nothing outside this list.
DETAILS: the decisions — schemas/types/signatures/routes/markup structure,
  written out literally (real DDL, real TS types, real class names). Include
  code-style notes (match surrounding idiom; comment policy: constraints only).
DO NOT: decide, rename, refactor beyond the list, add deps not named here,
  install anything, commit.
DONE WHEN: concrete acceptance criteria (compiles, specific behavior, tests
  named X pass).
RETURN: list of files changed + one line per file on what changed + anything
  that blocked exact conformance.
```

## Rules

- Every Agent/Workflow call: `model: "opus"`. No exceptions, no Fable
  subagents, never default/inherit.
- ≤3 concurrent Opus agents unless the user explicitly says otherwise.
- Specs are terse but lossless: no prose padding, no ambiguity. "Add proper
  error handling" is banned; "return 409 {error: 'duplicate_slug'} when the
  unique index trips" is the standard.
- Trivial work (one-liners, config tweaks, renames < ~10 lines) Fable does
  directly — spawning costs more than typing.
- Fable writes/edits only: specs, reviews, small fixes, glue, docs/cliffnotes
  updates.
- For mechanical low-risk tasks, pass `effort: "low"` on the agent call;
  default effort otherwise.
- If an agent returns "blocked on a decision", that is a spec bug: Fable
  decides, amends the spec, respawns. Never tell an agent "use your judgment".
