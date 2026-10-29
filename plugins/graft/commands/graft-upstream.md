---
description: Send one of this copy's changes to the upstream project, as a pull request or a patch
argument-hint: "<the change: its subject, or its number in graft record>"
---

A change upstream accepts is one nobody has to carry again. Prepare one of the user's changes for the project it came from, in the clone in the current directory. $ARGUMENTS names the change.

1. Run `graft record` to list the changes, and pick the one $ARGUMENTS names. If it's unclear, show the list and ask which.
2. If the copy is tracked on graftfork.com (`list_copies`, matching `git remote get-url origin`), call `send_upstream` with the copy's id and the change. It answers with the command for this copy's setting, or says sending is off: then stop and say so.
3. Run the command (`graft upstream '<change>'`, with `--test "<the project's test command>"` if you know it). It builds `graft/upstream/<change>` with only that change on the copy's release and prints the pull request's title and text, taken from the change's recorded reason.
4. If it says the change doesn't apply alone, explain that it builds on another change, and offer to send that one first.
5. Show the user the title and text. Push and open the pull request only if `send_upstream` gave `--open` or the user asks: `graft upstream '<change>' --open` pushes to their fork and opens it with their own `gh` login. For projects that take patches by email, `--patch <dir>` writes the patch instead.

Never open a pull request under any account but the user's own, and never push to the user's own branch.
