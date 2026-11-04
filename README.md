# graftfork for Claude Code

[graftfork](https://graftfork.com) (Graft) rebuilds a program blind from its spec (a [duramen](https://github.com/craigm26/duramen) record) and keeps the build only if every case passes; rebuilds are on Premium, Team and Enterprise, and fit programs that answer one request at a time. It also keeps your changed copy of an open-source project up to date: on each upstream release it moves your changes onto it, runs your tests, and keeps the moved branch only if every change lands and the tests pass. Otherwise your copy stays where it was and Graft says why. Graft keeps the moved branch for you to take (`/graftfork:graft-take`, or `graft pull`); a GitHub pull request is an opt-in per copy.

This is plugin version 1.6.0. It connects Claude Code to Graft and teaches it to record why each change was made.

- **MCP server** `https://graftfork.com/mcp` (55 tools, and regen_order_draft while the hosted drafter is open): track a repository, list copies, check status, sync now, take a move, explain why a sync stayed, resolve the conflicts a sync stopped on (on your own Claude account) and answer its questions, and check usage; send a change upstream, with hosted upstream builds and pull request tracking; check a change against the project's tests; tested dependency upgrades; rebuilds from a [duramen](https://github.com/craigm26/duramen) spec; and fork watch alerts. Graft runs your tests before anything moves. The full list is at [graftfork.com/docs/agents](https://graftfork.com/docs/agents#tools).
- **Skill `graft-reasons`**: whenever Claude changes a dependency, a fork or a vendored package, it writes the reason into the commit message in the form Graft reads, and offers to track the repository with Graft.
- **Command `/graftfork:graft-status`**: how your copies are doing and what to do next.
- **Command `/graftfork:graft-take`**: fetches the latest move Graft delivered for this clone's copy, as `graft/<release>`.
- **Command `/graftfork:graft-upstream`**: has Graft build one of your changes alone on your release and run your tests on it, then gives you the pull request's title and text, taken from the change's reason. You push and open the pull request from your own account (Claude does it with your own `gh` login only if you ask), and Graft tracks it until it's merged. Graft never pushes or opens one under its own name.

## Install

In Claude Code:

```
/plugin marketplace add craigm26/graft-claude-code
/plugin install graftfork@graftfork
```

Had the plugin under its old name, `graft`? See [Upgrading from 1.5](#upgrading-from-15): Claude Code still calls your marketplace `graft-claude-code`, so install `graftfork@graft-claude-code` instead.

Restart Claude Code, run `/mcp`, choose `plugin:graftfork:graftfork`, and sign in: Claude Code opens your browser to allow Graft on your account (sign up free at [graftfork.com](https://graftfork.com/#invite)). Then try: "Track this fork with Graft", or `/graftfork:graft-status`.

On the consent page you choose the scopes (`read`, `sync`, `resolve`) and a monthly limit for agent work. Every use is listed in your dashboard. If a tool answers `insufficient_scope`, disconnect graftfork in `/mcp` and connect again, ticking the scope named in the answer. See [Agents and Claude Code](https://graftfork.com/docs/agents) and the [HTTP API](https://graftfork.com/docs/api).

## Upgrading from 1.5

1.6.0 renames the plugin and its marketplace from `graft` and `graft-claude-code` to `graftfork`. The commands are now `/graftfork:graft-status`, `/graftfork:graft-take` and `/graftfork:graft-upstream`, and the server in `/mcp` is `plugin:graftfork:graftfork`.

Claude Code keeps the name a marketplace had when you first added it. If you added this one before 1.6, it is still `graft-claude-code` on your machine, even after `/plugin marketplace update` or `/plugin marketplace add` again, so the install line above (`graftfork@graftfork`) answers that there is no such marketplace.

The marketplace still lists the old plugin, `graft`, unchanged at 1.5.0, so your install keeps loading after the marketplace refreshes. `/plugin update graft@graft-claude-code` keeps you on 1.5.0; it never reaches 1.6. To switch, either install the new plugin from the marketplace you have:

```
/plugin install graftfork@graft-claude-code
/plugin uninstall graft@graft-claude-code
```

or start clean, so the marketplace is named `graftfork` like a new install:

```
/plugin uninstall graft@graft-claude-code
/plugin marketplace remove graft-claude-code
/plugin marketplace add craigm26/graft-claude-code
/plugin install graftfork@graftfork
```

Either way, restart Claude Code, run `/mcp`, choose `plugin:graftfork:graftfork` and sign in again: the server has a new name, so the old sign-in doesn't carry over. Permission rules that name the old tools (`mcp__plugin_graft_graft__...`) need the new names (`mcp__plugin_graftfork_graftfork__...`).

## Upgrading from 1.4

Plugin 1.4 sent `GRAFT_TOKEN` as a bearer header; since 1.5.0 the plugin signs in with OAuth. Updating 1.4 in place gives you 1.5.0 under the old name. Switch to `graftfork` as in "Upgrading from 1.5", then run `/mcp`, choose `plugin:graftfork:graftfork` and sign in. On CI or a machine with no browser, keep your token and add the server with it instead (below).

## Without the plugin

```
claude mcp add --transport http graftfork https://graftfork.com/mcp
```

Claude Code opens your browser to allow Graft. For CI or a machine with no browser, pass a personal token instead: `--header "Authorization: Bearer $GRAFT_TOKEN"` (make one at https://graftfork.com/app#settings). Tokens are shown once, stored only as a hash, and two-factor sign-in on the website doesn't affect them.

Any other MCP client: Streamable HTTP at `https://graftfork.com/mcp`. Clients that support MCP authorization connect with OAuth; others send an `Authorization: Bearer <token>` header.

## What's in here

Configuration and instructions only: `.claude-plugin/marketplace.json`, and in `plugins/graftfork/` the plugin manifest, `.mcp.json`, the skill (`skills/graft-reasons/`) and the three commands (`commands/graft-status.md`, `graft-take.md`, `graft-upstream.md`). `plugins/graft/` is the plugin under its old name, unchanged at 1.5.0, kept so a 1.5 install keeps loading until its user switches. Graft's engine and service are not part of this repository.

## License

Apache-2.0
