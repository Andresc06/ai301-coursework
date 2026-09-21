# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
|active-maintainer|last 5 commits (commit history)|at least 3 commits by a human and not a bot in the last 60 days|required|
|replies|reply times in last 5 issues|at least a reply within 30 days|preferred|
|not-archived|"This repository has been archived" banner|banner absent|required|
|active-repo|latest release date and last push to any branch|a release within the last 6 months, or a push to the default branch within the last 30 days|required|
|bounded-scope|issue body, thread, and linked-PR history|passes if the issue describes one problem or one feature, even when it lists several sub-steps that all serve that outcome. It fails if it is an explicit tracking/umbrella issue meant to be split into separate PRs, if the thread shows the approach is still undecided, or if 2+ linked PRs on it are closed/unmerged|required|
|unclaimed|assignee, linked PRs, and thread comments|no assignee, no open linked PR, and no unanswered claim comment in the thread|required|
|ai-contributions|CONTRIBUTING.md, AI_POLICY.md, or PR/issue templates|no explicit ban on AI-generated/AI-assisted contributions; if no such policy file exists, that counts as a pass|required|

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept only if every required check passes. If any required check fails or is unclear, reject the issue. Preferred checks never affect the verdict and are only used to rank accepted issues.
