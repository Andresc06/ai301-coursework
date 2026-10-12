# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

Where it lives: in a bundle, the Plan context block's "Plan:" scope/files text, read against the Candidate PR's Diff. Live, `plan.md`'s Scope and Files sections (including any `## Deviations` note), read against `git diff main...HEAD`.
What good looks like: every changed file or hunk falls inside the plan's stated scope, or is explicitly re-tied by a deviation note; silent drift looks like a diff doing more (an extra file, an unrelated refactor) or less (a named file never touched, with no note explaining why) than the plan says. The description's own claims about what changed, read against the diff, are silent drift too: a description claiming more or less than the diff delivers.

## Test evidence (harness category: not-tested)

Where it lives: in a bundle, the Candidate PR's "Test evidence" section, read against the Plan context block's "Test plan:" text. Live, `test_evidence.md`'s before/after and repo-checks output, read against `plan.md`'s Test plan section.
What good looks like: decisive evidence names a specific observable result (an exact value, an exit code, a log line) and shows it both before and after, reusing the plan's test plan steps or close variants; it also reports the outcome of the repo's own checks (unit tests, integration tests, lint, typecheck), even when one fails, rather than omitting it. "Tests pass" with nothing observable named is not decisive.

## Diff quality (harness category: unreviewable)

Where it lives: in a bundle, the Candidate PR's "Diff" and "Commits" sections. Live, `git diff main...HEAD` and `git log main..HEAD --oneline`.
What good looks like: a reviewable change shows only the fix, nothing riding along; debris tells include leftover debug prints, commented-out code, unrelated formatting churn across untouched lines, or a drive-by edit to a file the plan never named.

## Standards and comms (harness category: standards-wall)

Where it lives: in a bundle, the repo-facts block's "pull requests" line (template presence and asks) and "contribution policy" line. Live, the repo's `.github/PULL_REQUEST_TEMPLATE.md` (Path Review's has Summary, Issue, Changes, Testing checkboxes including the seeded-bug xfail-marker box, Screenshots/Demo, and Notes for Reviewers) and `docs/CONTRIBUTING.md`'s stated AI-use policy.
What good looks like: every template section carries real content, not the placeholder comment it shipped with; only checkboxes that are actually true are ticked; where the repo asks for an AI-use disclosure, the description states it in the author's own words. A description that leaves a template section as its original placeholder text, or ticks a box that isn't true, fails this even if the code itself is fine. (Whether the description's claims match the diff's actual contents is plan fidelity, above — this heading is about whether the repo's own stated asks were honored.)
