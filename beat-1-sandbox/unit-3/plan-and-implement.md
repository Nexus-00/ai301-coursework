# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Nexus-00

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/55#issuecomment-6031936759

Here's my plan for this. It's in `ingestion/parsers/skill_extractor.py`, and all five tests fail for the same reason: each helper only looks for the technology's name.

- `_detect_languages`: the import pattern is `\b(import|require)\s+`, so `require('fs')` (no space) never matches. I'll change it to `\bimport\s+|\brequire\s*\(` and add a `const`/`let`/`var` declaration signal. For TypeScript I'll add `interface Name {`, primitive type annotations (`: string`, `: number`, ...) and `Promise<` as signals, and label the language TypeScript when the filename is `.ts` or one of those matches.
- `_detect_databases`: add an alias table mapping `psycopg2`, `psycopg` and `asyncpg` to PostgreSQL.
- `_detect_tools`: detect Docker from a Dockerfile (`FROM <image>` plus at least one other instruction like `RUN` or `EXPOSE`) or a compose file (`services:` with an `image:` or `build:` key). Requiring two lines keeps SQL's `FROM` from counting.
- Remove the five `xfail` markers, per CONTRIBUTING.

Not in scope: other databases' drivers, and some false positives I noticed but no failing test covers (the Python annotation regex matching `id: string`, `"git"` matching inside `"github"`). Happy to open separate issues for those.

To test: `pytest tests/unit/test_skill_extractor.py -q` should go from `13 passed, 5 xfailed` to `18 passed`. I'll also check that SQL with a `FROM` line doesn't produce Docker and Python text doesn't produce TypeScript, then run `pytest tests/unit`, ruff, black and mypy as CI does.

I used an AI assistant (Claude) to help draft this plan. I've read the code and checked each point myself.

---

## Your branch

**Branch**

fix/55-skill-extractor-missing-languages

**Evidence**

Same environment as the Unit 2 reproduction (Arch Linux on WSL2, Python 3.14.7, pytest 9.1.1, `.venv` from `pip install -e ".[dev]"`).

**Before**: `main` at `2f4e82f`:

```
$ git checkout main
$ pytest tests/unit/test_skill_extractor.py -q
..x....xx..xx.....                                                       [100%]
13 passed, 5 xfailed
$ pytest tests/unit/test_skill_extractor.py -q --runxfail
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_text_with_typescript_files
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_database_technology_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_devops_tool_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_javascript_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_docker_compose_detection
5 failed, 13 passed
$ pytest tests/unit -q
375 passed, 53 xfailed, 1 warning
```

**After**: `fix/55-skill-extractor-missing-languages`:

```
$ git checkout fix/55-skill-extractor-missing-languages
$ pytest tests/unit/test_skill_extractor.py -q
..................                                                       [100%]
18 passed
$ pytest tests/unit/test_skill_extractor.py -q --runxfail
..................                                                       [100%]
18 passed
$ pytest tests/unit -q
380 passed, 48 xfailed, 2 warnings
$ ruff check .
All checks passed!
$ black --check .
110 files would be left unchanged.
$ mypy api/ core/ ingestion/ rag/ agent/ safety/ --ignore-missing-imports
Success: no issues found in 76 source files
```

The five tests named in the issue now pass on their own assertions, and their `xfail` markers are removed. Across the whole unit suite, exactly those five moved from xfailed to passed (375 → 380, 53 → 48). The second warning is an unawaited `AsyncMock` coroutine from an async test elsewhere in the suite. The skill extractor tests have no async code, and running the file on its own gives no warnings.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--limit 3` (smoke run on pkg-01, pkg-02, pkg-03): `agreement: 3/3 scored items`.
2. Full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`. The one miss was pkg-20.
3. Full run with `--save-run`, saved as `eval-run.txt`, same rubric: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

**pkg-20** (ghostty-org/ghostty#11261). My rubric decided **accept**. The gold label is **reject**.

The plan itself is good, and my four checks all grade the plan's content. Solution passed on "Generation-counter fix directly addresses the stale-prev-after-page-growth diagnosis, which matches the repro's isolated trigger". Scoped passed because the plan "explicitly excludes the thread-rejected unconditional recompute and any wider refactor". Validated and Truthful passed too.

The gold note says why it should be rejected: "the comment contains no AI-use disclosure and ghostty's stated policy requires disclosing all AI usage". The package's repo facts say so directly: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". None of my checks reads the contribution policy or compares the plan comment with it. My `procedure.md` lists "AI disclosure if required" as step 0 of the read order, but nothing in `rubric.md` turns it into a grade, so a missing disclosure can't fail a package. It was a problem with the comment, and my rubric only grades the plan.

**Check rationale**

> | Scoped | The plan's scope statement against the original issue | This passes if this change fixes the problem, and does not add any other features unrelated to the issue, otherwise fail. | required |

It's written as two conditions about the change itself, not about how the scope section looks. A scope check that just asked "does the plan have a scope statement?" would pass almost every package, because almost every plan has one. Asking whether the change fixes the problem *and* adds nothing unrelated catches both directions of scope failure.

The first half is what pkg-04 (fzf) fails on. That plan is docs-only, and the grader's evidence was "Plan states 'Not in scope: any change to fzf's input handling code,' so the underlying key-swallowing behavior the issue reports is left unfixed". The plan is small and tidy, but it leaves the bug in place, and only the "fixes the problem" clause catches that. The second half is for the scope-creep packages, where the plan rewrites more than the issue asks for. All four were rejected (`scope-creep 4/4`).

**Trade-offs**

**A case I accept it will miss.** None of my checks read the repo's contribution policy or the plan comment's wording, so a strong plan whose comment breaks a repo rule gets accepted. pkg-20 is that case: ghostty requires disclosing all AI use, the comment doesn't, and my rubric accepted it (`thread-convention 1/2`). I left it as it is because the run clears the bar at 19/20. Adding a policy check would also need its own pass condition for repos that say nothing about AI, and I didn't want to add that without re-running the clear-accept packages to make sure they still pass.

**Nothing else changed, and here is how I know.** I didn't edit `rubric.md` between the full run and the saved run. The saved `eval-run.txt` records `rubric.md  sha256:e08848ebe1ad2412`, and that matches the file in `tools/plan-check/`. Both full runs gave the same categories line (`clear-accept 7/7  scope-creep 4/4  thread-convention 1/2  unbuildable 3/3  wrong-cause 4/4`), with pkg-20 as the only miss each time.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
