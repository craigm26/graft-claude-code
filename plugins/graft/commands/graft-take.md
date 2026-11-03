---
description: Take the latest move Graft delivered for a copy into this clone, as graft/<release>
argument-hint: "[repo or copy id]"
---

Graft delivers a moved copy itself: it keeps the moved branch, graft/<release>, for the user to take. Fetch it into the clone in the current directory. $ARGUMENTS names the repository or copy id; if empty, use this clone's `origin` remote (owner/name).

1. Call `list_copies` and find the copy (by id, or by repository matching `git remote get-url origin`). If the tools are missing or answer "unauthorized", stop and say how to connect. If the graft tools are missing, run `/mcp`, choose graft (`plugin:graft:graft`), and sign in to allow it. If a tool answers insufficient_scope, the connection lacks that scope: disconnect graft in `/mcp` and connect again, ticking the scope named in the answer. Without a browser, make a token at https://graftfork.com/app#settings, set `GRAFT_TOKEN` to it, add the server with `claude mcp add --transport http graft https://graftfork.com/mcp --header "Authorization: Bearer $GRAFT_TOKEN"`, and restart Claude Code.
2. Call `take_move` with the copy's id. If it says there's no move to take, say so with the copy's last sync in plain words (`status`), and stop.
3. Make sure this is a clone of that copy and its branch is up to date: `git fetch origin <branch>`. Don't touch uncommitted work.
4. Run the commands `take_move` returned, in order: download the bundle with its link (it works for an hour), then the line that fetches it as `graft/<release>`. Neither switches branches. The fetch is never forced: if `graft/<release>` already exists in the clone with commits the delivery doesn't have, or is checked out, git refuses and the line says nothing was changed. Then ask the user whether to rename theirs first (`git branch -m graft/<release> graft/<release>-mine`), and never delete it or force the fetch. With the CLI, `graft pull` refuses the same way, and `graft pull --replace` keeps the old branch as `graft/<release>-kept-<n>` before taking the move.
5. Tell the user the branch is there, which release it's on, and how to take it: `git switch graft/<release>`, or `git checkout <branch> && git merge --ff-only graft/<release>`.

Never push anything, and never change the user's own branch unless they ask.
