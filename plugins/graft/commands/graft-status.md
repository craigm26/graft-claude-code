---
description: How your Graft copies are doing - last sync per copy, what's waiting, and what to do next
argument-hint: "[repo or copy id]"
---

Use the Graft MCP tools to report on the user's copies. $ARGUMENTS names one repository or copy id to focus on; if empty, cover every copy.

1. Call `list_copies`. If the tools are missing or answer "unauthorized", stop and say: set `GRAFT_TOKEN` to a token from https://graftfork.com/app#settings and restart Claude Code; `/mcp` should then show `plugin:graft:graft`.
2. For each copy (or the one asked for), give one line: the repository and branch, the upstream release it sits on, and the last sync in plain words (moved with its pull request link, stayed and why, waiting, or running).
3. For each copy whose last sync stayed or is waiting, call `explain_stop` and give its `next` action in a sentence.
4. Call `list_pending_conflicts`. If conflicting files wait, offer to resolve them now (get_conflict, then submit_resolution with the whole file). If questions wait, list each with its options and ask the user to pick; never answer for them.
5. End with `usage`: syncs used this month against the limit.

Keep it short: a compact list, then the one or two things worth doing. Don't start a sync unless the user asks.
