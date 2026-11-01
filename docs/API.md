# TerminalKit

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

POSIX uses a private supervisor that keeps the terminal session ID reserved even
after the interactive shell exits. Closing the handle terminates all remaining
processes in that terminal session (including foreground/background job-control
process groups), then reaps the supervisor. Shells that deliberately daemonize a
process into a new session escape this ownership boundary. Windows uses a
kill-on-close Job Object. Call `Close` or Studio's `Shutdown` at application exit;
do not rely on garbage collection for prompt cleanup.

`TerminalScreen` is a bounded ANSI/VT screen model. It decodes UTF-8 across chunk
boundaries and supports cursor motion, erase, scroll regions, alternate screen,
application cursor keys, and 16/256/RGB foreground/background colours. OSC payloads
are discarded; output cannot change the clipboard, launch URLs or execute a
command. This is a compact IDE terminal, not a full xterm implementation: bounded 2000-row
scrollback and no mouse reporting, glyph widths use one codepoint per cell, and
advanced terminal protocols/attributes are not implemented.

Studio's `StudioTerminalWindow(projectRoot)` renders the screen with native Canvas
input and a monospaced font. Type directly into the terminal. Ctrl-C interrupts,
Ctrl-D sends EOF, and arrows navigate shell history/line editing. Paste is explicit
(button, Cmd-V on Mac, Ctrl-Shift-V elsewhere); Copy Screen copies only the current
grid. New Shell destroys the previous session and starts afresh. Hiding the window
keeps its shell alive; `Shutdown` terminates the shell and closes the window.

Build/test:

```sh
python3 tools/build_terminalkit.py
python3 tools/build_terminalkit.py --target x86_64-windows --cc x86_64-w64-mingw32-gcc --native-only
python3 tools/build_terminalkit.py --target arm64-darwin --native-only
python3 tools/build_terminalkit.py --target x86_64-darwin --native-only
python3 tools/build_terminalkit.py --seal-only
```

Seal includes only runtime slices whose source receipt matches the current native
bridge. Six-target compiler checks prove syntax and semantic compatibility; they
do not prove native target execution. Linux native PTY and Studio GUI smoke tests pass. The full native PTY suite also
passes on arm64 macOS (m4-casa), including job cleanup and abrupt owner death.
The sealed model and actual Studio GUI smoke pass there too; six consecutive
standalone GUI runs also passed after the shutdown correction.
Intel macOS is built and compiler-checked but has not been executed natively;
Windows x64 is built and the sealed consumer cross-links, with native Windows
execution still unverified. Darwin shutdown closes the PTY master before reaping
the session leader, avoiding a terminal-teardown wait cycle.

Test suites and fixtures are maintainer-local and are not part of this public repository.
