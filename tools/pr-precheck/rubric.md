# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | the diff's changed files or lines, read against the plan's scope and any `## Deviations` notes | every changed file or hunk falls inside the plan's stated scope, or is explicitly covered by a deviation note; nothing the plan didn't authorize is silently included | required |
| description-matches-diff | the PR description's Summary/Changes claims, read against the actual diff | every claim in the description is something the diff actually does; the diff doesn't do more or less than described without a stated reason | required |
| test-evidence-observable | `test_evidence.md`'s before/after, read against the plan's test plan | names a specific observable result, reuses the plan's test plan steps or clearly-related variants, and shows both before and after | required |
| repo-checks-reported | `test_evidence.md`'s results for every check the repo's own PR template/CONTRIBUTING.md states (for this repo: unit tests, integration tests, lint, typecheck, and frontend tests) | every one of the repo's stated checks has its actual outcome reported, pass or fail, with a reason if it failed; a failing check reported honestly still passes this — an unreported or hidden check fails it | required |
| diff-is-reviewable | the diff itself and the commit list | no debug leftovers, commented-out code, or unrelated drive-by edits. Every hunk is traceable to the diagnosed fix | required |
| standards-and-disclosure | the PR description, read against the repo's PR template sections and `docs/CONTRIBUTING.md`'s stated policy | every template section has real content, not placeholder text; any stated AI-use disclosure ask is addressed | required |

## Verdict rule

Hold if any required check is fail or unclear. An unclear grade is treated like a failure. Ready only if every required check passes. A PR that honestly discloses a shortfall is graded on what it discloses, not penalized just for disclosing less than a perfect run.
