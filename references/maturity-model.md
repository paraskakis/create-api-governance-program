# API Governance Maturity Model

Five dimensions, five levels. Read the level descriptors before scoring — scoring from intuition produces uniformly flattering results clustered at 3.

## Contents
- [Scoring rules](#scoring-rules)
- [Level meanings](#level-meanings)
- [Dimension 1: Standards](#dimension-1-standards)
- [Dimension 2: Design review](#dimension-2-design-review)
- [Dimension 3: Automation](#dimension-3-automation)
- [Dimension 4: Lifecycle](#dimension-4-lifecycle)
- [Dimension 5: Metrics & feedback](#dimension-5-metrics--feedback)
- [Common patterns](#common-patterns)

## Scoring rules

**Score behavior, not policy.** A documented mandatory review that teams skip when the deadline is tight scores 2, not 3. What's written in Confluence is evidence of intent; what happens in the merge queue is evidence of level.

**A level requires the levels beneath it.** Automation scores 4 only if the standards it enforces actually exist (Standards ≥ 2). CI running a ruleset nobody agreed to is a source of ignored warnings, not level-4 automation — score the reality, which is usually Automation 2 with an unusual amount of noise.

**Score the estate, not the best team.** One exemplary squad with a perfect pipeline doesn't lift the organization's score. Ask what a *randomly chosen* API does. If practice varies wildly, score the median and note the variance — that spread is often the most actionable finding in the assessment.

**Whole numbers only.** Half-levels are a way of avoiding an uncomfortable score. Round down: if you're torn between 2 and 3, it's 2.

**Partial credit doesn't accumulate across dimensions.** Each dimension scores independently. Resist averaging into an overall "maturity score" — a single number hides exactly the imbalance that tells you what to fix, and invites the org to chase the number.

## Level meanings

| Level | Name | What's true |
|---|---|---|
| 1 | Ad hoc | Nothing shared. Outcomes depend entirely on who happened to build it. |
| 2 | Documented | It's written down and findable. Following it is optional or unverified. |
| 3 | Reviewed | A human checks before release. The gate is real but manual, and its cost scales with volume. |
| 4 | Automated | A machine checks, in the pipeline, without a meeting. The gate holds at scale. |
| 5 | Measured | Compliance and impact are measured, and the data changes the standards themselves. |

Level 5 is not "more automation" — it's the feedback loop closing. An org can be excellently automated at 4 forever and never learn that rule 17 is ignored by every team because it's wrong.

---

## Dimension 1: Standards

Is there a written ruleset, owned and versioned?

| Level | Descriptor |
|---|---|
| 1 | No shared conventions. Each team's API reflects its own habits. Consistency is coincidence. |
| 2 | A style guide exists and is findable. Unclear ownership, rarely updated, compliance unknown. |
| 3 | Standards are owned by a named person or group, versioned, and consulted during review. Exceptions are discussed. |
| 4 | Standards are expressed as machine-readable rules (a linter ruleset) alongside the prose, so the document and the check cannot drift apart. |
| 5 | Rules carry usage data — which are violated most, which get waived — and that evidence drives revisions. Rules that consistently lose their arguments get removed. |

**Evidence signals:** the style guide's URL or repo path; its last commit date; a named owner; a CHANGELOG or version history; whether a linter ruleset exists beside the prose; whether waivers/exceptions are recorded anywhere.

**Frequent trap:** a thorough, handsome, two-year-old style guide nobody has opened. That is level 2 — the artifact's quality doesn't raise the score, its use does. Check the commit date and ask when it was last cited in a review.

---

## Dimension 2: Design review

Does anything check a design *before* it's built?

| Level | Descriptor |
|---|---|
| 1 | No design step. APIs are reviewed, if at all, as implementation code in a PR — after the contract is already expensive to change. |
| 2 | A review process is documented and available, typically opt-in. Some teams use it; most discover it after shipping. |
| 3 | Design review reliably happens before build for APIs in scope, with a checklist and a stated turnaround. Skipping is an exception someone notices. |
| 4 | Routine conformance checks moved to automation; human review is reserved for domain modelling, resource design and whether the API should exist. Reviewers see a pre-linted spec. |
| 5 | Review outcomes are tracked — turnaround time, findings by category, rework rate — and recurring findings get promoted into automated rules or into the standards. |

**Evidence signals:** a review checklist; where review is requested (ticket type, PR template, channel); a stated SLA and whether it's met; ADRs or decision records; what fraction of last quarter's APIs actually went through it.

**Frequent trap:** scoring 3 because a review board exists. Ask how many APIs shipped last quarter and how many the board saw. The ratio is the score.

---

## Dimension 3: Automation

Can a machine catch violations without a meeting?

| Level | Descriptor |
|---|---|
| 1 | No automated checking. Specs may not even be validated for syntax. |
| 2 | Linting exists but runs locally or ad hoc, or runs in CI as a non-blocking warning stream teams have learned to ignore. |
| 3 | Spec validation and linting run in CI on every change, visibly, and failures are expected to be addressed — though the gate can be overridden without ceremony. |
| 4 | The gate blocks merge. Breaking-change detection runs against the published version. Waivers are explicit, attributed and time-bound rather than a silent override. |
| 5 | Rule-level pass/fail data is collected across the estate and feeds standards revision; noisy or low-value rules are tuned or retired rather than universally suppressed. |

**Evidence signals:** a ruleset config (`.spectral.yaml`, `redocly.yaml`, vacuum, 42Crunch); the CI workflow file that invokes it; whether the job is required or advisory in branch protection; breaking-change tooling (`oasdiff` or equivalent); how overrides are recorded.

**Frequent trap:** a `.spectral.yaml` in the repo scores nothing by itself. Find the workflow that runs it and check whether the check is required. A config with no runner is level 1 with good intentions.

---

## Dimension 4: Lifecycle

Are versioning, deprecation and discovery governed?

| Level | Descriptor |
|---|---|
| 1 | No shared versioning approach. Deprecation happens by announcement, or by breakage. Nobody has a list of the organization's APIs. |
| 2 | Versioning and deprecation policies are written. An inventory exists somewhere and is partly accurate. |
| 3 | Policies are followed for new work: versions are negotiated deliberately, deprecations get notice periods, a catalog is maintained and mostly current. |
| 4 | Lifecycle state is machine-visible — `Deprecation` and `Sunset` headers, catalog entries generated from specs rather than hand-maintained, automated consumer notification. |
| 5 | Consumer migration is measured: who still calls the deprecated version, how adoption of the new one is progressing, whether sunset dates hold. Retirement decisions use that data. |

**Evidence signals:** a versioning policy; a deprecation policy with a notice period; an API catalog or developer portal and how entries get there; `Deprecation`/`Sunset` headers in live responses; a record of an API actually retired on schedule.

**Frequent trap:** confusing a developer portal with lifecycle governance. A portal is a discovery surface; it scores here only to the extent it reflects real lifecycle state. A beautiful portal listing three deprecated APIs as current is evidence for level 2.

---

## Dimension 5: Metrics & feedback

Is governance measured, and do the standards change as a result?

| Level | Descriptor |
|---|---|
| 1 | Nothing measured. Whether governance is working is a matter of opinion. |
| 2 | Activity is counted — reviews held, APIs catalogued — without connection to outcomes. |
| 3 | Compliance is measured across the estate: what share of APIs pass the ruleset, what share went through review. |
| 4 | Outcome measures are tracked, not just compliance: time-to-first-successful-call, integration support load, breaking changes reaching consumers, duplicate capability. |
| 5 | The loop closes. Measurement changes the program — rules are retired, review scope narrows or widens, the increment plan is rewritten from evidence on a cadence. |

**Evidence signals:** a dashboard and who looks at it; lint pass rate across repos; review turnaround; consumer-reported integration issues; any recorded instance of a standard being changed *because of* data.

**Frequent trap:** counting activity and calling it measurement. "We held 40 reviews" is level 2. "Time-to-first-call dropped from 9 days to 2" is level 4. The test is whether the number could ever cause someone to stop doing something.

---

## Common patterns

Recognizing the shape of an estate speeds up both scoring and target selection.

**The handsome document** — Standards 3, everything else 1. Someone wrote an excellent style guide; nothing enforces it and teams don't read it. The instinct is to improve the document. The increment that pays is Automation 1→2: make a subset of those rules machine-checkable so the guide starts having consequences.

**The bottleneck board** — Design review 3, Automation 1. Every API goes through a review board that catches naming and pagination issues by hand. Review is slow, teams resent it, and it's beginning to be skipped. Moving Automation 1→3 doesn't reduce governance, it rescues it — the board's remaining time goes to domain modelling, which is what it was for.

**The tooling-first estate** — Automation 3, Standards 2, Design review 1. Strong platform engineering bought linting before anyone agreed what the rules should be. Warnings are ignored because they encode one team's preferences. The increment is Standards 2→3: get named ownership and genuine agreement, then the existing automation starts working.

**The silent breaker** — Lifecycle 1, everything else 2–3. Design quality is fine; consumers get broken without warning. Nothing in standards or review addresses versioning. Lifecycle 1→2 plus breaking-change detection in CI relieves the pain the org actually feels.

**Uniform 1s** — a genuinely greenfield or small estate. The temptation is a full program. Usually the right increment is Standards 1→2 alone: a short ruleset, one owner, and nothing else until something hurts.
