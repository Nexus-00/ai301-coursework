# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/229

**Branch**

fix/55-skill-extractor-missing-languages

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`. Missed pkg-04 (`not-tested 3/4`).
2. Full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`. Missed pkg-20 (`standards-wall 1/2`).
3. `--only pkg-04,pkg-20,pkg-01,pkg-02,pkg-13`, after tightening `test-evidence` and `repo-standards`: `agreement: 5/5 scored items`.
4. Full run with `--save-run`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`. Missed pkg-11 (`clear-accept 6/7`, `failed: repo-standards`).
5. `--only pkg-11,pkg-20,pkg-01,pkg-16`, after loosening the disclosure wording: `agreement: 4/4 scored items`.
6. Full run, saved as `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

**pkg-11** (BurntSushi/ripgrep#3477). My rubric decided **accept** in the final run. The gold label is **accept**.

The package's repo facts state ripgrep's AI policy: "AI-assisted coding is welcome with a human in the loop who understands the work; comments to maintainers must be written by humans in their own words". The PR description ends with: "Per the repo's AI policy: this change was AI-assisted with me reviewing and understanding every line, and this text is in my own words." The diff touches one file, the test evidence shows the repro re-run alongside `cargo test -p ignore passes (214 tests)`, and nothing extra rides along, so `plan-fidelity`, `test-evidence` and `diff-quality` pass.

The interesting part is `repo-standards`. In run 4 my rubric **rejected** this package on that check. I had just tightened it to say the description "must include the disclosure (naming the tool and the extent of the help)". pkg-11's disclosure doesn't name a tool, so it failed, even though ripgrep's policy never asks for one. I changed the rule so the disclosure has to meet what *that repo's* policy asks for. ripgrep asks for a human in the loop and own-words text, and the description says both, so it passes. The gold note agrees: "own-words AI disclosure per the repo's stated policy".

**Check rationale**

> | repo-standards | Repo facts → PR template and contribution policy, read against Candidate PR → Description and Diff. | Every required template ask is met (e.g. issue link, changelog/whatsnew entry, tests) or the PR states why it does not apply. Treat every PR as AI-assisted: if the repo states an AI-disclosure policy, the Description must include a disclosure that meets what that policy asks for (e.g. if the policy asks for the tool and the extent of the help, both are named) or this check fails; a disclosure that meets the policy passes even if it says less; with no stated AI policy, this part passes. | required |

The disclosure clause went through three versions:

1. **"If the repo has an AI-disclosure policy, the Description includes the disclosure."** In run 2, pkg-20 (ghostty) got through. Ghostty requires disclosing "all AI usage in any form", and the PR has no disclosure, but the grader wrote: "no evidence AI was used in producing this PR, so the strict AI_POLICY.md disclosure requirement is not triggered."
2. **I added "Treat every PR as AI-assisted"** so a missing disclosure can't be read as "no AI was used", and required the disclosure to name "the tool and the extent of the help". pkg-20 was rejected after that. But naming the tool is ghostty's rule, not everyone's, and in run 4 it rejected pkg-11 (ripgrep), whose policy doesn't ask for a tool name.
3. **"a disclosure that meets what that policy asks for ... passes even if it says less".** The bar now comes from each repo's own policy. Ghostty's tool-and-extent example stays in as an example, not a universal requirement.

I rejected keeping AI disclosure as its own check. My first rubric had a separate `AI-policy` row, but disclosure is just one of the repo's stated asks, the same as a template section or a whatsnew entry, so it belongs in the one check that reads Repo facts.

**Trade-offs**

**A package whose result it changes, and the canaries I re-ran.** Loosening the clause from "naming the tool and the extent of the help" to "meets what that policy asks for" flipped pkg-11 from reject to accept. The risk was that the same loosening would let pkg-20 back through, so I re-ran `--only pkg-11,pkg-20,pkg-01,pkg-16` (4/4):
- **pkg-20** is still rejected. Ghostty's policy itself asks for the tool and the extent, and the PR has no disclosure at all.
- **pkg-01** (pandas, the other standards-wall package) is still rejected on its unmet template asks.
- **pkg-16** (a clear-accept) still passes.

The full run then came in at 20/20 with `standards-wall 2/2` and `clear-accept 7/7`.

**A case I accept it will miss.** The check now relies on the grader reading each repo's policy correctly. A policy with an unusual ask buried in prose, like "disclose in the commit trailer, not the description", could be graded against the description anyway, and a PR that followed the policy might fail. None of the 20 packages has a policy like that. I'd rather follow each repo's own wording than go back to one fixed list that's wrong for ripgrep.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
