# Status

Last updated: 2026-11-16.

## Completed

### Shipped in the Studio executor

- C++20 core with Luau 0.747.
- `IExecutionBackend` seam with two backends: `harness` and `roblox` (Studio).
- Named-pipe protocol with streaming logs, cancel, timeout, and reset.
- CLI (`serve`, `run`, `compile`, `repl`, `backends`, `version`) and tkinter UI.
- Studio transport: engine-compiled ModuleScript execution in a live play
  session, with output and error streaming and yields.
- Test suites: Luau API smoke, wave surface, libraries, Python protocol/CLI,
  and UI import.

### Explored for the Player (private, not shipped)

- A Player-side transport was prototyped against the live client.
- The work established that the framework above the seam is reusable; the
  missing piece is a transport that can execute code in the Player and return
  output reliably.
- The explored user-mode and kernel-level routes did not produce a stable,
  shippable backend.

What the exploration established:

- The protocol, UI, CLI, and job lifecycle are backend-agnostic; a new backend
  needs no changes above the seam.
- The Studio transport proved the engine-compiled ModuleScript pattern for a
  live client.
- The Player does not expose the same writable Source path, so a Player
  transport must deliver code by a different mechanism.
- The client's integrity checks are the gate, not the scripting API.
- The kernel route is not a drop-in continuation; it needs a signed driver or a
  kernel debugger.

The prototypes, addresses, dumps, and research notes are not in this repository.

## Blocked

- The Player's client-integrity checks are the gate. The user-mode and
  kernel-level routes that were tried did not survive them reliably.
- The kernel route was rolled back. A production route would need a properly
  signed driver or a kernel debugger; both are substantial projects, not a
  continuation of the prototype.
- No stable Player transport means no Player backend can be registered as
  `available` today.

## Next

The next useful work is not another bypass attempt. It is the backend contract:

1. Define the transport interface and its failure modes.
2. Implement attach/detach and availability reporting.
3. Execute a script and stream output and errors.
4. Add cancel and timeout.
5. Package it and test it against a matrix of client builds.

See [`ROADMAP.md`](ROADMAP.md) for acceptance criteria.

## What is not here

- No offsets, RVAs, signatures, or memory maps.
- No kernel driver, loader, or injection code.
- No crash dumps or test certificates.
- No step-by-step bypass instructions.

Those stay in the private workspace. This repository is the public planning
surface.
