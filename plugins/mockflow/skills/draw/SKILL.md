---
description: Draw something on a MockFlow board. Use this when the user wants to be certain the result is created in MockFlow rather than as text.
disable-model-invocation: true
---

# Draw on a MockFlow board

The user asked for this explicitly, so there is no judgement call to make: the
result goes on a MockFlow board.

**What to draw:** $ARGUMENTS

If that is empty, ask what they want on the board and stop. Do not guess.

## This is not optional

Create it with the MockFlow tools. Do not answer with a mermaid block, ASCII art,
a markdown table standing in for a diagram, or a file. The user ran this command
precisely to rule those out.

The only thing that stops you is MockFlow being unreachable. If the MockFlow
tools are missing or a call fails because the user is not signed in, say so
plainly and help them connect. Do not fall back to text and present it as what
they asked for.

## Everything else

The board conventions apply as normal: pick the component that fits, put several
components on one board by passing `projectUrl`, and give the user the board
link when it is done.
