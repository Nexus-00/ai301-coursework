# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->
Where it lives:
Eval: Plan context (scope, "not in scope" list, files named, any deviation note) read against Candidate PR → Diff and Description.
Live: your plan.md and its deviation notes, read against your branch's diff and your draft PR description.

What good looks like:
Every changed file and hunk falls inside the plan's stated scope or a deviation note. Everything the plan scoped is in the diff, or its absence is disclosed. The description claims nothing the diff does not do. Silent drift is a mismatch in either direction (more or less than the plan) with no note.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->
Where it lives:
Eval: Candidate PR → Test evidence, read against Plan context's test plan (and the repro steps it builds on).
Live: your captured test output (before/after) and the repo's CI or local check results, read against your plan's test plan.

What good looks like:
The evidence names the observable behavior the test plan expects and shows it after the fix (a value, output, or clean run), and shows the repo's own suite or checks ran. A check not run is acceptable only if the PR says so and why. A sentence saying the fix works ("colors work now", "tests pass") with no command or output shown is a claim, not evidence.


## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->
Where it lives:
Eval: Candidate PR → Diff and Commits.
Live: your branch's diff against the base branch and its commit list.

What good looks like:
Every hunk serves the fix and the fix is easy to find. No debug prints, commented-out blocks, dead code, formatting churn, or drive-by edits to unrelated code. Commits describe the change they contain.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->
Where it lives:
Eval: Repo facts → pull requests (PR template asks) and contribution policy (including any AI policy), read against Candidate PR → Description and Diff.
Live: the repo's PR template and CONTRIBUTING.md, read against your draft description and diff.

What good looks like:
Every required template ask is met with real content (e.g. the issue is referenced with "closes #N", a changelog/whatsnew entry is in the diff if asked for), or the PR states why it does not apply. Treat every PR as AI-assisted. If the repo mentions the use of AI assistance (either writing on comments or code generation), the description includes that disclosure, or else fail. A missing disclosure never means "no AI was used".