---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Before writing code, make the branch the house rules ask for (its name, its base, and where its worktree lives come from your profile's CLAUDE.md and the repo's CLAUDE.md). Commit only to that branch, never to the default branch.

Call the Skill tool with "tdd" where possible, at seams you state in your reply.

Run typechecking regularly, single test files regularly, and the full test suite once at the end. Follow the repo's and profile's build rules (for example, one heavy build at a time on a shared machine).

Once done, commit with subjects in the house format, push, and open the PR: call the Skill tool with "pr" for the body. Then tell the user the PR is ready for `/code-review`, which reviews it with fresh context against the linked issue.
