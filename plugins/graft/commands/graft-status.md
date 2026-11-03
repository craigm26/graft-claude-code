---
description: How your Graft copies are doing - last sync per copy, what's waiting, and what to do next
argument-hint: "[repo or copy id]"
---

Use the Graft MCP tools to report on the user's copies. $ARGUMENTS names one repository or copy id to focus on; if empty, cover every copy.

1. Call `list_copies`. If the tools are missing or answer "unauthorized", stop and say how to connect. If the graft tools are missing, run `/mcp`, choose graft (`plugin:graft:graft`), and sign in to allow it. If a tool answers insufficient_scope, the connection lacks that scope: disconnect graft in `/mcp` and connect again, ticking the scope named in the answer. Without a browser, make a token at https://graftfork.com/app#settings, set `GRAFT_TOKEN` to it, add the server with `claude mcp add --transport http graft https://graftfork.com/mcp --header "Authorization: Bearer $GRAFT_TOKEN"`, and restart Claude Code.
2. For each copy (or the one asked for), give one line: the repository and branch, the upstream release it sits on, and the last sync in plain words (moved, with how to take it: `/graft:graft-take` or its pull request link if the copy uses one; stayed and why; waiting; or running).
3. For each copy whose last sync stayed or is waiting, call `explain_stop` and give its `next` action in a sentence.
4. Call `list_pending_conflicts`. If conflicting files wait, offer to resolve them now (get_conflict, then submit_resolution with the whole file). If questions wait, list each with its options and ask the user to pick; never answer for them.
5. End with `usage`: syncs used this month against the limit. That limit is the user's own plan; when `coveredBy` lists organizations, also say that copies in those organizations work as Premium, up to `syncs.coveredSyncs` syncs this month, so a count above the own-plan limit isn't a problem.

Keep it short: a compact list, then the one or two things worth doing. Don't start a sync unless the user asks.
