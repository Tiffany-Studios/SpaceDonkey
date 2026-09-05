# Ownership and surface function

**Status: the surface boundary is declared. The mechanics below are proposed.** It is
written down because the alternative — two people holding slightly different models of
who owns what — has already cost us once, when a branch that was live got cleaned up as
stale. That was not a failure of care. It was the absence of a written boundary, which
is the thing this document is trying to be.

## The boundary

Declared by Paul, and recorded here rather than restated elsewhere:

| Surface                   | Owner | Purpose                                                                                                                     |
| ------------------------- | ----- | --------------------------------------------------------------------------------------------------------------------------- |
| `SpaceDonkey.wiki.git`    | Paul  | Fortify and celebrate prior art, distinguish established physics from speculation, teach the history, develop the synthesis |
| `PaulTiffany/SpaceDonkey` | Derek | Executable evidence: code, parameters, generated figures, CI, contributor mechanics                                         |

The boundary binds agents, not only people, and it binds in both directions:

- Agents working with Paul target the wiki repository and do not modify the main
  repository unless explicitly asked.
- Agents working with Derek target the main repository and do not modify the wiki unless
  explicitly asked.

The second line is the reciprocal of the first. A one-sided boundary is not a boundary,
and the branch deletion that prompted this document was a cross-surface write nobody had
agreed was out of bounds.

Note that the wiki's declared purpose — separating established physics from speculation
— is a claim about epistemic hygiene, not about formatting. That is what makes the
citation checks below worth building rather than decorative.

## The rule

Every GitHub surface gets a declared function before it is used.

The point is to decide what a surface is _for_ before its affordances decide for us. A
tool adopted because it was there tends to reshape the process around itself.

| Surface       | Declared function                        | Gate available           |
| ------------- | ---------------------------------------- | ------------------------ |
| Repository    | Executable evidence                      | Pull request, CI, owners |
| Wiki          | Current model of the architecture        | None                     |
| Pull requests | The human gate                           | `pr-policy`              |
| Actions       | Predetermined mechanical checks only     | —                        |
| Issues        | Public intake: questions, reports, ideas | —                        |
| Releases      | Frozen research checkpoints              | —                        |

| Projects | Private planning and prioritization | — |

Discussions have no declared function and are therefore not in use. That is the rule
working, not an oversight.

**Issues were narrowed to "falsifiable open questions and kill tests" when this table
was written, before the surface had been used.** Held to literally, that excludes a
broken build, a figure that misrepresents something, and a newcomer saying "this looks
wrong to me" — all of which belong somewhere, and none of which belong in the wiki or in
a pull request. The declared function is therefore widened to public intake. Kill tests
remain the most valuable thing an issue can carry; they are no longer the only thing it
may.

**Projects was declared out of use, and then used.** A board now exists and holds
sixteen cards. Declaring it after the fact is the weaker order and is noted as such. Its
function is private planning and prioritization: the place where many possible ideas get
reduced to the few worth acting on. It is deliberately not a gate — nothing has to
appear on it, and it stands in front of nothing.

## Single source of truth

Any one piece of content has exactly one place it is edited. Everything else links to it
or is generated from it. Nothing is maintained in two places, ever.

The split is **by content type, not by repository**. The test:

> Can CI check it?

Yes → repository. Parameter files, generated figures, code, CI configuration,
contributor mechanics. Gated, blocking, reviewed before it lands.

No → wiki. Architecture, reasoning, prior art, kill criteria, the current model.
Ungated, advisory, published on save.

These are different content, so there is no drift risk between them. The question "repo
or wiki?" only felt hard while we were treating it as one decision about one body of
material.

## Why both a gate and no gate is coherent

The instinct that bad physics should be blocked before it goes live, and the instinct
that a human should be able to write whatever they want on a wiki page, are not in
conflict. They apply to different layers.

