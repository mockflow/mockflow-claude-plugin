---
description: Check, pair or troubleshoot the connection between this machine and a MockFlow board. Use when board tools are missing or failing, when nothing appears on the board, or when the user asks which board they are connected to.
allowed-tools: Bash, mcp__mockflow__list_boards, mcp__mockflow__select_board
---

# MockFlow connection

Three things have to be true before anything can be drawn: the bridge helper is
installed, a daemon is running, and a board tab is paired.

Work through them in that order. Each check tells you which one failed.

## 1. Is the helper installed?

```bash
command -v mockflow-bridge
```

Nothing returned means it is not installed. Offer to install it and say what it
is: a small local program, about 2MB, no MockFlow account required.

```bash
npm i -g @mockflow/mockflow-bridge
```

If Node.js itself is missing, that comes first, from nodejs.org, version 18 or
newer.

## 2. Is a daemon running?

```bash
mockflow-bridge status
```

This reports whether the daemon is up and which boards are connected. If it is
not running, start it. Nothing can be drawn without it, and the MockFlow tools in
this session will be unavailable until it is up and the session has reloaded.

## 3. Is a board paired?

The daemon shows a **pairing code**. The user opens their board at
app.mockflow.com, clicks **Connect Local Agent** at the top of Ask Mida, and
enters that code. The button turns green.

Pairing is per computer and survives restarts, so this is a one time step. But
**codes change every time the daemon restarts**, so an old code will be rejected.
If pairing fails, get the current code rather than reusing one from earlier in
the conversation.

## Which board am I drawing on?

`list_boards` shows what is connected. `select_board` pins the target explicitly.
Without an explicit selection, draws go to the tab the user is focused on, else
the only connected tab.

## Nothing appeared on the board

In likelihood order:

1. The board tab was closed or reloaded, so the pairing is gone. Re-pair.
2. The daemon stopped. Check step 2.
3. Draws went to a different connected board. Check with `list_boards`.

## Reading files

File access is off by default. The daemon has to have been started with
`--workspace <path>` for the agent it spawns to read a folder. This does not
affect Claude Code reading files itself, only the Ask Mida side.
