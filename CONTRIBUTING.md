# Contributing

This document covers the mechanics: how to set up, what CI checks, and how to get a
change merged. Project scope and editorial direction are decided elsewhere; if you are
unsure whether an idea fits, open an issue and ask before building it.

## Setup

Two toolchains, both optional depending on what you are touching.

**Node**, for the documentation checks:

```bash
npx prettier@3.3.3 --check "**/*.{md,json,yml,yaml}"
npx markdownlint-cli2@0.13.0 "**/*.md" "#node_modules"
npx cspell@8.13.1 lint --no-progress "**/*.{md,py,json}"
```

**Python 3.11+**, for the tooling and figures:

```bash
pip install ruff==0.6.9 mypy==1.11.2
ruff check tools/ && ruff format --check tools/ && mypy tools/
python tools/figures/ringgen.py --all --check
```

`ringgen.py` itself needs no dependencies. It is standard library only, by design, so
anyone can regenerate a figure without setting up an environment.

## Opening an issue

Issues are public. Anyone with a GitHub account can open one — no review, no approval,
no standard of authorship. That is deliberate.

**An issue is never required.** Not before editing the wiki, not before opening a pull
request.

Some of us use issues as a personal planning surface: a to-do list kept with the project
rather than beside a desk, and the place where a few ideas out of very many get chosen
to act on. That is a working habit, not a rule. It is also part of why issues cannot be
a shared obligation — a surface that doubles as somebody's private ordering of their own
ideas is not a gate anyone else should have to pass through.

You do not need to be technical, and you do not need to be right. "This looks wrong to
me" is a complete report. Three templates give a shape — **Idea or claim**,
**Question**, **Problem** — and a blank issue is always available if none of them fit.
Delete any heading you do not need.

### Three surfaces, three standards

Separate doors, not a sequence. Nothing has to pass through one to reach another.

| Surface | What it is                                     |
| ------- | ---------------------------------------------- |
| Issues  | The public intake surface                      |
| Wiki    | The directly published human synthesis surface |
| `main`  | The mechanically reviewed executable record    |

The wiki's gate is human authorship and editorial authority — a property of the
contributor, not a process they completed. `main`'s gate is mechanical. Issues have
neither.

Issues being the ungated surface is exactly why they cannot stand in front of the
others: routing a wiki contribution through a channel with no authorship standard would
defeat the standard.

### Labels

Alongside GitHub's defaults, the labels below carry meaning here. People or agents with
appropriate repository permission may apply them. **A label requests work; it does not
grant an agent authority to act, or to write to the wiki.**

| Label            | Means                                         |
| ---------------- | --------------------------------------------- |
| `idea`           | A proposal or claim worth exploring           |
| `needs-citation` | A claim is asserted without a source          |
| `needs-check`    | Wants a skeptical read before it is relied on |
| `needs-model`    | Wants to be computed rather than asserted     |
| `wiki`           | Concerns the wiki surface                     |
| `repo`           | Concerns this repository                      |

The three `needs-` labels exist so a machine can hand work back to a human, or pick it
up, without either having to guess. They are requests, not judgments.

