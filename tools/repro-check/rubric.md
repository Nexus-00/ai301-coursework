# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment | Repro report's environment record | OS and version, runtime and version (NodeJS, CPython, etc), version of repo exists. Repo version matches what the issue targets, or the difference is stated. | required
| Reproducible | Repro report's steps | Contains steps to reproduce issue from scratch,  followable from a fresh install of the recorded version and faithful to the issue's trigger. | required
| Behavior Shown | Repro report's artifacts | The artifact shows the issue's behavior or says it did not reproduce, the attempt followed the issue's trigger, the output is shown, and any difference from the reporter's setup that could change the result is named. | required
| Honesty | The repro report's 'Actual:' line and conclusions, each compared with its artifact. | Report only claims what the artifacts show. | required
| Claim Comment | Candidate Claim Comment | Claim is specific to this issue with next steps | required
| AI Disclosure | Repo Facts | If the repo doesn't have an AI policy that requires disclosure, this passes. Otherwise, mention AI use. Rules about how comments must be written don't count. | required

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
The reproduction package is ready to post if all checks pass. If any of them do not pass, it is not ready. Any unclear checks count as failures.