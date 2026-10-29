# Graft for Claude Code

[Graft](https://graftfork.com) keeps your changed copy of an open-source project up to date: on each upstream release it moves your changes onto it, runs your tests, and delivers the moved branch only if every change lands and the tests pass. Graft keeps it for you to take (`/graft:graft-take`, or `graft pull`); a GitHub pull request is an opt-in per copy.

This plugin connects Claude Code to Graft and teaches it to record why each change was made.

- **MCP server** `https://graftfork.com/mcp`: track a repository, list copies, check status, sync now, explain why a sync stayed, and resolve the conflicts a sync stopped on, on your own Claude account. Graft runs your tests before anything moves.
- **Skill `graft-reasons`**: whenever Claude changes a dependency, a fork or a vendored package, it writes the reason into the commit message in the form Graft reads, and offers to track the repository with Graft.
- **Command `/graft:graft-status`**: how your copies are doing and what to do next.
- **Command `/graft:graft-take`**: fetches the latest move Graft delivered for this clone's copy, as `graft/<release>`.

## Install

In Claude Code:

```
/plugin marketplace add craigm26/graft-claude-code
/plugin install graft@graft-claude-code
```

Make a token in your dashboard under [Settings](https://graftfork.com/app#settings) (Graft is in beta; [request an invite](https://graftfork.com/#invite)) and put it in the environment Claude Code starts from:

```
export GRAFT_TOKEN=graft_...
```

Restart Claude Code. `/mcp` shows `plugin:graft:graft` when it's connected. Then try: "Track this fork with Graft", or `/graft:graft-status`.

Tokens are scoped (`read`, `sync`, `resolve`), shown once, stored only as a hash, and every use is listed in your dashboard. See [Agents and Claude Code](https://graftfork.com/docs/agents) and the [HTTP API](https://graftfork.com/docs/api).

## Without the plugin

```
claude mcp add --transport http graft https://graftfork.com/mcp --header "Authorization: Bearer ${GRAFT_TOKEN}"
```

Any other MCP client: Streamable HTTP at `https://graftfork.com/mcp` with an `Authorization: Bearer <token>` header.

## What's in here

Configuration and instructions only: `.claude-plugin/marketplace.json`, and in `plugins/graft/` the plugin manifest, `.mcp.json`, the skill and the command. Graft's engine and service are not part of this repository.

## License

Apache-2.0
