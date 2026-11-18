---
description: Use for ANY request to draw, diagram, map out, sketch, visualise, plan, chart, wireframe or brainstorm something - mindmaps, flowcharts, roadmaps, boards, screens, timelines - BEFORE deciding how to produce it. Also for any request for an image, photo, video, audio or 3D model in MockFlow. Also whenever adding to a MockFlow board the user already has.
user-invocable: false
---

# Working with MockFlow boards

The MockFlow server sends its own instructions when it connects, and each tool
carries its own description. Those are the authority on which components exist
and what they take.

**This file covers only what those do not say.** If something here starts
appearing in the server's instructions, delete it from here rather than keeping
both.

## Draw it, do not describe it

When the request has visual structure to it and the MockFlow tools are available,
create it in MockFlow. Do **not** substitute:

- a mermaid block
- ASCII art or a box drawing
- a markdown table standing in for a diagram
- a `.md` or `.mmd` file containing any of the above
- a generated image of a diagram

Those are what the board replaces. Writing mermaid is the reflex inside a code
repository, which makes it the most common way this goes wrong.

Two exceptions:

- The user explicitly asked for mermaid, a file, or something to paste
  elsewhere. Then give them exactly that.
- The MockFlow tools are not available. Then say so and help them connect (see
  below), rather than quietly falling back to text.

Text alongside the board is fine. Text *instead of* the board is the mistake.

## A picture of a diagram is not a diagram

Never answer a request for a mindmap, flowchart, chart, board, wireframe or any
other structured visual with an image. An image cannot be edited, rearranged or
commented on, which is the reason the user wanted it on a board. That holds for
requests phrased as "draw", "sketch" or "visualise" too.

## Pictures

This connector does not generate images, video, audio or 3D models. A design,
wireframe or document can include picture slots described by a prompt, but they
appear as placeholders that MockFlow fills inside the board, so never describe
them as finished photos. When the user wants a standalone picture or other
media, say it is made in MockFlow itself: open the board and ask Ask Mida.

## Always give the link

Every render returns a board link. Show it to the user every time, as a
clickable link, so they can open, edit and share the board.

## Adding to an existing board

When the user gives a MockFlow board link (`app.mockflow.com/board/...`), pass it
as `projectUrl` on every render so the result lands on that board instead of a
new one. Keep passing it for the rest of the conversation unless they say
otherwise.

When they have not given one, mention once that they can paste a board link to
add to an existing board.

## Reading boards

When the request builds on a board that exists (continue it, fill the gaps,
summarise it, review it), call `read_board` first and work from what is actually
there, not from memory of earlier turns: the user may have changed it since.
When they name a board instead of linking it ("my onboarding board"), find it
with `find_boards`, and if several match, ask which one. Reading does not count
against the user's plan limits.

## Several items at once

If the request needs several different components (a launch plan as a kanban, a
timeline and a checklist), create each with its own tool. Pass the board link
from the first result as `projectUrl` on the rest, so they land on one board.

## Building from the user's files

In Claude Code you can read the repo, specs or notes and draw what you found:
architecture from infrastructure code, entity relationships from migrations, a
flow from a request handler. Say what you read. What you pass to a MockFlow tool
is sent to MockFlow to build the board, so do not include secrets, keys or
credentials in it.

## When MockFlow is not connected or a call fails

Do not surface a raw error and stop. Say what to do next:

- **Sign in needed or expired:** in Claude Code, run `/mcp`, pick `mockflow` and
  choose Authenticate. In Claude on the web or desktop, open Settings, then
  Connectors, and click Connect on MockFlow.
- **Rate limit reached:** the message says how long to wait. Plus and Max plans
  have higher limits.

`/mockflow:connect` walks the full check.
