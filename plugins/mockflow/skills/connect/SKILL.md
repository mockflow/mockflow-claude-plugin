---
description: Check or fix the connection to MockFlow. Use when the MockFlow tools are missing or failing, when boards are not saved to the user's account, or when the user asks how to connect MockFlow.
---

# MockFlow connection

MockFlow is a hosted connector. The user signs in once with their MockFlow
account; boards they create are saved to that account. A free MockFlow account
works.

## The MockFlow tools are missing

- **Claude Code:** run `/mcp`. If `mockflow` shows as needing authentication,
  select it and choose Authenticate. A browser window opens to sign in to
  MockFlow and allow access. If `mockflow` is not listed, the plugin is not
  enabled: run `/plugin` and enable it, then restart the session.
- **Claude on the web, desktop or Cowork:** open Settings, then Connectors, find
  MockFlow and click Connect. Then turn it on for the chat from the tools menu.

## "Sign in to MockFlow to use this connector"

The sign in expired or was removed. Authenticate again as above.

## "Rate limit reached"

The user reached the number of items they can create in a period or in a day.
The message says how long to wait. Plus and Max plans have higher limits.

## The board did not go where the user expected

Without a board link, every render creates a new board. To add to an existing
board, the user pastes its link (`app.mockflow.com/board/...`) and it is passed
as `projectUrl`.

## Still failing

Check that app.mockflow.com opens in the browser. A firewall or proxy that blocks
it also blocks the connector. Otherwise point the user to
https://mockflow.com/docs/troubleshoot-mockflow-mcp
