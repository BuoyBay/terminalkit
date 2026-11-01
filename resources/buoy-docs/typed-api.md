---
title: "terminalkit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["TerminalCell", "TerminalScreen", "TerminalSession"]
dependencies: []
symbols: ["TerminalCell", "TerminalScreen", "TerminalSession"]
related: ["api.md", "getting-started.md", "index.md"]
---

# terminalkit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## TerminalSession

`Public Class TerminalSession`

A real PTY (POSIX) or ConPTY (Windows 10 1809+) session. One main-thread owner.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `ExitCode` | `Integer` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(shell As String, cwd As String, columns As Integer = 100, rows As Integer = 30)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `shell` | `String` | `ByVal` | Required |
| `cwd` | `String` | `ByVal` | Required |
| `columns` | `Integer` | `ByVal` | `100` |
| `rows` | `Integer` | `ByVal` | `30` |

Returns: no value.

### Poll

`Public Function Poll() As Boolean`

No parameters.

#### Returns

| Type |
| --- |
| `Boolean` |

### Read

`Public Function Read() As Bytes`

No parameters.

#### Returns

| Type |
| --- |
| `Bytes` |

### Write

`Public Function Write(data As Bytes) As Integer`

Nonblocking partial acceptance. The caller retains and retries the remainder.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `data` | `Bytes` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Integer` |

### Resize

`Public Sub Resize(columns As Integer, rows As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `columns` | `Integer` | `ByVal` | Required |
| `rows` | `Integer` | `ByVal` | Required |

Returns: no value.

### Close

`Public Sub Close()`

No parameters.

Returns: no value.

### Destructor

`Public Sub Destructor()`

No parameters.

Returns: no value.

## TerminalCell

`Public Class TerminalCell`

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Text` | `String` | No | No | `" "` | — |
| `Foreground` | `Integer` | No | No | `7` | — |
| `Background` | `Integer` | No | No | `0` | — |

## TerminalScreen

`Public Class TerminalScreen`

Bounded ANSI/VT screen, UTF-8 decoded across reads; OSC data is ignored, never executed. Supports cursor motion, erase, scroll regions, alternate screen and 16/256/RGB colours.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Columns` | `Integer` | No | No | `100` | — |
| `Rows` | `Integer` | No | No | `30` | — |
| `CursorX` | `Integer` | No | No | `0` | — |
| `CursorY` | `Integer` | No | No | `0` | — |
| `CursorVisible` | `Boolean` | No | No | `True` | — |
| `Cells` | `Array Of TerminalCell` | No | No | `[]` | — |
| `Foreground` | `Integer` | No | No | `7` | — |
| `Background` | `Integer` | No | No | `0` | — |
| `ApplicationCursor` | `Boolean` | No | No | `False` | — |
| `BracketedPaste` | `Boolean` | No | No | `False` | — |
| `HistoryCount` | `Integer` | Yes | No | — | — |
| `ScrollOffset` | `Integer` | Yes | No | — | — |
| `AlternateScreen` | `Boolean` | Yes | No | — | — |

### Constants

| Name | Type | Value / Default | Description |
| --- | --- | --- | --- |
| `HistoryCapacity` | `Integer` | `2000` | — |

### ScrollBy

`Public Sub ScrollBy(lines As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `lines` | `Integer` | `ByVal` | Required |

Returns: no value.

### FollowOutput

`Public Sub FollowOutput()`

No parameters.

Returns: no value.

### ClearHistory

`Public Sub ClearHistory()`

No parameters.

Returns: no value.

### VisibleCell

`Public Function VisibleCell(column As Integer, row As Integer) As TerminalCell`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `column` | `Integer` | `ByVal` | Required |
| `row` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `TerminalCell` |

### Constructor

`Public Sub Constructor(columns As Integer = 100, rows As Integer = 30)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `columns` | `Integer` | `ByVal` | `100` |
| `rows` | `Integer` | `ByVal` | `30` |

Returns: no value.

### Resize

`Public Sub Resize(columns As Integer, rows As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `columns` | `Integer` | `ByVal` | Required |
| `rows` | `Integer` | `ByVal` | Required |

Returns: no value.

### Line

`Public Function Line(row As Integer) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `row` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### TakeReplies

`Public Function TakeReplies() As String`

No parameters.

#### Returns

| Type |
| --- |
| `String` |

### Feed

`Public Sub Feed(data As Bytes)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `data` | `Bytes` | `ByVal` | Required |

Returns: no value.
