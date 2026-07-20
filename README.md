# claude-code-skill-fable-and-opus

Claude Code skill: **Fable architects, Opus builds.**

Fable (expensive, best judgment) makes every decision — UI, database schema,
API shapes, naming, file layout — and writes terse zero-decision specs. Opus
4.8 agents (cheaper, fast) implement them, max 3 concurrent. Fable reviews the
diffs and verifies. Result: Fable-quality decisions at closer-to-Opus cost.

## Install

```sh
git clone https://github.com/dested/claude-code-skill-fable-and-opus ~/.claude/skills/fable-opus
```

New Claude Code sessions pick it up automatically.

## Use

Opt-in only — it never triggers on its own:

```
/fable-opus build the billing settings page
```

## How it works

1. Fable reads the repo until it can spec without guessing.
2. Fable settles every decision (real DDL, real types, real component names).
3. One spec per parallel task: CONTEXT / TASK / FILES / DETAILS / DO NOT /
   DONE WHEN / RETURN.
4. Opus 4.8 agents implement, ≤3 at a time (say "use 6 agents" to raise it).
   Overlapping-file tasks serialize or get worktree isolation.
5. Fable reviews every diff, fixes small deviations itself, then runs
   typecheck/tests.

## Update

```sh
git -C ~/.claude/skills/fable-opus pull
```