These labels were created by an agent session — Claude Code,
[session record](https://claude.ai/code/session_01AvwZT6wQHRwjp9vDNc7DAJ) — at
@derektiffany's direction, and were already live before this table existed. Labels are
repository settings rather than files, so what follows is the **reviewed declaration of
the intended taxonomy**, not a record of how it came to be. The settings were mutated
outside the gate and described inside it; that is the weaker order, and the same order
the `main` ruleset was configured in. Change this table if you change the settings.

## Before you open a pull request

Run the formatter, then the checks:

```bash
npx prettier@3.3.3 --write "**/*.{md,json,yml,yaml}"
npx markdownlint-cli2@0.13.0 "**/*.md" "#node_modules"
npx cspell@8.13.1 lint --no-progress "**/*.{md,py,json}"
```

Prettier writes; markdownlint and cspell report. If the three disagree with each other,
that is a configuration bug worth an issue rather than a workaround.

## Spelling

The spell checker runs against `project-words.txt`, a plain alphabetical list of domain
vocabulary. When it flags a legitimate term:

- **Add it to `project-words.txt`.** Orbital mechanics, materials, and spaceflight terms
  belong there permanently.
- **Do not add inline `cspell:disable` comments.** They suppress the check for
  everything nearby and they are invisible in review. One shared dictionary is the
  point.
- **Check the spelling first.** The list is not a place to enshrine a typo.
- **This project is US English.** `cspell.json` loads `en_US`, so British forms are
  spelling errors here, not vocabulary — fix the prose rather than adding `licence`,
  `generalises` or `organisation` to the dictionary. This is the easiest way to turn the
  shared list into a silencer without noticing.

## Figures

Figures are generated from parameters, never hand-edited. Read
[`docs/figures/README.md`](docs/figures/README.md) for the reasoning and the workflow.
The short version:

- Edit the parameter file, not the SVG.
- Run `python tools/figures/ringgen.py --all`.
- Commit the parameter file and the regenerated SVG together.

CI regenerates every figure and fails if a committed SVG differs. A hand-edited figure
will be caught, and would have been silently overwritten anyway.

## Python

- `ruff` for linting and formatting, `mypy --strict` for types.
- Type annotations on every function. `--strict` is on and will complain.
- Standard library by default. A new third-party dependency needs a sentence in the pull
  request explaining why the standard library cannot do it.
- Docstrings on anything doing non-obvious geometry or physics. The formula is not
  self-documenting; write down what it computes and in what units.
- Units in the name or the docstring, always: `altitude_km`, `radius_px`. Unit confusion
  is the single most common way this class of code goes wrong.

## Claims and non-claims

Anything technical states both. This is borrowed deliberately from
[AlphaClaw](https://github.com/PaulTiffany/AlphaClaw), where it is load-bearing, and the
pull request template has a section for it.

- An estimate is not a result.
- A schematic is not a design.
- A figure with an exaggerated axis says so on its face.
- A number without a source is a placeholder, and should be labelled as one.

Speculation is welcome and useful. Speculation presented as a finding is not.

## Working with coding agents

Agent instructions live in [`AGENTS.md`](AGENTS.md), the canonical cross-tool file.
[`CLAUDE.md`](CLAUDE.md) is a thin wrapper that imports it, because Claude Code reads
`CLAUDE.md` rather than `AGENTS.md`. Put tool-neutral guidance in `AGENTS.md` so the two
cannot drift.

### Pull requests to `main`

The rule depends on the target branch, and only `main` is gated.

An agent opening a pull request against `main`:

1. Opens it as a **draft**. GitHub will not merge a draft, so this is a mechanical stop
   rather than an honour system.
2. Ticks exactly one **Attribution** box and names the tool used.
3. Leaves the **human review** box alone.

A human then reads the diff, ticks "I have reviewed my own changes," and promotes the
pull request from draft to ready for review. That checkbox is a statement about a
human's own reading; an agent ticking it makes the record false, which is worse than
having no record.

The `pr-policy` workflow enforces the shape of this: attribution present and
unambiguous, and the review box consistent with the draft state.

### Pull requests between working branches

No gate. Agent-opened and agent-merged pull requests are fine, and using a branch as
persistence or context between agent sessions is an intended workflow.

`main` is the reviewed record. Everything else is scratch space.

### What the gate actually is

Be clear-eyed: a checkbox is an attestation, not a control. The real controls are GitHub
refusing to merge drafts, and branch protection requiring a CODEOWNERS review. See
[`docs/branch-protection.md`](docs/branch-protection.md) for the settings, which have to
be configured by hand.

### Attribution in commits

```text
Co-authored-by: Claude <noreply@anthropic.com>
```

Substitute whichever tool did the work. Where a human wrote part of it, the human is the
author and the agent is the co-author.

## Commits

Present tense, lowercase, describing the change:

```text
add tether spacing parameter to ringgen
fix altitude exaggeration in the equatorial figure
document the image-model failure modes
```

No strict convention beyond readability. Small commits over large ones.

## CI

| Workflow                                                                     | Runs on                 | Blocking           |
| ---------------------------------------------------------------------------- | ----------------------- | ------------------ |
| `lint` — prettier, markdownlint, cspell, ruff, mypy, figure check, SVG parse | every pull request      | yes                |
| `pr-policy` — attribution and the human gate                                 | pull requests to `main` | yes                |
| `links` — external link check                                                | weekly schedule         | no, opens an issue |

External link rot is not a contributor's fault, so it never blocks a pull request.

## Review

Expect questions about claims, units, and whether a number has a source. That is the
review being useful, not the reviewer being difficult. Push back when the reviewer is
wrong; the point is to get it right, not to defer.
