# Architecture

## The seam

SaintNova's core talks to one interface:

```cpp
class IExecutionBackend {
public:
    virtual ~IExecutionBackend() = default;

    virtual BackendInfo info() const = 0;
    virtual ExecutionResult execute(const ExecutionRequest& request,
                                    const OutputSink& sink,
                                    std::atomic<bool>* cancel) = 0;
    virtual void cancel() = 0;
    virtual void reset() {}
};
```

- `info()` returns `id`, `name`, `kind`, `available`, and `detail`.
- `execute()` runs one script and streams `OutputLine`s through `sink`.
- `cancel()` is safe from another thread; execution polls the atomic flag.
- `reset()` drops persistent state.

The protocol server, CLI, UI, job lifecycle, and logging are above this seam.
A new backend does not change them.

## Registration

`make_default_core()` in the Studio executor registers the built-in backends:

```cpp
core->register_backend(std::make_unique<HarnessBackend>());
core->register_backend(std::make_unique<RobloxBackend>());
```

A Player backend would add one line:

```cpp
core->register_backend(std::make_unique<PlayerBackend>());
```

The first registered backend is the default. `saintnova backends` lists it;
`saintnova run --backend player` dispatches to it.

## The Studio reference

The Studio backend is the reference live-client transport. It attaches to
`RobloxStudioBeta.exe`, finds the loader's globals and the template Source
buffer, writes the wrapped script, and reads the loader's state, log, and result
back. The loader clones the template and requires it, so the engine compiles the
Source inside the play session.

The public contract and the exact steps are in the main repository:
[`docs/BACKENDS.md`](https://github.com/CorkyPotato/SaintNova-Roblox-Executor/blob/main/docs/BACKENDS.md).

## A Player backend

A Player backend has the same shape but a different transport. It needs:

- **Attach** — find the target client, confirm the transport is present, and
  report `available = false` with a specific reason when it is not.
- **Execute** — deliver the script to the transport, run it, and stream output
  and errors back through the `OutputSink`.
- **Cancel** — signal the transport and return `ok = false`, error `"cancelled"`
  at a safe point.
- **Reset** — drop any per-session state.
- **Timeout** — respect `timeout_ms`; do not rely on the script to cooperate.

The transport itself is the hard part and is intentionally not specified here.
This repository does not ship a transport, a driver, or a bypass. The interface
is the stable part; the transport is the implementer's project.

## Failure rules

- `available = false` with a specific `detail` is always better than a backend
  that pretends and fails later.
- A cancelled run must return the documented error, not a generic failure.
- Output must be streamed as it is produced and repeated in the result.
- One job per client at a time; the server enforces this.
