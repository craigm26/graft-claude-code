---
description: Take the latest move Graft delivered for a copy into this clone, as graft/<release>
argument-hint: "[repo or copy id]"
---

Graft delivers a moved copy itself: it keeps the moved branch, graft/<release>, for the user to take. Fetch it into the clone in the current directory. $ARGUMENTS names the repository or copy id; if empty, use this clone's `origin` remote (owner/name).

1. Call `list_copies` and find the copy (by id, or by repository matching `git remote get-url origin`). If the tools are missing or answer "unauthorized", stop and say: set `GRAFT_TOKEN` to a token from https://graftfork.com/app#settings and restart Claude Code.
2. Call `take_move` with the copy's id. If it says there's no move to take, say so with the copy's last sync in plain words (`status`), and stop.
3. Make sure this is a clone of that copy and its branch is up to date: `git fetch origin <branch>`. Don't touch uncommitted work.
4. Run the commands `take_move` returned, in order: download the bundle with its link (it works for an hour), then `git fetch` it as `graft/<release>`. Don't run the last command (switching branches) unless the user asks.
5. Tell the user the branch is there, which release it's on, and how to take it: `git switch graft/<release>`, or `git checkout <branch> && git merge --ff-only graft/<release>`.

Never push anything, and never change the user's own branch unless they ask.
