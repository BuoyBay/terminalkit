---
title: "terminalkit Getting started"
summary: "Install, import, and explore the public terminalkit API."
category: "Guide"
symbols: ["TerminalSession", "TerminalCell", "TerminalScreen"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Install the terminalkit package into your project’s packages directory. Import the module you need: `TerminalKit`.

Start with the [API contracts](api.md) for usage, constraints, and platform limitations. The [typed API](typed-api.md) lists constructors, each parameter and its default, properties, and return values directly from source.

## Own the session for its whole lifetime

Create `TerminalSession` with an explicitly selected shell executable, working
directory and terminal size. Keep one main-thread owner for `Read`, `Write`,
`Poll`, `Resize` and `Close`; the session is not a shared worker-thread transport.
Arrange a `Finally` cleanup path that calls `Close()` even if polling, rendering
or input handling throws. Do not rely on the destructor for prompt cleanup.

On each UI tick, read available bytes and feed them to `TerminalScreen`, which
preserves UTF-8 decoding state between chunks. Retain input until `Write()`
accepts it: acceptance can be partial, so retry only the unaccepted suffix.
Each write must contain at most 65536 bytes. An empty `Read()` means no bytes were
returned on that call; it is not proof that the shell exited. Use `Poll()` to
observe shell completion and read `ExitCode` only after it returns False and
before closing the session. Shell exit is also not proof that all background
jobs have exited.

## Interrupt a command or close the terminal

Treat keyboard interruption and session shutdown as separate user actions.
Sending Ctrl-C as terminal input is interpreted by the terminal and foreground
program; it does not guarantee termination. Ctrl-D has terminal EOF semantics,
which depend on the active terminal mode and program. Neither action releases
the `TerminalSession` handle.

For a terminal close, replacement shell or application exit, stop scheduling
reads and writes for that session and call `Close()`. Close is idempotent and
terminates the owned terminal processes, including background job-control
groups on POSIX and processes in the Windows job. POSIX processes that create a
new session escape that ownership boundary. Cleanup includes native waits, so
allow for synchronous shutdown work in the UI lifecycle. Hiding a terminal
view can intentionally leave the session alive; ensure application shutdown
still reaches its owner and closes it.

TerminalKit has no separate transport EOF flag or graceful shutdown handshake.
If final output is required, arrange a bounded drain policy before Close; an
empty read alone cannot establish that all descendant output is complete.
After Close, `Poll()` returns False and `Read()` returns empty bytes, while
`Write()` and `ExitCode` reject the closed session. Those closed-handle results
must not be mistaken for a successful child exit.

[Package overview](index.md)
