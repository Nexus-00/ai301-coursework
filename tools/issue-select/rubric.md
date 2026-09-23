# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| recency | The last 5 default-branch commits under Repo facts (dates and authors) | At least one default-branch commit by a human less than 32 weeks old | required |
| scoped | Issue text | Fails only if (a) the issue calls itself a tracking, umbrella, meta, or mega issue, or the work is meant to be split across multiple PRs or contributors (e.g. per-module or codebase-wide tasks that different people pick up); (b) the thread shows unresolved design debate with no maintainer decision; (c) a maintainer says it needs core or internal changes; (d) it's a usage question; (e) the "linked PRs" line shows 2 or more closed, unmerged PRs and none open or merged (previous attempts were abandoned); or the issue has no labels and no maintainer (MEMBER/OWNER/COLLABORATOR) comment, meaning nobody on the project has triaged it. A list of steps or files that together make one change still passes, even if it touches several pages. A short or terse body is fine. Optional "additional suggestions" or nice-to-haves beyond the core fix don't count against scope; grade the core ask. | required |
| unclaimed | Repo claim status | Issue is unclaimed | required |
| ai-policy | The "contribution policy" line under Repo facts | Fails only on an outright ban of AI-generated contributions. Conditions (disclose, understand, test, human review) pass. No statement passes. | required |

## Verdict rule

Accept only if every required check grades pass. Any fail or unclear on a required check rejects the issue. preferred checks never change the verdict; they only rank issues that are already accepted.