This mirrors a structure described in _WikiSkill: Compiling Agent Experience into
Persistent Knowledge for Skill Evolution_ (Tang et al., 2026), which separates immutable
raw traces, a persistent knowledge layer, and executable skills — and gates changes to
the executable layer **while preserving the knowledge layer regardless of whether a
proposed change is accepted**. That is also how scientific literature behaves: a refuted
hypothesis stays in the record, because knowing what failed is knowledge.

Borrowed as structure, not cited as authority. That paper is about agent skill
evolution, not repository governance, and the correspondence is posited here rather than
demonstrated.

## The ratchet

An advisory that nobody clears becomes wallpaper. The proposed shape that keeps advisory
checks honest without gating human writing:

- Advisory on absolute state. Nothing is blocked at the door.
- Blocking on regression. The count of uncited claims may not increase.

Humans write freely; the trend can only improve. Whether this is worth the machinery is
open.

## What is checkable, honestly

Two ideas that sound like one and should not be promised together:

- **Citation hygiene.** Every page carries sources; every numeric claim is cited or
  explicitly marked as uncited. Genuinely lintable and cheap.
- **Physical plausibility.** Asserting parameter files against orbital velocity and
  material limits, so a figure cannot depict something arithmetically impossible. Narrow
  and achievable. It does not check reasoning in prose, and nothing here should be
  described as "CI checks the physics."

## Verification, not trust

Where an agent maintains a model that judges human writing, the answer to "who checks
the agent" is not a second agent. It is that the check must be reproducible from
committed inputs, so a human who does not trust the agent can audit the result as a
diff.

`ringgen.py --all --check` is the pattern: the figure is regenerated from its parameters
and any drift fails. An advisory that says "this contradicts orbital velocity, here is
the arithmetic" is checkable. One that says "this seems wrong" is a second opinion
wearing a CI badge.

## Open questions

1. **Who decides placement when a piece of content is genuinely ambiguous?** No
   tie-breaker is proposed. The first real case is already here:
   `docs/figures/README.md` is half mechanics ("run this to regenerate") and half
   argument about the world ("diffusion models have no representation of an orbital
   radius"). By the rule above, the argument belongs in the wiki.
2. **`CODEOWNERS` now contradicts the declared boundary.** It assigns `*` to
   @PaulTiffany, which combined with required Code Owner review means every change to
   the surface Derek owns needs Paul's approval. This was ambiguous when the file was
   written and is not any more. Deliberately not changed unilaterally in the pull
   request that introduced it — it needs Paul's confirmation and Derek's handle.
3. **The wiki is unlicensed.** See [`branch-protection.md`](branch-protection.md).
   Sharper now that the wiki is the declared home of prior art: a surface whose stated
   purpose is to fortify and credit other people's work carries no license granting
   anyone the same courtesy.
4. **Figures cross the boundary.** They are generated here and displayed on wiki pages.
   The rule is that the wiki embeds by URL and never keeps a copy, so the figure has one
   source. Unverified: GitHub has historically served raw `.svg` as `text/plain`, which
   breaks image embeds. Check with `curl -sI <raw url> | grep -i content-type` once a
   figure is on `main`, and fall back to committing a PNG beside the SVG if it does not
   render.
5. **One sanctioned cross-surface flow needs agreeing.** Any advisory checker for
   uncited claims has to live in the main repository — wikis cannot run Actions — while
   reading the wiki. That is an agent on Derek's surface reading Paul's. It should be
   read-only, report into Issues rather than writing to the wiki, and be named here
   before it is built.
6. **This document is in the wrong place by its own rule.** A governance decision is not
   CI-checkable, so by the test above it belongs in the wiki. It is here because it
   needs review before it binds anyone, which suggests the test needs a third category
   for things that are neither evidence nor current model, but agreements. Since the
   boundary above is now declared rather than proposed, the question is live: does the
   agreement live on Derek's surface, on Paul's, or in both with one of them generated?
