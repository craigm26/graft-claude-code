---
description: Send one of this copy's changes to the upstream project, as a pull request from your own account or a patch
argument-hint: "<the change: its subject, or its number among the latest move's carried changes>"
---

A change upstream accepts is one nobody has to carry again. Prepare one of the user's changes for the project it came from, for the copy whose clone is in the current directory. $ARGUMENTS names the change.

1. Call `list_copies` and find the copy whose repository matches `git remote get-url origin`. If the tools are missing or answer "unauthorized", stop and say how to connect. If the graft tools are missing, run `/mcp`, choose graft (`plugin:graft:graft`), and sign in to allow it. If a tool answers insufficient_scope, the connection lacks that scope: disconnect graft in `/mcp` and connect again, ticking the scope named in the answer. Without a browser, make a token at https://graftfork.com/app#settings, set `GRAFT_TOKEN` to it, add the server with `claude mcp add --transport http graft https://graftfork.com/mcp --header "Authorization: Bearer $GRAFT_TOKEN"`, and restart Claude Code. If the clone isn't one of the user's copies, say so and stop.
2. Call `send_upstream` with the copy's id and the change. It answers with the change, its reason, and `pullRequest` (the title and text, taken from the change's recorded reason). If no change matches, its answer lists the latest move's carried changes: show that list and ask which one. If it says sending changes upstream is off for this copy, say so (it's turned on in the copy's settings on graftfork.com) and stop.
3. Call `build_upstream` with the copy's id and the change. Graft builds `graft/upstream/<change>` with only that change on the copy's release, runs the copy's tests on it, and keeps the branch as a download and a patch file. In a terminal, `graft upstream '<change>'` (npm `graftfork` 0.2.0 or later) does the same and waits for it.
4. Call `upstream_status` with the build's id until the build finishes (it takes a few minutes; `graft upstream-status <build>` in a terminal). If it says the change doesn't apply alone, explain that it builds on another change, and offer to send that one first.
5. Show the user the pull request's title and text and the tests' verdict, and run the commands `upstream_status` gave to fetch the branch into this clone. The user pushes it to their own fork and opens the pull request from their own account; run those steps (`git push`, `gh pr create`) only if the user asks. For projects that take patches by email, use the patch file from `upstream_status`, or `graft upstream '<change>' --patch <dir>`.
6. Once the pull request is open, call `track_upstream_pr` with the build's id and its URL (`graft upstream-pr <build> <url>` in a terminal). Graft checks it daily and drops the change on the next sync once it's merged.

Never open a pull request under any account but the user's own, and never push to the user's own branch.
