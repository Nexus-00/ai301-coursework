# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Nexus-00

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/55#issuecomment-5905277861

I'd like to work on this. I'll run `pytest tests/unit/test_skill_extractor.py --runxfail` on `main` at `2f4e82f` to confirm the five xfail-marked tests fail on their assertions (JS/TS in `_detect_languages`, Docker in `_detect_tools`, PostgreSQL in `_detect_databases`), then post my environment, steps, and output here before opening a PR.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/55#issuecomment-5905309375

**Result:** Reproduced. All five tests named in the issue fail their assertions on `2f4e82f`.

**Environment**

- OS: Arch Linux on WSL2, kernel `6.6.87.2-microsoft-standard-WSL2` (x86_64)
- Python 3.14.7 (venv), pytest 9.1.1
- Repo: `codepath/pathreview-ai301-fa26-s3`, `main` at `2f4e82f52efbcfcc57d65b3fa5348672163ca088`, fresh clone, no local changes

**Steps**

```bash
git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
cd pathreview-ai301-fa26-s3
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pytest tests/unit/test_skill_extractor.py -q
pytest tests/unit/test_skill_extractor.py -q --runxfail
```

The first pytest command is the issue's own step. The second adds `--runxfail`. The five tests carry `@pytest.mark.xfail(strict=True)`, so without the flag they show up as `xfailed` and the assertion output is hidden.

**Observed**

Issue's command:

```
..x....xx..xx.....                                                       [100%]
13 passed, 5 xfailed in 0.99s
```

With `--runxfail`:

```
>       assert any("typescript" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:65: AssertionError
>       assert any("postgres" in s.lower() or "sql" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:146: AssertionError
>       assert any("docker" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:162: AssertionError
>       assert any("javascript" in s.lower() or "js" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:195: AssertionError
>       assert any("docker" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:213: AssertionError
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_text_with_typescript_files
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_database_technology_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_devops_tool_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_javascript_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_docker_compose_detection
5 failed, 13 passed in 0.25s
```

**Expected:** each test finds the named skill (TypeScript, JavaScript, Docker, PostgreSQL/SQL) in the extractor's output.

**Actual:** none of the five finds it. Each one fails on its `any(...)` assertion, and the other 13 tests in the file pass. I haven't changed any code or looked into the cause yet. That comes next, before the PR.

## Eval iterations

**Run history**

1. 20/20 (bar 18/20: PASS). This is the final run, recorded in `eval-run.txt`.

**Package analysis**

`pkg-09` (sharkdp/fd#2033). My rubric said **accept**, and the gold label is **accept**. The report says outright that it did not reproduce the bug: "Result: I could NOT reproduce scenario 2", and "Actual: in every run, all ONE batches were written before any TWO batch." It passed because my checks look at whether the proof is real, not at whether the bug showed up. Behavior Shown passes when "the artifact shows the issue's behavior or says it did not reproduce." Here the attempt used the issue's trigger (two `--exec-batch` commands over the argument-size limit), the `order.log` output is shown, and the report names what differed: "uniform name lengths" and "a much lower forced limit than my 2 MiB ARG_MAX." Honesty passes because the report claims nothing its `order.log` artifact doesn't show. The environment line records fd 10.4.2 (the issue's version), Arch Linux, the kernel, and ARG_MAX. fd's policy asks for disclosure only in pull requests, so AI Disclosure passes too.

**Check rationale**

> | Honesty | The repro report's 'Actual:' line and conclusions, each compared with its artifact. | Report only claims what the artifacts show. | required

This check compares each claim with its artifact instead of asking whether the bug reproduced, because an honest cannot-reproduce is still a useful report and a confident diagnosis with nothing behind it is not. `pkg-15` shows why. It says "I verified this race condition" but has no transcript, so Honesty fails it even though it sounds certain. `pkg-09` never saw the bug, but every claim in it points to the `order.log` output, so it passes. The evidence column names the "Actual:" line and the conclusions specifically, because that's where over-claiming shows up. The steps and environment are covered by their own checks.

**Trade-offs**

The AI Disclosure check says "Rules about how comments must be written don't count." That keeps packages from failing on a voice rule: `pkg-03` (ripgrep wants human-voiced comments) and `pkg-09` (fd wants comments in the contributor's own words, with disclosure only in PRs). Both were accepted in the final run, matching gold, and `pkg-20` (ghostty, which requires disclosing all AI use) was still rejected. The cost is a repo that writes its disclosure requirement as a voice rule, like "comments must say which parts were AI-written." My rubric could read that as a voice rule and pass a package that doesn't disclose. I accept missing that case. None of the 20 packages has a policy like that, and a grader reading the actual policy text would catch it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
