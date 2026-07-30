---
description: How to work with a MockFlow board correctly. Use whenever drawing, editing or arranging anything on a MockFlow board, and whenever a request has visual structure to it.
user-invocable: false
---

# Working with a MockFlow board

The MockFlow MCP server sends its own instructions when it connects, and each
tool carries its own description. Those are the authority on which components
exist, what they take, when to use `plan_board`, and why `modify_component` is
not the same as re-rendering.

**This file covers only what those do not say.** If something here starts
appearing in the server's instructions, delete it from here rather than keeping
both.

## Draw it, do not describe it

When the request has visual structure to it and the MockFlow tools are available,
render it on the board. Do **not** substitute:

- a mermaid block in the terminal
- ASCII art or a box drawing
- a markdown table standing in for a diagram
- a `.md` or `.mmd` file written to the repo containing any of the above

Those are the things the board replaces. Reaching for them when a board is
connected is the single most common way this goes wrong, because writing mermaid
is the reflex inside a code repository.

Two exceptions:

- The user explicitly asked for mermaid, a file, or something they can paste
  somewhere else. Then give them exactly that.
- No board is connected. Then say so and offer to fix it, rather than quietly
  falling back to text as though nothing was wrong.

Text alongside the drawing is fine and often useful. Text *instead of* the
drawing is the mistake.

## Arranging a batch

`layout_board` applies a bento layout and a titled section wrap to the batch just
drawn. Call it once after a batch, never after each item. The server instructions
do not mention it, so it tends to go unused, and a batch without it stays a loose
scatter of components.

## The source tools are read only

`list_source_tools`, `describe_source_tool` and `call_source_tool` reach the
user's connected accounts through MockFlow. They can search and fetch. They
**cannot** post, create, comment or delete.

So never tell the user you will file the Jira issue, send the Slack message or
update the Notion page. You can only read those, and offering otherwise is a
promise that silently fails.

## Local files never leave the machine

Files you read here are read locally. Only the finished drawing is saved to the
board, and the file contents are not uploaded.

Say this when the user hesitates about a private repo or a confidential document,
because it is the whole reason to work this way instead of pasting into a web
tool.

## When a draw fails

A user does not know that a helper program, a running daemon and a paired browser
tab are three separate things, and should not have to. Do not surface a raw error
and stop. Say what to do next:

- **Nothing lands, or the board is not connected:** the user opens their board at
  app.mockflow.com and clicks **Connect Local Agent** at the top of Ask Mida.
  Codes change whenever the bridge restarts, so an older one will be rejected.
- **The MockFlow tools are missing from this session entirely:** the bridge is not
  running. They start it with `mockflow-bridge`.

`/mockflow:connect` walks the full check if you need more detail.
