---
name: sd-pickup
description: Start a session in SpaceDonkey. Use at the beginning of any session here, before touching anything. Covers reading the handoff, running its validation block and reporting first, and queueing stale memory for the wrap.
---

# Picking up a session

The rule that makes this worth having: **report before doing any work.** A session that
starts editing and mentions a failing check three tool calls later has already spent the
budget that the check was supposed to protect.

## 1. Read the handoff

`.scratch/NEXT-SESSION.md` is the handoff. It is gitignored, so it is the one place that
may hold environment-specific detail — absolute paths, which commands are on `PATH`,
which addresses to commit as. Nothing in this directory may assert those things.

Read the whole file before running anything. In particular read its "threads" section:
items are frequently left open **on purpose**, with the reasoning recorded. Re-deciding
one from first principles wastes the session that decided it.

### When there is no handoff

Because it is gitignored, it will often be absent: a fresh clone, a different machine, a
cleared scratch directory, or anyone who has never run a session here. **That is a
normal starting state, not an error, and not a signal that something was lost.**

**Absence must not invite you to invent prior state.** Do not reconstruct a plausible
handoff from git history, open pull requests or memory and then proceed as though it
were handed to you. A guessed pickup reads exactly like a real one and is worse than
none.

Instead:

1. **Say plainly that no handoff exists** and that you are starting from the committed
   repository alone.
2. **Run the committed checks** — the suite in `AGENTS.md` under "Commands", at the
   pinned versions in `sd-pr`. Those live in the repository, so they work with no
   handoff.
3. **Confirm the task with the human before starting it.** With no handoff there is no
   pickup prompt, so there is nothing to infer intent from. Ask.
4. **Do not write a handoff at pickup.** The first one is written at the wrap,
   describing what this session actually did. A handoff authored before the work is a
   prediction wearing the costume of a record.

## 2. Run the validation block, and report

The handoff carries a validation block. Run it, then report what passed and what failed,
as a table, before the first edit.

Two habits that have earned their place:

- **Report a number, not a verdict.** "Was 52, still 52" is a finding. "Fine" is not.
- **A command not found is neither a pass nor a fail.** Say which checks you could not
  run. Environments differ; the handoff records what this one needs.

Then run the repository suite from `AGENTS.md`. `sd-pr` has the pinned-version forms.

## 3. Extend the audit rather than trusting it

The block is a floor, not a ceiling. It greps the surfaces that were known to matter
when it was written, and this repository publishes to more of them than it used to —
branches, pull request bodies, review comments, issues, project board cards, the wiki.

If a surface is public and was not in the block, check it anyway and add it to the block
you emit at the wrap.

## 4. Do not enumerate other sessions or agents

Session and agent names are derived from their working directories, so listing them
pulls the names of unrelated repositories into this one's context without any file being
read. No grep can see that happen, and nothing that surfaces that way may be written
into the repository, the board, or a handoff.

Do not list them from a session in this project.

## 5. Queue the stale memory now, do not fix it now

Scan the project memory and the handoff for entries this session is about to **complete
or contradict** — finished work still described as pending, pointers to the previous
session's task, superseded scratch documents.

Write them down for the wrap. Do not clean them up at pickup: you do not yet know which
ones this session will settle, and a memory corrected at pickup and again at the wrap
has been edited twice to land in the same place.

## 6. Confirm the task before starting it

The handoff's pickup prompt has gone stale more than once — it named the **last**
session's task rather than the next one. If the prompt and the threads disagree about
what this session is for, say so and ask. That question costs one exchange; guessing
wrong costs the session.
