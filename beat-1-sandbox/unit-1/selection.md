# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/55

**Verdict output**

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````
Scope: all three candidates are in codepath/pathreview-ai301-fa26-s3. House rule applied: classmate claim comments (NONE association) don't block.

Accepted, ranked by fit profile:
1. #68 — Keyword search raises ZeroDivisionError on an empty index. Python in the RAG retriever (BM25 ranking), closest to my ML experience; small and bounded (two files, 2–4 h estimate, one xfail marker to remove).
2. #55 — Skill extractor fails to detect JavaScript/TypeScript. Python; my TypeScript experience helps with the domain (what counts as JS/TS evidence in text). Broader than #68: 5 xfailed tests across three detection helpers.
3. #53 — PII scrubber misses (555) 123-4567. Python regex fix with 4 named failing tests; weakest fit (regex in a safety module, no ML or TypeScript).

Rejected: none.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "recency", "grade": "pass", "evidence": "Latest default-branch commit 2026-09-16 by human author Aburke225, 6 days old"},
      {"name": "scoped", "grade": "pass", "evidence": "Single bug: KeywordSearcher.index([]) raises ZeroDivisionError; relevant files rag/retriever/keyword_search.py and tests/unit/test_keyword_search.py; labelled bug/good first issue/rag/tier-1"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no linked or referencing PRs; only claim is classmate acordero4852 (NONE), which the Path Review house rule ignores"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PULL_REQUEST_TEMPLATE.md contain no AI statement; silence passes"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/55",
    "checks": [
      {"name": "recency", "grade": "pass", "evidence": "Latest default-branch commit 2026-09-16 by human author Aburke225, 6 days old"},
      {"name": "scoped", "grade": "pass", "evidence": "Single bug in ingestion/parsers/skill_extractor.py detection helpers with 5 named xfailed tests as acceptance; labelled bug/ingestion/tier-1; no design debate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, zero comments, no linked or referencing PRs"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PULL_REQUEST_TEMPLATE.md contain no AI statement; silence passes"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
    "checks": [
      {"name": "recency", "grade": "pass", "evidence": "Latest default-branch commit 2026-09-16 by human author Aburke225, 6 days old"},
      {"name": "scoped", "grade": "pass", "evidence": "Single regex bug in safety/pii_scrubber.py for '(555) 123-4567' with repro and 4 named failing tests; labelled bug/good first issue/safety/tier-1"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no linked or referencing PRs; claim and repro comments are from classmate bhargavchintam (NONE), ignored under the Path Review house rule"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PULL_REQUEST_TEMPLATE.md contain no AI statement; silence passes"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `--only issue-01,issue-02,issue-03` (cheap smoke run): 1/3. Missed issue-01 (failed `scoped`) and issue-03 (`unclaimed` was `preferred`, so nothing rejected it).
2. `--only issue-01,issue-03`: 1/2. issue-03 fixed after I made `unclaimed` required; issue-01 still failed `scoped`.
3. `--only issue-01`: 0/1. Grader called it "an umbrella of bundled sub-tasks".
4. `--only issue-01,issue-03`: 1/2.
5. `--only issue-01,issue-05,issue-10`: 2/3. issue-01 still failing, now on "discrete sub-items".
6. `--only issue-01`: 0/1.
7. `--only issue-01,issue-05,issue-10`: 3/3, after I rewrote clause (a) around "meant to be split across multiple PRs or contributors".
8. Full run: `agreement: 16/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)`. Missed issue-12, issue-15, issue-19 and issue-20.
9. `--only issue-12,issue-15,issue-19,issue-20`: 4/4, after I added `ai-policy`, clause (e), the "no labels and no maintainer comment" condition, and the "additional suggestions" sentence.
10. Full run, saved as `eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Issue analysis**

**issue-15** (zulip/zulip#19589). My rubric's decision: **reject**. Gold label: **reject**.

The issue looks friendly on the surface. It carries `help wanted, area: integrations, good first issue`, the repo is active (last 5 default-branch commits run from 2026-07-14 to 2026-08-03, all by humans), and the policy "AI tools allowed; contributors must personally understand, test, and be able to explain every change" is a condition, not a ban. So `recency` and `ai-policy` pass.

The rejection comes from `scoped`. The bundle's repo-facts line reads:

> this issue: assignees: none; linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123 (closed)

That matches clause (e) of my `scoped` check: "the 'linked PRs' line shows 2 or more closed, unmerged PRs and none open or merged (previous attempts were abandoned)". The grader's evidence in the final run was: "Linked PRs line shows zulip/zulip#20840 (closed) and zulip/zulip#23123 (closed), 2+ closed unmerged PRs with none open or merged." A required check failed, so the verdict rule rejected it.

In the 16/20 run, before clause (e) existed, my rubric accepted this issue. The gold note is "years of design debate and two abandoned PRs behind a friendly label". Clause (b) (unresolved design debate) should have caught it, but the thread is 97 comments over five years and the grader couldn't reliably judge whether the debate was ever settled. The two closed PRs are a signal on one line that the grader can't miss, so I added a clause that fails on that signal.

**Check rationale**

The `scoped` check, as currently written:

> | scoped | Issue text | Fails only if (a) the issue calls itself a tracking, umbrella, meta, or mega issue, or the work is meant to be split across multiple PRs or contributors (e.g. per-module or codebase-wide tasks that different people pick up); (b) the thread shows unresolved design debate with no maintainer decision; (c) a maintainer says it needs core or internal changes; (d) it's a usage question; (e) the "linked PRs" line shows 2 or more closed, unmerged PRs and none open or merged (previous attempts were abandoned); or the issue has no labels and no maintainer (MEMBER/OWNER/COLLABORATOR) comment, meaning nobody on the project has triaged it. A list of steps or files that together make one change still passes, even if it touches several pages. A short or terse body is fine. Optional "additional suggestions" or nice-to-haves beyond the core fix don't count against scope; grade the core ask. | required |

Each part of this wording comes from a specific eval miss:

- **"Fails only if ..."** My first version asked for "one bounded change", and the grader was stricter than I meant. Listing the fail cases and letting everything else pass removes that guesswork.
- **Clause (a), "meant to be split across multiple PRs or contributors".** issue-01 (a conda docs task) kept failing. First the grader called it "an umbrella of bundled sub-tasks", then "discrete sub-items", because the issue lists several doc files to edit. The test that matters is whether the work is meant to go into separate PRs, so I made that the core of the clause. I added "A list of steps or files that together make one change still passes". The "codebase-wide tasks that different people pick up" example keeps issue-05 (codebase-wide type annotations) failing, and "mega" keeps issue-10 ("megaissue") failing.
- **Clause (e), two or more closed unmerged PRs.** Added for issue-15, which has two abandoned PRs (see Issue analysis).
- **The "no labels and no maintainer comment" condition.** Added for issue-20, a feature wish filed by `cursor[bot]` with no labels and no comments. Nobody on the project had agreed the feature should exist.
- **"Optional 'additional suggestions' ... grade the core ask."** Added for issue-19. A maintainer filed a bug with two named causes, and the grader failed it on `scoped` because of an "Additional suggestions" list below the fix.

**Trade-offs**

**What changed and how I checked it.** Clauses (e) and the triage condition tighten `scoped`, while the "additional suggestions" sentence loosens it. So after adding them I re-ran the four misses with `--only issue-12,issue-15,issue-19,issue-20` (4/4), then ran the full suite to see whether anything else flipped. The saved full run shows `clear-accept 8/8` in its categories line, so none of the eight clear-accept issues started failing.

Before writing the clauses I also checked the bundles directly:

- **Clause (e).** issue-09 (gold accept) has one closed PR (`conda/conda#11627 (closed)`), so the "2 or more" threshold lets it pass. The other bundles with closed PRs (issue-03, 05, 10, 18) also have open or merged ones, so (e) doesn't touch them.
- **The triage condition.** Every clear-accept issue has at least one label, so it can't trip.

**A case I accept it will miss.** Clause (e) says "none open or merged", so an issue with abandoned PRs *plus* open ones passes `scoped`. issue-18 (excalidraw) has `#11021 (closed)` and `#11460 (closed)` next to four open PRs, and in the final saved run the grader wrote: "linked PRs include open ones, so the abandoned-PR fail condition doesn't trigger". That's intended: open PRs are a claim, and claims belong to `unclaimed`, not `scoped`.

In the saved run, `unclaimed` passed issue-18 on "assignees: none", so it was accepted. That is my only miss (19/20, `claimed 3/4`). I'm leaving it: the run clears the bar, and adding an open-PR clause to `scoped` would duplicate the job `unclaimed` is meant to do.

The triage condition also gives something up. A real, bounded bug that a maintainer simply hasn't labelled or commented on yet will be rejected. I accept that, because in Path Review the issues are labelled, and on outside repos I'd rather skip an untriaged issue than build something the maintainers never agreed to.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

1. I am familiar with TypeScript, and I think I can spend a few hours on this issue.
2. The verdict identified that the issue is recent, it's specific to the Skill Extractor, and it is unclaimed.
3. I can claim it pretty easily.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
