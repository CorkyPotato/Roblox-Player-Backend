# Roblox Player Backend

![Status](https://img.shields.io/badge/status-design%20%2F%20not%20shipped-orange)

Design notes and a build plan for a Roblox Player backend for
[SaintNova Executor](https://github.com/CorkyPotato/SaintNova-Roblox-Executor).

**This repository does not contain a working Player executor.** It contains the
contract, the honest status of the work so far, and the milestones needed to
build one. The shipped Studio executor lives in the main repository.

## Why this exists

SaintNova's core is built around `IExecutionBackend`: the protocol server, CLI,
UI, job lifecycle, and logging do not know what environment a script runs in.
The Studio backend is the reference live-client implementation. A Player backend
is the same contract with a different transport.

Keeping that work in a separate repository does two things:

- The Studio release stays a working, self-contained product.
- Builders get a focused place for the Player transport without wading through
  the Studio executor or its release process.

## Status

| Track | State |
| --- | --- |
| Framework, protocol, UI, CLI, tests | Shipped in the Studio executor |
| Studio transport | Shipped and tested |
| Player transport | Explored; no stable release |
| Player backend in this repository | Not implemented |

See [`STATUS.md`](STATUS.md) for what was completed, what is blocked, and what
was deliberately left out.

## Start here

- [`STATUS.md`](STATUS.md) — completed, blocked, next.
- [`ARCHITECTURE.md`](ARCHITECTURE.md) — the seam and the transport contract.
- [`ROADMAP.md`](ROADMAP.md) — milestones and acceptance criteria.
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — how to propose work.

## Scope

In scope:

- A `PlayerBackend` that implements `IExecutionBackend`.
- A transport that attaches to the Player, runs a script, streams output and
  errors, supports cancellation, and reports availability honestly.
- Tests, packaging, and documentation for that backend.

Out of scope for this repository:

- Kernel drivers, offsets, dumps, and client-integrity bypass code.
- A public release of the private research from the original workspace.
- Changes to the Studio executor's release.

## Contributors

See [`CONTRIBUTORS.md`](CONTRIBUTORS.md).

## License

MIT; see [`LICENSE`](LICENSE).

Not affiliated with Roblox Corporation. Roblox is a trademark of Roblox
Corporation.

Not affiliated with Roblox Corporation. Roblox is a trademark of Roblox
Corporation.
