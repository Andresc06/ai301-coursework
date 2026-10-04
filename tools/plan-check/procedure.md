# Procedure: how this skill grades a plan package

## Read order

1. Read the issue body and its comment thread (Thread highlights) first, to understand what behavior is reported and any maintainer signals.
2. Read the Repro evidence block next, and note exactly what it pins down: the steps, the actual output, and the expected output.
3. Read the candidate plan (Diagnosis, Scope, Changes, Test plan) and the candidate plan comment last, now that you know what behavior and signals they must match.

## Evidence gathering

Pull the following text blocks: ### Diagnosis, ### Scope, ### Changes, ### Test plan, and ### Candidate plan comment — and compare each against the Repro evidence block, the Thread highlights, and the repo-facts contribution-policy line, as relevant to the check being graded.

## Check execution

Fail if the comparison doesn't match the specified criteria. Pass if it does and is explicitly stated. Unclear if the criteria or the needed information is missing.

## Verdict assembly

Hold if any required check is false or unclear (?). Ready if every required check passes. Preferred checks never change ready vs. hold.