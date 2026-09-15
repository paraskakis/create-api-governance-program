---
name: create-api-governance-program
description: Design or fix an API governance program using a maturity model — score the current state across standards, design review, automation, lifecycle and metrics from evidence, then plan exactly one level of movement instead of an aspirational end state. Emmanuel Paraskakis's method. Use when the user says "/create-api-governance-program" or asks about API governance, rolling out an API style guide, setting up an API design review process, standing up an API council or center of excellence, enforcing standards across teams, or getting consistency across APIs built by different squads. Use it even when they never say "governance" — "every team invents their own pagination and we can't stop it", "our APIs are all inconsistent", "how do we stop breaking changes reaching customers" and "we wrote standards and nobody follows them" are all this skill. Produces a markdown scorecard plus a one-increment plan. Does not review an individual OpenAPI spec (that's `design-api-review`) and does not design an API (that's `design-api`).
license: MIT
metadata:
  version: "1.0.0"
  author: "Emmanuel Paraskakis / Level 250"
---

# Create API Governance Program

Design an API governance program that an organization will actually adopt. Score where they are today across five dimensions, then plan **one level of movement** — not the end state. This is Emmanuel Paraskakis's method for API governance, and it sits alongside the design method: `design-api` (requirements → spec) and `design-api-review` (spec → compliance verdict) govern *one* API; this skill governs *the estate*.

**Design one increment, not the destination.** The characteristic failure of governance programs is arriving fully formed — a council, a 40-page style guide, a mandatory review board, a portal — before anyone has felt the problem it solves. Teams route around it, and governance acquires a reputation as a tax. A program that moves one dimension one level, proves it, and then moves again is slower on paper and far faster in practice.

## Scope and Routing

Triggering is defined in the frontmatter description. In scope: assessing an organization's API governance maturity and designing the next increment — standards, review, automation, lifecycle, metrics, and the operating model that carries them. Out of scope: reviewing a specific OpenAPI spec against a ruleset (use `design-api-review`), designing an API (use `design-api`), and writing the style guide's individual rules — this skill decides that a style guide is the next increment and who owns it; `design-api-review`'s bundled `API-standards.md` is a good starting ruleset to hand them.

Security governance (threat modelling, auth standards, pen-test gates) usually runs as its own program with its own owner. Score what touches API design here, and say plainly when security belongs in a separate track rather than absorbing it.

## Inputs

**Nothing is required.** Ask in one line and start from whatever comes back:

> Tell me about your API estate — roughly how many APIs, how many teams, and what's going wrong that made you think about governance.

If they point you at a repo, an org, or a set of specs, read them: real artifacts beat self-report, and the assessment gets much sharper. If they give you only a sentence of context, run the assessment as an interview instead. Never block on inputs — a governance conversation that opens with a document request loses the room.

## Why one increment

Maturity models invite a specific mistake: seeing five levels and planning to reach level five. Resist it. Levels aren't a ladder an organization climbs on schedule; they're a description of what's currently load-bearing. Jumping two levels in a dimension means building enforcement for standards nobody has internalized, or dashboards measuring a process that doesn't yet happen. The artifact exists, the behavior doesn't, and the program is discredited.

One level per dimension per cycle also keeps the plan falsifiable. If moving design review from documented to reviewed doesn't reduce the inconsistency people complained about, you learn that cheaply and can change course — which is impossible when eleven things shipped simultaneously.

## The maturity model

Five dimensions, five levels each. Read `references/maturity-model.md` for the full matrix, the evidence signals that justify each score, and the scoring rules — do this before assigning any scores, because scoring from memory produces flattering, uniform results.

| Dimension | The question it answers |
|---|---|
| **Standards** | Is there a written ruleset, owned and versioned? |
| **Design review** | Does anything check a design *before* it's built? |
| **Automation** | Can a machine catch violations without a meeting? |
| **Lifecycle** | Are versioning, deprecation and discovery governed? |
| **Metrics & feedback** | Is governance measured, and do the standards change as a result? |

Levels run: **1 Ad hoc → 2 Documented → 3 Reviewed → 4 Automated → 5 Measured**. Expect dimensions to sit at genuinely different levels — an org with a beautiful style guide and no CI check is Standards 2, Automation 1, and that gap *is* the finding.

## Stage 1: Assess from evidence

Score each dimension 1–5, and record the evidence beside each score. Evidence means an artifact you or they can point to: the style guide's URL, the linter config in the repo, the CI job that runs it, the deprecation policy, the catalog. "We have standards" is not evidence; a file is.

