# Roadmap

The order matters: a transport that cannot report availability or cancel is not
useful, even if it can run a script once.

## 1. Transport contract

- [ ] Define the transport interface (attach, execute, stream, cancel, detach).
- [ ] Document the failure modes and the availability rules.
- [ ] Add a fake transport for tests.

Acceptance: a unit test can run a script through the fake transport and receive
streamed output, a result, and a cancellation.

## 2. Attach and availability

- [ ] Find the target client and the transport endpoint.
- [ ] Report `available = false` with a specific reason when either is missing.
- [ ] Handle client restart and stale handles.

Acceptance: `saintnova backends` reports `player` as available only when the
transport is actually reachable.

## 3. Execute and stream

- [ ] Deliver a script to the transport.
- [ ] Stream `print`/`warn` and runtime errors.
- [ ] Return the full transcript in `ExecutionResult`.

Acceptance: the example scripts produce the same output shape as the Studio
backend.

## 4. Cancel and timeout

- [ ] Poll the cancel flag at safe points.
- [ ] Enforce `timeout_ms`.
- [ ] Return `ok = false`, error `"cancelled"` on cancel.

Acceptance: a long-running script stops within the timeout and a cancel request
returns promptly.

## 5. Packaging

- [ ] Build the backend as part of the core.
- [ ] Register it without changing the protocol, UI, or CLI.
- [ ] Add a release note and a compatibility table.

Acceptance: the existing Python and Luau suites pass with the new backend
registered.

## 6. Test matrix

- [ ] Client builds: at least two versions.
- [ ] Attach/detach cycles.
- [ ] Repeated runs and reset.
- [ ] Error paths: missing transport, stale handle, client exit.

Acceptance: the matrix is recorded in the repository with pass/fail results.

## Non-goals

- Publishing offsets, dumps, drivers, or bypass code.
- Changing the Studio executor's release process.
- Claiming Player support before the backend is stable.
