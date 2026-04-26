---
name: suckless
description: Critique code or designs through a suckless lens — call out bloat, gratuitous abstractions, and speculative configurability, then propose deletions. Use when the user invokes /suckless, asks for a "suckless review", "audit for bloat", "what would suckless cut", or other deletion-focused critique.
argument-hint: [files, diff range, or design to audit]
---

# Suckless

Review code following the [suckless](https://suckless.org/philosophy/) philosophy.

Find the bloat and call it by name.

## Scope

Default: review the current branch diff against the repository's detected default branch, plus working-tree changes, including untracked files.

If the user asks you to review something else, use that instead

## Workflow

1. Read the target. Findings must be specific — don't skim.
2. Apply the philosophy; tag each finding with an exact locator: `file:lines` for code.
3. Output a ranked critique. One row per finding: `VERDICT — locator — symbol or design element — one-sentence reason`. Rank by expected deletion and complexity reduction, descending. Verdicts:
   - `BLOAT` — delete entirely.
   - `INLINE` — wrapper without reuse; fold into the call site.
   - `COLLAPSE` — multiple things doing one job; merge them.
   - `TRIM` — load-bearing, but bloated.
   - `KEEP` — earns its place; use sparingly.
4. Stop. The user decides what to apply — they have context you don't.

## The philosophy

- **Configuration with one used value** — a knob nobody turns. Delete it, hardcode the value, add it back when a second value is real.
- **Single-caller helpers and wrappers** — a name without reuse is a name to remember. Inline.
- **Plugin systems, hooks, extension points** built before the second extension exists — scaffolding for features no one asked for.
- **Future-proofing abstractions** — interface with one impl, factory with one product, strategy with one strategy. The future arrived; it has one implementation.
- **Defensive code for impossible inputs** — internal callers and types you control don't need runtime validation.
- **Try/catch that re-raises, logs-and-rethrows, or swallows quietly** — ceremony around a failure that helps no one.
- **Dependencies pulled in for one function** — 50KB import for a one-line helper. Copy the function, drop the dep.
- **Commented-out or scaffold code kept "in case"** — git remembers. You can let go.
- **Accommodating broken upstream at every call site** — fix the source or normalize once at the boundary; don't fan the chaos out.

## Gotchas

- **Name the deletion.** "Feels over-engineered" is a vibe; "delete `X` — only ever called with the default" is a finding. Specific or silent.
- **Boundaries are not bloat.** Keep validation/adapters for user input, external APIs, and untrusted data, but normalize once at the edge instead of spreading defensive checks through trusted internal code.
- **Tests are not bloat.** Lines of test code aren't lines you delete to win.
- **Mind the call-site count.** A library with hundreds of callers earns abstractions a one-shot script doesn't. If the verdict depends on usage, count and say so.
