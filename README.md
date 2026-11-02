# PTY/ConPTY ANSI grid

A real interactive terminal transport: POSIX PTYs on macOS/Linux and ConPTY on
Windows 10 version 1809 or newer. Shell stdin/stdout/stderr share the terminal;
interactive shells receive a controlling terminal, keyboard signals and actual
terminal dimensions. Every process launch uses an explicit executable path and
working directory; no project-supplied helper is executed implicitly.

`TerminalSession(shell, cwd, columns, rows)` starts a session. `Read`, `Write`,
`Poll`, `Resize`, `ExitCode` and idempotent `Close` expose main-thread polling.
Writes may accept only a prefix; retain and retry the remainder. Reads are bounded
and nonblocking. Windows input uses a bounded native worker queue rather than
blocking the UI on a full input pipe. An unsupported Windows host reports a clear
ConPTY requirement instead of falling back to line-oriented pipes.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add terminalkit --git https://github.com/BuoyBay/terminalkit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## API entrypoints

- `TerminalSession(shell, cwd, columns, rows)`
- `Read`
- `Write`
- `Poll`
- `Resize`

## Platform and scope

Uses PTYs on POSIX hosts and ConPTY on Windows. The bounded grid is a terminal model, not a full system terminal emulator.

## Build and API reference

Source is in `modules/`, with native/runtime inputs where required. Use the
[package guide](docs/API.md) for backend-specific build prerequisites, ownership
rules and supported targets. Rebuilding a native backend requires that target's
compiler and SDK; sealing an archive alone does not rebuild the backend.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```

## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
