# suckless-skill

A skill that reviews code using the [suckless](https://suckless.org/philosophy/) philosophy. 

Call out bloat, gratuitous abstractions, and speculative configurability.

## Install

The repo is structured both as a Claude Code plugin and as a drop-in standalone skill.

## What it does?

Output a ranked table of findings, each tagged with one of: `BLOAT`, `INLINE`, `COLLAPSE`, `TRIM`, `KEEP`. 

Example:
TODO

## What "suckless" means here

- The burden of proof sits with anything that wants to stay, not with the deletion.
- The reader of the resulting code is competent. Code that exists to babysit the reader is itself bloat.
- Keep edge handling: validate and normalize user input or third-party API data once at the boundary; don't spread defensive checks through trusted internal code.

## License

MIT — see [LICENSE](./LICENSE).
