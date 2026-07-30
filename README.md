# MockFlow for Claude Code

**Brainstorm visually with Claude Code.** Think an idea through out loud and watch
it take shape on a real canvas: the mindmap, the plan, the architecture, the
screens, all appearing on the MockFlow board open in your browser as you talk.

Claude Code lives in a terminal, so everything it works out comes back as
scrolling text. Anything structural, how a system fits together, where a flow
branches, what the screens should be, how a quarter is shaped, has to be held in
your head or rebuilt by hand somewhere else afterwards. This gives Claude
somewhere to draw, so the thinking is visible while it happens, and what you are
left with is a board your team can open rather than a transcript nobody scrolls
back through.

It works both ways. Claude reads the board too, so you can rearrange things,
delete a branch, add a note, and then ask it to carry on from what is now there.

This plugin is the Claude Code front end for
[**mockflow-bridge**](https://github.com/mockflow/mockflow-bridge), the small
local program that does the actual drawing. The plugin wires the two together,
checks the setup for you, and teaches Claude how to use the board properly. The
bridge is required and the plugin will prompt you to install it if it is missing.

```
Claude Code   ->   this plugin   ->   mockflow-bridge   ->   your board tab
the brain          the wiring         the local daemon       where it draws
```

## Why use it

**Think out loud and see it.** Say "walk me through the ways we could price this"
and get each option laid out rather than five paragraphs. Say "what would the
onboarding look like" and get screens. The conversation stays a conversation, but
the parts with shape to them land somewhere you can actually look at.

**Build boards from things only your machine can see.** Point Claude at a private
repo, an internal spec, a folder of meeting notes. It reads them locally and
draws what it found. The files are never uploaded. Only the finished drawing is
saved to your board. Hosted design tools cannot do this, because the content
never reaches them.

**It draws live, on the board you are looking at.** Not an export, not a file to
import afterwards. Components appear on the open tab while you watch, and they
are saved through your own MockFlow session.

**It costs no MockFlow AI credits.** The thinking happens in Claude Code, on your
existing subscription. MockFlow is the canvas, not the model.

**You do not have to know what to ask for.** Describe what you are trying to work
out and Claude chooses the form: a branching flow, a board of screens, a
timeline, a chart, whatever fits. You never need to name a component type or
learn what MockFlow calls it.

**It edits instead of duplicating.** Ask for a change to something already on the
board and it modifies that component in place, keeping its position and size,
rather than dropping a second copy on top.

**What you end up with is an ordinary MockFlow board.** Share the link and a
teammate opens it in a browser, drags things around and leaves comments. They
need no plugin, no terminal and no idea any of it was generated.

## What the plugin adds to the bridge

The bridge works on its own, but wiring it up by hand means copying a secret
token out of a terminal into a `claude mcp add` command, and Claude then has to
work out the board conventions for itself. The plugin closes both gaps:

| Without the plugin | With it |
| --- | --- |
| `claude mcp add --transport http ... /mcp/<token>`, pasted by hand | one `/plugin install`, no token ever seen |
| Silence when the bridge is missing, stopped or unpaired | told at session start, in plain language, with the fix |
| Claude guesses the board conventions | it knows them, and knows what to say when a draw fails |
| Updates are a manual npm reinstall | `/plugin update` |

## Real world uses

Very little of this is about code. Anything you would otherwise think through in
a document, a whiteboard session or your head works here.

**Thinking through a half formed idea.**
> I am toying with a subscription tier for small teams, help me think it through visually

Branches appear as you talk. Delete the ones that are wrong, keep going on the
rest, and ask it to go deeper where it looks promising.

**The meeting you just came out of.**
> here is the transcript from this morning's planning call, turn it into a board of decisions, owners and open questions

An hour of talking becomes something the people who missed it will actually read.

**Research you have been collecting.**
> read these interview notes and map where customers keep getting stuck

The pattern across twenty conversations, laid out instead of remembered.

**Planning a quarter.**
> lay out the roadmap in goals.md, showing which workstreams depend on which

**Weighing options before committing.**
> compare the three vendors in vendors/ side by side on the criteria we care about

**Anything with geography in it.**
> map our accounts from customers.csv, then draw the three sales territories we are considering

A real geocoded map, with your own regions drawn on top of it. The same works for
store locations, delivery zones, event venues, market entry or a field team's
coverage. Ask it to redraw a boundary and it moves.

**Getting your bearings in an unfamiliar codebase.**
> make a mindmap of how the payments module is structured

Drawn from the real source, not from a two year old wiki page.

**Working from tools you already use.**
> pull my open Jira tickets and lay them out as a kanban

Connected sources are read through your MockFlow account, so there are no extra
credentials and nothing leaves your control.

## Install

```
/plugin marketplace add mockflow/mockflow-claude-plugin
/plugin install mockflow@mockflow
```

The plugin checks what is missing at session start and tells you. If you do not
have the bridge yet:

```bash
npm i -g @mockflow/mockflow-bridge   # once
mockflow-bridge                      # start it
```

Then open your board at app.mockflow.com, click **Connect Local Agent** at the
top of Ask Mida, and enter the pairing code the bridge is showing. That is a one
time step per computer.

Requires Node.js 18 or newer and a MockFlow account.

## Use

Just ask, in your own words. There is no command to remember, and no need to name
a component type unless you want a particular one. Follow ups work the same way:

> add a Blocked column to that kanban

changes the board you already have rather than drawing a second one.

Two commands exist. You will not normally need either.

```
/mockflow:draw a flowchart of the payment flow
```

Same as asking in plain English, but it removes any doubt. Typed this way, the
result goes on the board and cannot come back as text in the terminal. Useful
when you want certainty, or when a plain request produced a diagram in the
terminal instead of on the board.

```
/mockflow:connect
```

Checks the bridge, the daemon and the board pairing, and tells you which of them
needs attention.

## What is in here

```
.claude-plugin/marketplace.json   the marketplace, lists the plugin below
plugins/mockflow/                 the plugin itself
```

The plugin sits in `plugins/` rather than at the repo root so `marketplace.json`
and `plugin.json` never share a `.claude-plugin/` directory.

Inside the plugin:

| Path | Purpose |
| --- | --- |
| `.mcp.json` | connects to the bridge through `bin/mockflow-connect` |
| `bin/mockflow-connect` | resolves the bridge binary and hands over the stdio MCP connection |
| `bin/mockflow-preflight` | session start check, silent when everything works |
| `skills/board-conventions` | how to use the board well, loads itself, never appears as a command |
| `skills/draw` | the optional `/mockflow:draw` guarantee, only runs when typed |
| `skills/connect` | pairing, status and troubleshooting |

Component definitions are deliberately **not** in here. They are fetched live
from MockFlow by the bridge, so new components work without updating this plugin.

## Development

```bash
git clone git@github.com:mockflow/mockflow-claude-plugin.git
cd mockflow-claude-plugin
claude --plugin-dir ./plugins/mockflow
```

`--plugin-dir` needs no marketplace, so this is how to iterate. `/reload-plugins`
picks up edits to the skills; changes to `.mcp.json` or the `bin/` scripts need a
fresh session. Before pushing:

```bash
claude plugin validate ./plugins/mockflow
```

Nothing works without a running bridge, so keep one up while testing. See
[mockflow-bridge](https://github.com/mockflow/mockflow-bridge).

## Licence

MIT. Use of the MockFlow service itself is governed by the MockFlow terms of
service.
