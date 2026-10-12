# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. Read the plan which is in `plan.md`, including any `## Deviations` note first, and list its scope, files, test plan, and any deviations.
2. Read the issue and its thread in order to confirm what behavior the plan and PR must address.
3. Read the diff (`git diff main...HEAD`, live, or the Diff section of the bundle's Candidate PR) and the commit list, noting every changed file and hunk against the plan's stated scope from step 1.
4. Read the test evidence (`test_evidence.md`, live, or the bundle's Test evidence section) against the plan's test plan.
5. Read the PR title and description (`pr_draft.md`, live, or the bundle's Candidate PR Title/Description) last, now that you know what the diff and evidence actually show, so you can check its claims against them.

## Evidence gathering

1. For `diff-matches-plan` list every file and hunk the diff touches, and compare it against the plan's scope and any deviation notes.
2. For `description-matches-diff` list every claim the description's summary or changes sections make, and compare each against what the diff has.
3. For `test-evidence-observable` pull the before and after from test evidence, and compare it against the plan's test plan steps.
4. For `repo-checks-reported` list every check the repo's own PR template or CONTRIBUTING.md states as required (read the repo-facts block or the real template/CONTRIBUTING.md — do not assume a fixed list), and pull the reported outcome of each from test evidence.
5. For `diff-is-reviewable` list any debug leftovers, commented-out code, or hunks unrelated to the plan's scope from the diff.
6. For `standards-and-disclosure` list the repo's PR template sections and any stated contribution/AI-disclosure policy, and compare against what the description actually has.

## Check execution

Grade every check in order from `rubric.md`, even after one fails, report all of them. For each check, compare the evidence against its pass condition. Grade pass if it's explicitly stated and matches, and fail if it's explicitly stated and doesn't match. For unclear checks, grade as unclear if the needed information is missing from the package. Never assume a fact that isn't present in the package just to make a check pass.

## Verdict assembly

Hold if any required check is fail or unclear. Ready if every required check passes.
