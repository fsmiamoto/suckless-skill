# suckless-skill

A skill that reviews code using the [suckless](https://suckless.org/philosophy/) philosophy. 

Call out bloat, gratuitous abstractions, and speculative configurability.

## Install

The repo is structured both as a Claude Code plugin and as a drop-in standalone skill.

Install via the plugin marketplace:

```text
/plugin marketplace add fsmiamoto/suckless-skill
/plugin install suckless@suckless-skill
```

## What it does?

Output a ranked table of findings, each tagged with one of: `BLOAT`, `INLINE`, `COLLAPSE`, `TRIM`, `KEEP`. 

Example:

```text
/suckless src/api.ts
```

| Verdict | Locator | Symbol | Reason |
| --- | --- | --- | --- |
| `BLOAT` | `src/api.ts:12-25` | `RetryPolicy` | Interface/factory pair has one implementation and one caller; delete both and pass the delay directly. |
| `INLINE` | `src/api.ts:4-8` | `buildHeaders()` | Single-caller wrapper around an obvious object literal; fold it into `fetchUser()`. |
| `TRIM` | `src/api.ts:40-58` | `normalizeUser()` | Keep the API boundary, but collapse the repeated null checks after normalization. |
| `KEEP` | `src/api.ts:31-36` | `parseUserResponse()` | External API data is untrusted; one boundary parser earns its place. |

## What "suckless" means here

- The burden of proof sits with anything that wants to stay, not with the deletion.
- The reader of the resulting code is competent. Code that exists to babysit the reader is itself bloat.
- Keep edge handling: validate and normalize user input or third-party API data once at the boundary; don't spread defensive checks through trusted internal code.

## License

MIT — see [LICENSE](./LICENSE).
