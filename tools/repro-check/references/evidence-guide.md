# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: in an eval bundle, the repro report's `Environment:`
line (or an `Environment.` heading), compared to the issue's own
stated version and OS and to the repo facts block's latest release
line. In live mode, the issue thread's own version and OS fields
(usually filled in from the repo's bug report template), compared to
your draft's environment line, and the repo's Releases page for what's
latest.

What good looks like: the version, the OS, and anything else the
repo's bug report template asks for (build or toolchain, driver,
shell, whatever the repo facts block lists) are all named, placed
precisely enough that a maintainer could tell exactly what ran. If
that version is different from the issue's own version or from the
repo's latest release, the report says so in words ("issue filed
against X, behavior unchanged on Y") instead of leaving a stranger to
notice the gap on their own.

## Steps

Where it lives: the repro report's numbered steps or command
transcript, compared to the issue's own steps to reproduce. In live
mode, your draft's steps compared to the issue's own reproduction
section.

What good looks like: every input, file, or config the steps depend on
is shown in full, a command block, a file's exact contents, or a
precisely described UI action. Nothing is called private, internal, or
"our own setup" that a stranger can't get their hands on. The steps
reach the issue's actual trigger, in the order the issue describes it.
A named, deliberate substitution for an equivalent trigger is fine; a
silent one is not.

## Behavior shown

Where it lives: the repro report's output, log, or artifact, compared
to the issue's own stated actual behavior (its quoted error text, exit
code, whether it crashes or not, or a described symptom). In live
mode, the issue thread's own quoted error or log compared to your
draft's captured output.

What good looks like: the artifact came from actually running the
steps shown. Real output, not a claim that it ran. And it specifically
shows the issue's own behavior: the same error, the same exit code,
the same crash or non-crash character the issue reports. Not an
adjacent symptom, and not just proof that setup or an earlier step
succeeded. A report that honestly tried the issue's trigger and didn't
get the behavior still counts, an honest cannot-reproduce that says
what was tried and what might differ shows behavior just as fully as a
successful reproduction does.

## Honesty

Where it lives: the report's own concluding language, words like
"confirmed," "guaranteed," "conclusively," "100%," or just a plain
statement of fact, read against what the Behavior-shown artifact
actually demonstrates. This is the evidence family behind the
rubric's one `preferred` check, so it never turns an accept into a
reject on its own, but it's worth reading closely on every accepted
package.

What good looks like: how sure the writing sounds matches how strong
the evidence actually is. A confident claim reads fine when the
artifact backs it up exactly. It reads worse when it reaches past a
single run, a transcript that was never shown, or an artifact that
shows something else. An honest cannot-reproduce that says so plainly,
without borrowing confidence it hasn't earned, is just as calibrated
as a clean reproduction. A diagnosis or root cause claim needs a shown
transcript behind it, not just reasoning from memory.

## Comms

Where it lives: the candidate claim comment itself, and the repo
facts block's contribution or AI policy line (CONTRIBUTING.md,
AI_POLICY.md or AI_USAGE_POLICY.md, or wherever the repo states it). In
live mode, your draft claim comment and the target repo's own
CONTRIBUTING.md / AI policy doc, found on the 5-minute tour
(CONTRIBUTING, AGENTS.md, and whatever AI policy file either one
points to).

What good looks like, two things, both checkable without the repro
report:

Specificity: the claim names this issue's version and/or behavior, so
nothing in it would paste unchanged onto a different issue. It
promises a next artifact or investigation step rather than a fix, a
merge, or a timeline, and it reads like one specific person addressing
this issue, not flattery or urgency filler.

Disclosure: read what the policy actually asks for. If it names an
explicit AI-disclosure requirement for comments, only an explicit
disclosure statement satisfies it (the tool used, the extent of help).
If it only asks that comments be human-written, prose that reads as
specific and first-person satisfies it, nothing generic or
bot-voiced. If it says nothing about comments at all, silent,
permissive, or code/PR-only, there's nothing to disclose and this
passes automatically.
