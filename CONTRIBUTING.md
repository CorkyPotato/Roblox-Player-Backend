# Contributing

This repository is for the Player backend's design, status, and tests. The
shipped Studio executor is in
[SaintNova-Roblox-Executor](https://github.com/CorkyPotato/SaintNova-Roblox-Executor).

## Good contributions

- Design proposals for the transport contract.
- Test plans and compatibility matrices.
- Documentation fixes.
- A `PlayerBackend` implementation that respects the interface and the
  availability rule.

## Not accepted

- Kernel drivers, offsets, dumps, or client-integrity bypass code.
- Code that claims a working Player backend without a reproducible test.
- Changes to the Studio executor's release from this repository.

## Proposing work

Open an issue with:

1. The problem.
2. The proposed interface or behavior.
3. How it can be tested without shipping sensitive material.
4. What happens when the transport is unavailable.

Keep the `IExecutionBackend` contract intact. If the contract needs to change,
propose it in the main repository first.
