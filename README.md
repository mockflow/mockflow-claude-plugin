# MockFlow for Claude

**Turn what Claude works out into a board you can edit and share.** Ask for a
flowchart, a mindmap, the screens of an app, a roadmap or an architecture diagram,
and Claude creates it as a real MockFlow board in your account: every shape,
card and label editable, ready to share with your team.

Works in Claude on the web, the Claude desktop app, Cowork and Claude Code. You
sign in once with your MockFlow account; a free account works.

## Why use it

**Think out loud and see it.** Say "walk me through the ways we could price this"
and get the options laid out rather than five paragraphs. Say "what would the
onboarding look like" and get screens.

**You do not have to know what to ask for.** Describe what you are trying to work
out and Claude picks the form: a branching flow, a set of screens, a timeline, a
chart, a map. You never need to name a component.

**It reads your boards too.** Name a board or paste its link and Claude reads
what is on it, then builds from there: fill the gaps in a plan, summarise a
board, review a flow, or add to it instead of starting a new one. Reading does
not count against your plan's limits.

**What you end up with is an ordinary MockFlow board.** Share the link and a
teammate opens it in a browser, moves things around and leaves comments. They need
no plugin and no idea any of it was generated.

**In Claude Code, build from your own code.** Claude reads the repo and draws what
it found: architecture from infrastructure code, entity relationships from
migrations, a flow from a request handler.

## Examples

> make a mindmap of how the payments module is structured

> draw the cloud architecture from our terraform in infra/

> wireframe the notification settings screen for a mobile app

> here is the transcript from this morning's planning call, turn it into a board of decisions, owners and open questions

> map our store locations and draw the three delivery zones we are considering

> plan a product launch: timeline, kanban of tasks and a budget chart

## Install

**Claude on the web, desktop and Cowork:** add MockFlow from the directory, then
click **Connect** and sign in to MockFlow.

**Claude Code:**

```
/plugin marketplace add mockflow/mockflow-claude-plugin
/plugin install mockflow@mockflow
```

Then run `/mcp`, select `mockflow` and choose **Authenticate**. A browser window
opens to sign in to MockFlow and allow access. That is a one time step.

## Use

Just ask, in your own words. Two commands exist; you will not normally need
either.

```
/mockflow:draw a flowchart of the payment flow
```

Same as asking in plain English, but the result always goes on a board and never
comes back as text.

```
/mockflow:connect
```

Checks the connection and sign in, and tells you what needs attention.

## What is in here

```
.claude-plugin/marketplace.json   the marketplace, lists the plugin below
plugins/mockflow/                 the plugin itself
```

| Path | Purpose |
| --- | --- |
| `.mcp.json` | the MockFlow connector, `https://app.mockflow.com/ideaboard/connector/mcp`, signed in with OAuth |
| `skills/board-conventions` | how to use MockFlow well, loads itself |
| `skills/draw` | the optional `/mockflow:draw` command |
| `skills/connect` | sign in and troubleshooting |

Component definitions are not in here. The connector serves them, so new
components work without updating this plugin.

## Development

```bash
git clone git@github.com:mockflow/mockflow-claude-plugin.git
cd mockflow-claude-plugin
claude --plugin-dir ./plugins/mockflow
```

`/reload-plugins` picks up edits to the skills; changes to `.mcp.json` need a
fresh session. Before pushing:

```bash
claude plugin validate .
claude plugin validate ./plugins/mockflow
```

To draw live on a board you have open, and to keep local files on your machine,
use [mockflow-bridge](https://github.com/mockflow/mockflow-bridge) instead.

## Licence

MIT. Use of the MockFlow service itself is governed by the MockFlow terms of
service.
