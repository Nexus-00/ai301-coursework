# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | Candidate PR → Diff's changed files and hunks, read against Plan context's scope, "not in scope" list, and any deviation note; Candidate PR → Description's claims read against the Diff. | Every change falls inside the plan's scope or a stated deviation; everything the plan scoped is done or its omission is disclosed; the Description claims nothing the Diff does not do. | required |
| test-evidence | Candidate PR → Test evidence, read against Plan context's test plan. | The evidence shows the command or steps that were run and their result (quoted output, or a before/after against the test plan's control) matching the after-state the test plan names, and shows the repo's own suite or checks were run, or plainly states which were not run and why. A sentence saying the fix works ("works now", "tests pass") with nothing shown is a claim, not evidence, and fails. | required |
| diff-quality | Candidate PR → Diff and Commits. | Every hunk serves the fix: no debug prints, commented-out blocks, dead code, unrelated reformatting, or drive-by edits. | required |
| repo-standards | Repo facts → PR template and contribution policy, read against Candidate PR → Description and Diff. | Every required template ask is met (e.g. issue link, changelog/whatsnew entry, tests) or the PR states why it does not apply. Treat every PR as AI-assisted: if the repo states an AI-disclosure policy, the Description must include a disclosure that meets what that policy asks for (e.g. if the policy asks for the tool and the extent of the help, both are named) or this check fails; a disclosure that meets the policy passes even if it says less; with no stated AI policy, this part passes. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
All required checks must pass before this pull request is ready to submit (accept); any required fail means hold (reject). If there are unclear checks, they count as failures. An honestly disclosed shortfall (a deviation note, a check not run with the reason stated) is graded on whether the disclosure is accurate, not as a fail by itself.