When you have repo access, go looking — a ruleset file, a CI workflow that runs it, whether the check blocks a merge or just warns, deprecation headers in live specs. When you don't, ask for the artifact by name rather than asking whether the practice exists: "where does the style guide live?" surfaces the truth faster than "do you have standards?", because the answer to the second is almost always yes.

Score what is true today, not what is written down as policy. A mandatory review that gets skipped under deadline is not level 3. This will occasionally be uncomfortable, and it is the most valuable thing in the assessment — an honest low score is what makes the increment plan credible.

## Stage 2: Name the pain, then pick the target

Before choosing what to improve, name the one to three failures the organization is actually experiencing: customers hit breaking changes, every team invented its own pagination, onboarding to an internal API takes three weeks, the same capability got built twice.

Then pick the dimension(s) to move by asking which movement most relieves that pain — **not** which dimension scores lowest. The lowest score is often lowest because it doesn't matter here. An org bleeding from breaking changes needs Lifecycle 1→2 and Automation 1→2 far more than it needs a Metrics dimension it would never read.

Move one or two dimensions. Three is usually a sign the pain hasn't been narrowed enough.

## Stage 3: Specify the increment

For each moving dimension, make the next level concrete: what gets built, who owns it by name or role, what "done" looks like, and what it costs in people and weeks. An increment that can't name an owner isn't a plan, it's a wish.

Push enforcement toward automation wherever the rule is machine-checkable. A review board that spends its time catching naming violations a linter would catch is burning the organization's scarcest governance resource — senior attention and goodwill — on work a CI job does for free. Reserve human review for what machines genuinely can't judge: whether the resource model matches the domain, whether this API should exist at all.

## Stage 4: Rollout

Start with a pilot team that wants it. Volunteers produce a reference implementation and an advocate; conscripts produce compliance theatre and a cautionary tale. Name the pilot, the expansion sequence, and the point at which the increment becomes default rather than optional.

Co-author standards with the teams who'll follow them. A ruleset handed down is argued with; a ruleset contributed to is defended. This is slower by a week or two and is most of the difference between adoption and circumvention.

## Stage 5: Set the re-assess cadence

Say when they score again — a quarter is usually right — and what evidence they'll bring. A maturity model used once is a slide; used on a cadence it becomes the program's feedback loop.

## Output

Write the program to a markdown file using this structure:

```markdown
# API Governance Program: [org or estate scope]
*Assessed [date] · [N] APIs across [M] teams*

## Where you are today
| Dimension | Level | Evidence |
|---|---|---|
| Standards | [1-5] | [the artifact, or its absence] |
| Design review | [1-5] | [...] |
| Automation | [1-5] | [...] |
| Lifecycle | [1-5] | [...] |
| Metrics & feedback | [1-5] | [...] |

## The pain this program must relieve
1. [Named failure, with how it shows up]

## Target: one level of movement
[Dimension] [current]→[next] — because [how it relieves the named pain]

## The increment
### [Dimension]: [current]→[next]
- **Build:** [the artifact or practice]
- **Owner:** [role or name]
- **Done looks like:** [observable condition]
- **Cost:** [people, weeks]

## Rollout
- **Pilot:** [team, why them, duration]
- **Expand:** [sequence]
- **Becomes default:** [trigger]

## How you'll know it worked
[Measures tied to the named pain, with current baseline]

## Deliberately not doing yet
| Dimension | Staying at | Revisit when |
|---|---|---|
| [...] | [level] | [condition] |

## Re-assess
[Date/cadence, and what evidence to bring]
```

The "Deliberately not doing yet" table is load-bearing, not filler. Governance programs get padded because saying "not yet" feels like under-delivering; writing the deferrals down as decisions with revisit triggers is what lets the increment stay small without looking careless.

## Key principles

1. **Score from artifacts, not assertions.** Ask where the style guide lives, not whether one exists. Every score in the table carries its evidence, and an honest low score is worth more than a generous one — the whole plan rests on the assessment being believable.
2. **One level, one or two dimensions.** The pull toward designing level five is strong and it is the failure mode this method exists to prevent. Movement that lands beats a program that impresses.
3. **Pain picks the target, not the lowest score.** Improve the dimension whose movement relieves a named failure. A dimension can sit at level 1 indefinitely if nothing depends on it.
4. **Automate what's machine-checkable; reserve humans for judgment.** Governance capacity is senior attention, and it's finite. Spending it on naming conventions a linter catches is how review boards become bottlenecks and then get abandoned.
5. **Volunteers first.** Pilot with a team that wants it. Adoption spreads through a working example and a peer who vouches for it, far better than through a mandate.
6. **Deferrals are decisions.** Write down what you're not doing and when to revisit it. That's what makes a small increment defensible to the sponsor asking why the program isn't bigger.
