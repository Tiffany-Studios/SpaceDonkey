---
name: sd-wrap
description: End a session in SpaceDonkey and write the handoff. Use when wrapping up, handing off, or asked what is left. Covers flagging stale memory, naming what dangles, auditing the public surfaces, and the shape of .scratch/NEXT-SESSION.md.
---

# Wrapping a session

The output is `.scratch/NEXT-SESSION.md`. It is gitignored, so it is the one place that
may hold environment-specific detail: absolute paths, which commands are on `PATH`,
which address to commit as. Keep that detail out of everything committed, including this
directory.

Work through the five steps below, then write the file.

## 1. Flag what has gone stale

Drift lives in documents as much as in memory, and a wrong pointer is worse than a
missing one because it gets followed.

Go looking for:

- memory entries and handoff lines describing work that this session **finished**
- pointers to the previous session's task, especially anything named "next"
- superseded scratch documents and earlier handoffs
- claims that were true when written and are not now — a check that has since been
  fixed, a count that has moved, a file that has been renamed

**Propose deletion or archival. Do not silently rewrite history**, and do not delete
someone's record of a decision because the decision was reversed. The reversal is the
interesting part.

## 2. Name what is dangling

Everything this session started and did not finish, plus everything it created that
somebody now has to deal with: a draft pull request awaiting a human, an open question
raised in review, a branch that can be deleted, a backup file left somewhere.

A half-done thing named in the handoff is a task. A half-done thing not named in the
handoff is a trap.

## 3. Audit the public surfaces

Anything reachable by someone who is not you is in scope. That is more than it was, and
**it grows without warning** — this list gained four entries in a single afternoon, the
day a new repository went from not existing to being publicly listed.

Within a repository: the default branch, every pushed branch, pull request bodies,
review comments, issues, and project board cards. Alongside it: the wiki. Across the
organisation: **every other repository it owns**, their release notes, any package or
marketplace listing published from them, and the organisation's own profile fields —
description, profile README, topics, homepage.

Release notes and listing copy are the ones most often missed, because they are written
once, in a hurry, at the moment of shipping, and never diffed again.

Check the histories as well as the trees. A commit message is as public as a file.

**Do not hardcode a count.** An earlier version of this skill said "the cheapest sixth
of it", and the arithmetic was wrong within a week. Report what you checked and what
each returned: "clean across eight surfaces" is a result; "looks fine" is not.

**If this session published something new, say so in the handoff**, so the next audit
starts from the longer list rather than rediscovering it.

## 4. Check the pickup prompt names the next task

This is the step that has failed most often. The prompt at the top of the handoff gets
copied forward from the previous one and keeps describing the task that is now **done**.

Read the prompt you just wrote as though you had no other context. Does it name what the
next session is for, or what this one was for? If it names a task, does anything in the
threads contradict it?

Say explicitly which decisions are open and **whose** they are. An open question with a
named owner gets answered; an open question addressed to nobody gets rediscovered.

## 5. Name one workflow improvement

Exactly one, and a small one. Something that would have saved this session time, not a
reorganisation of how the project works.

The threshold for building it is the third time it comes up, not the first. Say which
time this is.

## The file

Write `.scratch/NEXT-SESSION.md` with these sections, in this order:

| Section          | Contents                                                                  |
| ---------------- | ------------------------------------------------------------------------- |
| Pickup prompt    | Paste-ready. Names the next task, the constraints, and what not to assume |
| Validation block | A runnable shell block, plus what each check should return                |
| Pull requests    | A row per pull request, its state, and who it is waiting on               |
| Threads          | The live work. Reasoning, not just status                                 |
| Open, lower      | Real but not urgent                                                       |
| Dangling         | From step 2                                                               |
| CLI gotchas      | Commands that cost a round trip, with the form that worked                |
| Improvement      | From step 5                                                               |

Two rules about the validation block, both learned the hard way:

- **Record the expected value beside each check**, so the next session can tell "still
  dirty at 52" from "newly dirty at 52". A check whose expected output is not written
  down cannot be failed.
- **Say when something is known-dirty or known-inert, and why it was left.** A check
  that fails every session and is meant to trains everyone to ignore the block. Mark it,
  and name whose decision it is.

The CLI gotchas row earns its place because the handoff is gitignored, which makes it
the one file allowed to record environment-specific truths. A command that failed twice
and worked on the third attempt costs the same three attempts next session unless the
working form is written down. Record the form that worked, not the ones that did not.

Write the threads for someone with no memory of the conversation. State what was
decided, what was deliberately **not** decided, and the reasoning. A thread that records
only the current status will be re-argued from scratch, which is exactly what this file
exists to prevent.
