# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's `Environment:` line (tool version, OS, and anything else the repo's bug report template asks for), compared to the version the issue names and the repo facts block's latest release line. | It names the version, the OS, and anything else the template asks for, with enough detail that a maintainer could tell exactly what was run. If the version is different from what the issue names or from the latest release, the report says so instead of leaving that unmentioned. | required |
| steps-followable | The repro report's steps (commands, file contents, or described actions), compared to the issue's own steps to reproduce. | Every input, file, or config the steps use is shown in full. Nothing is called private, internal, or otherwise unshareable. The steps actually reach the issue's trigger, in the order the issue describes it. A substitution is fine as long as it's named; a silent one is not. | required |
| behavior-shown | The repro report's output, log, or artifact, compared to the issue's own actual behavior (its error text, exit code, whether it crashes or not, or a symptom it describes). | The artifact came from actually running the steps shown, not just a claim that it ran, and it shows the issue's own failure: the same error, the same exit code, the same crash or non-crash behavior. Not something adjacent to it, and not just proof that setup worked. A report that honestly tried the issue's trigger and didn't get the behavior counts too, as long as it says what it tried and what might be different. | required |
| honest-outcome | How sure the report sounds (words like confirmed, guaranteed, conclusively, or just a plain statement of fact), compared to what the artifact in behavior-shown actually demonstrates. | How confident the writing sounds should match how strong the evidence actually is. A confident claim reads stronger when the artifact backs it up exactly. An honest cannot-reproduce that says so plainly, without overclaiming, counts as calibrated too. | preferred |
| claim-specific-and-honest | The candidate claim comment. | It names this issue's specific version and/or behavior, so it wouldn't make sense pasted onto a different issue. It promises a next step, a report, or an investigation, not a fix, a merge, or a date. And it reads like one person talking about this issue, not flattery, urgency, or the kind of stock phrases a bot would use ("amazing project", "guaranteed", "please assign me"). | required |
| ai-disclosure | CONTRIBUTING.md, AI_POLICY.md/AI_USAGE_POLICY.md, or the repo's stated contribution policy, read together with the claim comment and the repro report | If the policy requires disclosing AI assistance in comments, the claim comment or repro report says AI was used and how much. If it only asks that comments be human-written, the comment reads like specific, first-person writing instead of something generic. If no such policy exists, or it says nothing about comments, that counts as a pass automatically. | required |

## Verdict rule

Accept the package only if every required check that applies passes.
If any required check fails, reject it. Treat `unclear` the same as a
fail.

On a claim-only draft, before the repro report exists, only
`claim-specific-and-honest` and `ai-disclosure` decide the verdict:
they're the only checks that don't need the repro report.

`honest-outcome` is the one `preferred` check. It never changes accept
to reject or the other way around, since everything it would fail is
already caught by `behavior-shown`.