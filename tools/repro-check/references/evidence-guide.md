# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
Eval: the repro report's environment record (usually an "Environment:" line), compared with the Issue section's stated version/OS and the Repo facts' latest release.
Live: my repro.md, compared with the issue body, later comments where maintainers confirm versions, and the repo's latest release.

What good looks like: The record names the tool version, the runtime,and the OS, and the versions match what the issue targets. Where they differ, the report says so. Any detail the issue says changes the failure (driver, build profile, platform) is recorded.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->

Eval: the Candidate repro report's commands, compared with the Issue section's repro steps.
Live: your repro.md, compared with the issue body's repro section and any maintainer comments that refine it.

What good looks like:
Followable: a stranger could re-run the repro steps from a fresh install.
Faithful: the steps keep the issue's trigger (same input, flags, syntax). Any change from the issue is stated.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
Eval: the code blocks, logs, and exit codes in the Candidate repro report, compared with the error, exit code, or symptom in the Issue section.
Live: the output in your repro.md, compared with the issue body.

What to compare: the error message, the exit code, and the kind of failure (panic, wrong output, hang, data loss).

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->

Where it lives: the report's "Actual:" line, its conclusions, and phrases like "confirmed," "verified," "root cause," "guaranteed," each compared with the artifact that is supposed to back it up.
The test: for each claim, point to the line of output that proves it. If you can't, the claim is unsupported.
What fails: package says "I verified this race condition is the cause" and "I am confident," but shows no transcript or output at all. It's a confident diagnosis resting on nothing.
What passes: an honest cannot-reproduce, such as "I ran X on version Y and did not see the crash; output below."

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->

Where it lives: the Candidate claim comment (Repo facts, or CONTRIBUTING.md/AI_POLICY.md), compared with the Issue and Thread highlights.
The test: could the comment be pasted onto a different issue unchanged? If yes, it's boilerplate.
What fails: ("Kindly assign it to me, I will fix it within 2 days guaranteed") names nothing about the bug and promises a result nobody can guarantee.
What passes: package names the exact symptom and version, then states a modest next step.