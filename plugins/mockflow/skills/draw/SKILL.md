---
description: Draw something on the user's MockFlow board. Use this when the user wants to be certain the result lands on the board rather than in the terminal.
disable-model-invocation: true
---

# Draw on the MockFlow board

The user asked for this explicitly, so there is no judgement call to make: the
result goes on their MockFlow board.

**What to draw:** $ARGUMENTS

If that is empty, ask what they want on the board and stop. Do not guess.

## This is not optional

Render it with the MockFlow tools. Do not answer with a mermaid block, ASCII art,
a markdown table standing in for a diagram, or a file written to the repo. The
user typed this command precisely to rule those out.

The only thing that stops you is a board that is genuinely not reachable. If the
MockFlow tools are missing from this session, or a render fails because no board
is connected, say so plainly and help them fix it. Do not fall back to text and
present it as if it were what they asked for.

## Everything else

The board conventions apply as normal: pick the component that fits, use
`plan_board` when the request needs several different components, `modify_component`
rather than a second render when changing something already there, and
`layout_board` once after a batch.
