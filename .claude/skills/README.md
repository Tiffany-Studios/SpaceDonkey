# Skills

Claude Code skills specific to this repository.

**Name them `sd-*`.** The prefix marks a SpaceDonkey skill as ours, so it is
distinguishable at a glance from the skills the harness ships with — `/sd-pr` rather
than `/pr`. The directory name, the `name:` in the frontmatter and the slash command all
have to match.

| Skill                             | What it covers                                                                                                                   |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| [`sd-pickup`](sd-pickup/SKILL.md) | Starting a session: read the handoff, run its validation block and report before working, queue stale memory for the wrap        |
| [`sd-pr`](sd-pr/SKILL.md)         | Opening a pull request here: the draft gate, the checkbox contract, pinned-version checks, and how to split a mixed pull request |
| [`sd-wrap`](sd-wrap/SKILL.md)     | Ending a session: flag what has gone stale, name what dangles, audit the public surfaces, and write the handoff                  |

These are read by Claude Code only. Anything that should bind every agent belongs in
[`AGENTS.md`](../../AGENTS.md) instead.

**Nothing here may assert a fact about one machine.** Absolute paths, which commands are
on `PATH`, which address to commit as: these differ per checkout, and a skill that
states them will be confidently wrong for the next person. Environment-specific detail
belongs in the gitignored handoff at `.scratch/NEXT-SESSION.md`, not in a committed
file.
