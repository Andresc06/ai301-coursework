# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-match-evidence | the plan's stated cause ("Diagnosis"/"Cause"), read against every step in the Repro evidence block | the diagnosis is consistent with every repro step — it explains the observed behavior, not just the issue title, and doesn't contradict any step | required |
| scope | the plan's stated change (what it will touch) | every file/change it commits to addresses the diagnosed cause or an explicitly-identified sibling instance of the same defect pattern; it does not commit to unrelated refactors or features (auditing/flagging other findings without committing to fix them does not count against this) | required |
| test | the plan's test plan, read against the repro evidence's steps | the test plan is grounded in the repro evidence (reusing its steps or clearly-related variants) and names the specific observable result that confirms the fix; it does not need to match the repro's wording exactly, but every case must tie back to the diagnosed behavior | required |
| executable-by-a-stranger | the plan's approach/files section | names the specific file(s) or component(s) to change, and either the exact function, or a concrete already-demonstrated method to pin it down (e.g. a debugging trace already run) — not just a promise to investigate | required |
| honest-about-unknowns | any risk, unknown, or deferred specific the plan states, read against what the repro evidence establishes | confident claims are backed by the repro evidence; explicitly flagged unknowns, untested platforms, or deferred specifics with a stated method count as honest, not a failure — fail only when the plan asserts something as settled fact that the evidence doesn't support | required |
| fixes-cause-not-symptom | the plan's stated cause, compared against its proposed change | the change addresses the mechanism named in the diagnosis, not just the reported symptom (an example could be fixing why an error occurs, not just catching/suppressing it) | required |
| comment-is-thread-aware | the candidate plan comment, read against thread highlights and the repo-facts contribution policy | acknowledges any maintainer signal already on the thread, and matches any stated contribution/AI-disclosure policy | required |

## Verdict rule

Hold if any required check is F or unclear (?). An unclear grade is treated like a failure (F). Ready only if every required check passes. Preferred checks never change ready vs. hold.
