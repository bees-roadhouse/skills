---
name: to-tickets
description: Break a plan, spec, or the current conversation into jobs, whole pieces of product one person finishes, each with a done sentence, published as issues.
disable-model-invocation: true
---

# To Tickets

Break a plan, spec, or conversation into **jobs**: whole pieces of a product, each handed to one person to finish, each with **done** written as what the user can do when it is finished.

Issues live in the repo's tracker (GitHub Issues or GitLab Issues, per the house rules); labels, branch names, commit subjects and MR shape follow the house rules in your profile's CLAUDE.md and the repo's CLAUDE.md.

## Why jobs and not slices

Bee's Roadhouse, 2026-10-30, after a month of tracer-bullet slices: "stop the slice thing. give person a job to finish.. this shit isn't released yet". Nothing is released, so there is no production to protect with strangler routes, parity-tested migrations or expand-contract sequences. Thin slices multiplied merge requests, reviews and builds, and a day of ten merges shipped nothing anyone could open. A job is sized so that finishing it puts something in front of the user.

## Process

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes a reference (a spec path, an issue number or URL) as an argument, fetch it and read its full body and comments.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code. Job titles and descriptions use the project's domain glossary vocabulary, and respect ADRs in the area you're touching.

### 3. Draft the jobs

Break the work into **jobs**.

<job-rules>

- A job is a whole piece of the product: a screen that works end to end on the real instance, a backend in the new language with the old one deleted, an app on the device. Not a layer, not a step, not a tracer bullet.
- **Done is a sentence about the user:** "Nate signs in at hive.gumbees.ai, makes a document, edits it, and sees both versions." If done cannot be written that way, the job is a layer or a step; fold it into the job it serves.
- One person finishes one job before taking the next. A job that needs two people is two jobs with a named seam between them.
- The old path goes in the same job. No strangler route beside the old one, no parity test suite kept alive for a migration, no expand-contract sequence: replace, delete, and let restart-from-main be the safety net.
- A job is one merge request, opened with auto-merge set, reviewed once on the whole. A job that outgrows a day gets a second merge request, never a tenth.
- Estimate in hours of the person's time, and name the real limits (review throughput, CI capacity, a decision only the owner can make).

</job-rules>

Give each job its **blockers**: the other jobs that must finish before it can start. Most jobs have none; a long blocker chain is a sign the work was cut into steps again.

### 4. Quiz the user

Present the proposed jobs as a numbered list. For each job, show:

- **Title**: short descriptive name
- **Done**: the sentence about the user
- **Owner**: who finishes it
- **Blocked by**: which other jobs (if any) must finish first
- **Hours**: the estimate, with the real limits named

Ask the user:

- Is each job whole? Could it be finished and used on its own?
- Is anything a step dressed as a job? Fold it in.
- Are the owners right, one person per job?

Iterate until the user approves.

### 5. Publish the jobs to the tracker

Publish the approved jobs as issues in the repo, one issue per job, blockers first so each job's blockers can reference real issue numbers. Set the tracker's native "blocked by" dependency where it supports it; otherwise list the blockers in the issue's "Blocked by" section. If the source was an existing issue, make each job its sub-issue. Label each job per the house rules (what kind of change it is, and `ready-for-agent` where the rules use state labels). The tracker is the only record: jobs never go to local files.

Work the **frontier**: any job whose blockers are all done.

Do NOT close or modify any parent issue.

<issue-template>

## Parent

A reference to the parent issue on the tracker (if the source was an existing issue, otherwise omit this section).

## Done

One sentence: what the user can do on the real instance when this job is finished, with the old path gone.

## What to build

The whole piece, from the user's perspective, not layer-by-layer implementation.

## Acceptance criteria

- [ ] Things the user does, one per line
- [ ] The old path is deleted

## Owner and estimate

Who finishes it, in hours, with the real limits named.

## Blocked by

- A reference to each blocking job, or "None (can start immediately)". Omit this section when blockers were set as native edges.

</issue-template>

Avoid specific file paths or code snippets: they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it and note briefly that it came from a prototype. Trim to the decision-rich parts, not a working demo, just the important bits.
