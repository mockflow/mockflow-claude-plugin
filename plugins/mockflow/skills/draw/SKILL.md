---
description: Gaps in the MockFlow board tool surface that its own instructions do not cover. Use whenever drawing, editing or arranging anything on a MockFlow board.
user-invocable: false
---

# Drawing on a MockFlow board

The MockFlow MCP server sends its own instructions when it connects, and each
tool carries its own description. Those are the authority on which components
exist, what they take, when to use `plan_board`, and why `modify_component` is
not the same as re-rendering.

**This file covers only what those do not say.** If something here starts
appearing in the server's instructions, delete it from here rather than keeping
both.

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
