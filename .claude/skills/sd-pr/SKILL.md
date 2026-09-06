---
name: sd-pr
description: Open a pull request in SpaceDonkey. Use when about to open, describe, or split a PR here. Covers the draft gate, the checkbox contract, pinned-version checks, and how to split a mixed PR.
---

# Opening a pull request

`AGENTS.md` has the policy. This is the working method, plus the things that have
actually gone wrong.

## Keep the body short

**The checkboxes do the mechanical work. The prose does not.** Aim for something that
fits on one screen.

Write for an educated layman. A reviewer here may not be a programmer, and a long body
does not make a change more reviewable — it makes it less likely to be read. The ask
from review has been explicit: keep pull requests legible to a non-specialist, and keep
best-practice ceremony from taking over the repository. Legibility is also what makes
the record good provenance later.

What earns its place in the body:

- one or two sentences on what changed
- anything you deliberately left out, and why
- anything you are unsure about
- claims and non-claims, when the change is technical

What does not: restating the diff, tables of every word you touched, or explaining
reasoning the code already shows. If you find yourself writing a third heading of
justification, the PR is either too big or the body is doing the diff's job.

This has gone wrong twice. Both times the fix was deleting two thirds of the body.

## The two boxes that matter

- **Open as a draft** when targeting `main`. Always. GitHub will not merge a draft, so
  it is a real stop rather than an honour system.
- **Never tick "I have reviewed my own changes."** It is a human's statement about their
  own reading of the diff. An agent ticking it makes the artifact a lie.

Tick the attribution box that is true, and name the tool.

## Merging takes two humans

`main` has a ruleset: pull request required, **one approving review**, the six CI checks
green, branch up to date, conversations resolved, no force pushes or deletions.

GitHub does not let an author approve their own pull request. So the checkbox is the
author's attestation and the required review is the second human.

**Nobody bypasses, including administrators, including you.** You are running with a
maintainer's credential and GitHub cannot tell you apart from them, so a bypass you
could use is a bypass the gate does not cover. If something genuinely cannot wait, that
is a human turning enforcement off in Settings and writing down why — see
`docs/branch-protection.md`. Never suggest it as a way around a red check.

**Never stack a pull request that targets `main`.** Base it on `main`, not on another
open pull request's branch. A stacked PR targets a branch, so the ruleset does not apply
to it, and merging it lets unreviewed content ride into `main` on the approval given to
the PR underneath. Stacking is fine between working branches, where there is no gate to
launder.

`CODEOWNERS` is a notification mechanism here, not a control — it auto-requests a review
from the owner. The ruleset is the control. "Require review from Code Owners" is off on
purpose: with `*` assigned to @PaulTiffany it would turn that notification into a hard
block on every change. Leave it off unless a human asks.

## Run the checks at the pinned versions

Not newer ones. Two pull requests have now shipped check claims based on whatever
version `npx` resolved to, which is a guess wearing a tick mark.

```bash
npx -y prettier@3.3.3 --check "**/*.{md,json,yml,yaml}"
npx -y markdownlint-cli2@0.13.0 "**/*.md" "#node_modules"
npx -y cspell@8.13.1 lint --no-progress "**/*.{md,py,json}"
python3 tools/figures/ringgen.py --all --check
uvx ruff check tools/ && uvx ruff format --check tools/ && uvx mypy tools/
```

`python`, `ruff` and `mypy` are often not on `PATH`; `python3` and `uvx` are the
reliable forms, which is why they are written that way above. Environments differ, so
check rather than assume. A "command not found" is a missing tool, not a failing check —
do not report it as either passing or failing, and say which checks you could not run.

If a check is genuinely not applicable, leave the box unticked and say why. An unticked
box with a reason is honest; a ticked box you did not verify is not.

## Splitting a mixed pull request

When a reviewer asks for concerns to be separated:

1. Branch from `origin/main`, not from the mixed branch — and target `main`, not the
   branch you split from.
2. Apply only the changes for one concern.
3. **Split by hunk, not by file, when a file straddles both.** A file-level split looks
   clean and can be wrong — one file held back wholesale left a flagged spelling error
   in the tree and would have shipped the "spelling fix" PR with a red spell check.
4. Prove there is no drift before opening:

   ```bash
   git diff origin/<original-branch> --stat -- <files you meant to carry>
   ```

   Only the files you intentionally held back should appear. Anything else is a change
   you made by accident, in content someone already approved.

5. Say in the body what you held back and where it went.

## Claim only what you ran

A pull request body is evidence. Three habits have each produced a false claim here, and
a reviewer caught all three.

**Say what actually executed.** A scheduled workflow does not run on a pull request. If
its actions were not exercised, the checks passing proves nothing about them — write
"read, not run" rather than letting six green ticks imply coverage they do not have.

**A green run does not prove a gate still bites.** If a change touches something that is
supposed to fail, make it fail on purpose and watch. Then put it back. Use a violation
that is an omission rather than a false statement — never tick the human review box to
test it, because that is the one lie the box exists to prevent.

**Verify provenance before attributing.** Do not write "as decided on page X" without
opening page X. An idea proposed in conversation is not a decision recorded on a page,
and dressing one as the other invents authority for it. This has happened once already:
three persona names were attributed to a wiki page that has never contained them.

## Look for the drift you just created

Changing a control means every description of that control is now suspect, including the
ones in files you did not open.

When Code Owners review was turned off, the fact was corrected in
`docs/branch-protection.md`, then later in `.github/workflows/pr-policy.yml`, then later
still in `CONTRIBUTING.md`, `.github/CODEOWNERS` and `docs/ownership.md`. Each round was
prompted by a reviewer finding the next stale copy. One grep after the first change
would have found all of them:

```bash
grep -rniE 'code ?owners? review|bypass|required approval|branch protection' \
  --include='*.md' --include='*.yml' --include='CODEOWNERS' .
```

Do that sweep as part of the change, not after someone else notices.

## Settings are not files, and the difference matters

Rulesets, labels, branch protection and project boards are repository settings. An agent
with a maintainer's credential can change them instantly, outside every gate, and no
pull request is involved.

That has happened three times here: the `main` ruleset, removing its bypass, and the
label taxonomy. Each was authorized. Each was also **changed first and described
afterwards**, which is the weaker order.

So, when a change involves settings:

- **Do not write that the pull request establishes them.** It does not. It
  retrospectively documents a mutation made outside the gate. Say that plainly.
- **Record who made the change and how**, in the reviewed record, since the audit log
  will attribute it to the human whose credential was used.
- **Check `docs/ownership.md` first.** Every GitHub surface there has a declared
  function, and using one in a new way — or at all — is a surface-function decision, not
  plumbing. Amend the declaration in the same pull request and say so in the title.

## When bumping a version, bump what it asks for too

Updating `actions/setup-node` while the workflow still requests an end-of-life runtime
two lines below fixes the wrapper and not the problem. After any dependency bump, check
what the thing is configured to use, not only what version of it is pinned.

## House rules worth remembering

- **US English.** `cspell.json` loads `en_US`. British spellings are errors here, not
  vocabulary — fix the prose rather than growing the dictionary.
- **Check the commit identity before the first commit of a session.** This repository
  sets `user.email` locally, and the global git config may point somewhere else
  entirely. `git config --local --get user.email` — if it comes back empty, stop and
  ask. Do not guess an address, and do not write anyone's address into the repository.
- **State claims and non-claims** for anything technical. An estimate is not a result.
- Never bundle a governance, workflow, or repository-settings change with a mechanical
  one. That is what caused the split above.
