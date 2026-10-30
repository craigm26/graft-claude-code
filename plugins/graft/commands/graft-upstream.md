---
description: Send one of this copy's changes to the upstream project, as a pull request from the user's own account
argument-hint: "<the change: its subject, or its number among the latest move's carried changes>"
---

A change upstream accepts is one nobody has to carry again. Prepare one of the user's changes for the project it came from, in the clone in the current directory. $ARGUMENTS names the change. Everything here is plain git and `gh`; Graft's own engine doesn't run on the user's machine.

1. Find the copy on graftfork.com (`list_copies`, matching `git remote get-url origin`) and call `send_upstream` with its id and $ARGUMENTS. It answers with the change as the latest move carried it, the release, and `pullRequest` (title and text, from the change's recorded reason), or says sending is off or nothing matches: then stop and say so, showing the list it gives.
2. Make sure the moved branch is in the clone: `take_move` (or `graft pull` from the `graftfork` npm package) fetches `graft/<release>`.
3. Build the branch with only that change: fetch upstream's release tag (`git fetch <upstream remote> tag <release>`, adding the upstream remote if it's missing), then `git switch -c graft/upstream/<short-name> <release>` and `git cherry-pick <commit>`, where `<commit>` is the commit on `graft/<release>` whose subject is the change's subject. If it doesn't apply alone, explain that it builds on another change, and offer to send that one first.
4. Run the project's tests on it if you know the command.
5. Show the user the title and text. Push and open the pull request only if the user asks: push the branch to their fork and run `gh pr create` against the upstream project with that title and text, under their own login.

Never open a pull request under any account but the user's own, and never push to the user's own branch.
