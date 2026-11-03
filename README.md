# Graft for Claude Code

[Graft](https://graftfork.com) keeps your changed copy of an open-source project up to date: on each upstream release it moves your changes onto it, runs your tests, and keeps the moved branch only if every change lands and the tests pass. Otherwise your copy stays where it was and Graft says why. Graft keeps the moved branch for you to take (`/graft:graft-take`, or `graft pull`); a GitHub pull request is an opt-in per copy.

This is plugin version 1.5.0. It connects Claude Code to Graft and teaches it to record why each change was made.

- **MCP server** `https://graftfork.com/mcp` (55 tools, and regen_order_draft while the hosted drafter is open): track a repository, list copies, check status, sync now, take a move, explain why a sync stayed, resolve the conflicts a sync stopped on (on your own Claude account) and answer its questions, and check usage; send a change upstream, with hosted upstream builds and pull request tracking; check a change against the project's tests; tested dependency upgrades; rebuilds from a [duramen](https://github.com/craigm26/duramen) spec; and fork watch alerts. Graft runs your tests before anything moves. The full list is at [graftfork.com/docs/agents](https://graftfork.com/docs/agents#tools).
- **Skill `graft-reasons`**: whenever Claude changes a dependency, a fork or a vendored package, it writes the reason into the commit message in the form Graft reads, and offers to track the repository with Graft.
- **Command `/graft:graft-status`**: how your copies are doing and what to do next.
- **Command `/graft:graft-take`**: fetches the latest move Graft delivered for this clone's copy, as `graft/<release>`.
- **Command `/graft:graft-upstream`**: has Graft build one of your changes alone on your release and run your tests on it, then gives you the pull request's title and text, taken from the change's reason. You push and open the pull request from your own account (Claude does it with your own `gh` login only if you ask), and Graft tracks it until it's merged. Graft never pushes or opens one under its own name.

## Install

In Claude Code:

```
/plugin marketplace add craigm26/graft-claude-code
/plugin install graft@graft-claude-code
```

Restart Claude Code, run `/mcp`, choose `plugin:graft:graft`, and sign in: Claude Code opens your browser to allow Graft on your account (sign up free at [graftfork.com](https://graftfork.com/#invite)). Then try: "Track this fork with Graft", or `/graft:graft-status`.

On the consent page you choose the scopes (`read`, `sync`, `resolve`) and a monthly limit for agent work. Every use is listed in your dashboard. If a tool answers `insufficient_scope`, disconnect graft in `/mcp` and connect again, ticking the scope named in the answer. See [Agents and Claude Code](https://graftfork.com/docs/agents) and the [HTTP API](https://graftfork.com/docs/api).

## Upgrading from 1.4

Plugin 1.4 sent `GRAFT_TOKEN` as a bearer header. 1.5.0 doesn't: it signs in with OAuth. After updating, run `/mcp`, choose `plugin:graft:graft` and sign in. On CI or a machine with no browser, keep your token and add the server with it instead (below).

## Without the plugin

```
claude mcp add --transport http graft https://graftfork.com/mcp
```

Claude Code opens your browser to allow Graft. For CI or a machine with no browser, pass a personal token instead: `--header "Authorization: Bearer $GRAFT_TOKEN"` (make one at https://graftfork.com/app#settings). Tokens are shown once, stored only as a hash, and two-factor sign-in on the website doesn't affect them.

Any other MCP client: Streamable HTTP at `https://graftfork.com/mcp`. Clients that support MCP authorization connect with OAuth; others send an `Authorization: Bearer <token>` header.

## What's in here

Configuration and instructions only: `.claude-plugin/marketplace.json`, and in `plugins/graft/` the plugin manifest, `.mcp.json`, the skill (`skills/graft-reasons/`) and the three commands (`commands/graft-status.md`, `graft-take.md`, `graft-upstream.md`). Graft's engine and service are not part of this repository.

## License

Apache-2.0
