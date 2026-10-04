# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in a bundle, the ### Diagnosis/Cause text inside the Candidate plan, read against every step and the Actual/Expected lines in the Repro evidence block. Live, the diagnosis part of plan.md, read against your posted unit-2 repro comment (or the house repro pack).
What good looks like: the stated cause explains every repro step and the actual behavior shown, not just the issue title, and never contradicts a step. For fixes-cause-not-symptom: the proposed change (below) addresses that same cause, not just the symptom the issue reports.

## Scope

Where it lives: the ### Scope/Changes text inside Candidate plan.
What good looks like: every named file or change is necessary to fix the diagnosed cause, with nothing unrelated added; it says what it won't touch.

## Executability

Where it lives: the same ### Changes text, read for its approach/order-of-work language rather than its file list.
What good looks like: a stranger could start working without asking the author anything — it names the specific file or function and what to do there, not just a general area.

## Test plan

Where it lives: the ### Test plan text, read against the numbered steps in Repro evidence.
What good looks like: reuses the repro evidence's exact steps and names the specific observable result that would confirm the fix, not just "reproduces the bug."

## Honesty

Where it lives: no package has a dedicated risks/unknowns heading — look for hedged or confident language inside Diagnosis, Changes, and Test plan itself, read against what Repro evidence actually established. Live, a ## Deviations entry once a plan is updated post-build.
What good looks like: wherever the plan expresses confidence, the repro evidence actually supports it; genuine unknowns are stated in plain words rather than glossed over.

## Comms

Where it lives: ### Candidate plan comment, read against Thread highlights and the repo-facts contribution-policy line.
What good looks like: acknowledges any maintainer signal already on the thread (e.g. limited review bandwidth) and matches any stated contribution/AI-disclosure policy.