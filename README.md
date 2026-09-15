# create-api-governance-program

A [Claude Code](https://claude.com/claude-code) skill for designing an API governance
program that an organization will actually adopt.

Part of Emmanuel Paraskakis's API method. Where `design-api` takes requirements to a
spec and `design-api-review` checks a spec against standards, those govern *one* API.
This one governs *the estate*.

## What it does

Scores an organization across five dimensions, then designs **one level of movement** —
not the end state.

| Dimension | The question it answers |
|---|---|
| Standards | Is there a written ruleset, owned and versioned? |
| Design review | Does anything check a design *before* it's built? |
| Automation | Can a machine catch violations without a meeting? |
| Lifecycle | Are versioning, deprecation and discovery governed? |
| Metrics & feedback | Is governance measured, and do the standards change as a result? |

Levels run **1 Ad hoc → 2 Documented → 3 Reviewed → 4 Automated → 5 Measured**.

The output is a markdown scorecard, a one-increment plan with named owners, a rollout
starting from a volunteer pilot, and a "deliberately not doing yet" table where each
deferral carries a revisit trigger.

## Why one increment

Governance programs characteristically fail by arriving fully formed — a council, a
40-page style guide, a mandatory review board, a portal — before anyone has felt the
problem they solve. Teams route around it and governance becomes a tax. Moving one
dimension one level, proving it, then moving again is slower on paper and faster in
practice.

## Layout

```
SKILL.md                      the method
references/maturity-model.md  5x5 matrix, evidence signals, scoring rules,
                              and five common estate patterns
evals/evals.json              3 test cases, 31 assertions
```

## Installing

Symlink it where Claude Code looks for skills:

```bash
ln -s "$PWD" ~/.claude/skills/create-api-governance-program
```

Then invoke with `/create-api-governance-program`, or just describe the problem —
"every team invents their own pagination and we can't stop it" triggers it too.

## Evaluation

Benchmarked against an unassisted baseline on the three cases in `evals/`:
100% (31/31) with the skill, 40% (13/31) without.

The pass rate overstates it. The assertions that actually discriminate all test one
behaviour — restraint in scope selection. The more interesting result is that on two
of three cases the baseline *wrote down the correct principle and then violated it*
("Aim for one level of movement, not an end state" — then moved five dimensions by two
levels). The skill isn't supplying knowledge the model lacks; it's supplying a
constraint the model won't impose on itself.

Known gaps, for the next iteration:

- Stage 1 says to *look for* artifacts but never says to *run* them. A test run claimed
  a linter wasn't installed and hand-evaluated rules instead; it was installed, and the
  unassisted baseline that ran it found the better result.
- The assertion set is blind to substance quality — a tightly-scoped but shallow plan
  would still score 31/31.
- The "names an owner" assertion rewards the failure mode: a bigger framework names
  more owners. It needs rebinding to the increment.

## License

MIT
