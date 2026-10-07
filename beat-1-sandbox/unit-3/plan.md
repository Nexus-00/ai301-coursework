# Plan: #55, skill extractor misses JavaScript, TypeScript, Docker and PostgreSQL

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/55
Base: `main` at `2f4e82f`
Branch: `fix/55-skill-extractor-missing-languages`

## What the reproduction showed

On `2f4e82f`, `pytest tests/unit/test_skill_extractor.py -q --runxfail` gives
`5 failed, 13 passed`. The five failures are the tests the issue names, and each
one fails on its `any(...)` assertion: the expected skill is missing from the
extractor's output. Nothing crashes. The other 13 tests pass.

## Cause

Reading `ingestion/parsers/skill_extractor.py` against each failing test's input:

| Test | Input | Why nothing is detected |
|---|---|---|
| `test_javascript_detection` | `const fs = require('fs');` | The only text signal is `\b(import\|require)\s+` (line 179). Real CommonJS is `require(` with no space, so it never matches. No filename is passed. |
| `test_text_with_typescript_files` | `export interface User {...}`, `id: string` | No `import` or `require` at all, and TypeScript is only chosen when the filename ends in `.ts` (line 186). Nothing in the text counts as TS. |
| `test_database_technology_detection` | `import psycopg2` | `_detect_databases` only matches the literal key `"postgresql"`. The driver name doesn't contain it. |
| `test_devops_tool_detection` | `FROM python:3.9` / `RUN ...` / `EXPOSE 8000` | `_detect_tools` only matches the literal `"docker"`. A Dockerfile never says "docker". |
| `test_docker_compose_detection` | `services:` / `web:` / `build: .` | Same as above for a compose file. |

So it's one kind of bug in three helpers: each looks for the technology's name
and nothing else.

## Approach

All changes are in `ingestion/parsers/skill_extractor.py`, plus the test file's
markers.

1. **JavaScript text signals** (`_detect_languages`). Change the import pattern
   to `\bimport\s+|\brequire\s*\(` so `require('fs')` matches. Add one JS-only
   signal: a `const`/`let`/`var` declaration (`\b(const|let|var)\s+\w+\s*=`).
   Python has neither keyword, so this doesn't start flagging Python text.
2. **TypeScript text signals** (`_detect_languages`). Add TS syntax checks:
   an `interface Name {` declaration, a TS primitive type annotation
   (`:\s*(string|number|boolean|void|any)\b`), and a `Promise<` generic. Each
   one that matches adds an evidence line to the JS/TS list. The language name
   becomes `TypeScript` when the filename is `.ts` **or** any TS syntax signal
   matched; otherwise it stays `JavaScript`.
3. **PostgreSQL drivers** (`_detect_databases`). Add a small class-level alias
   table, `DATABASE_ALIASES = {"psycopg2": "postgresql", "psycopg": "postgresql",
   "asyncpg": "postgresql"}`. When an alias appears in the text, record the
   canonical database (same display name and confidence as the existing
   `postgresql` entry) with evidence like `Found 'psycopg2' driver in content`.
4. **Docker file syntax** (`_detect_tools`). Detect Docker when the text has
   either:
   - a Dockerfile shape: a line starting with `FROM <image>` **and** at least
     one other line starting with a Dockerfile instruction (`RUN`, `CMD`,
     `EXPOSE`, `COPY`, `WORKDIR`, `ENTRYPOINT`), both upper-case; or
   - a compose shape: a top-level `services:` line **and** an `image:` or
     `build:` key under it.

   Requiring two lines keeps a bare SQL `FROM users` from counting as Docker.
   Records `Docker` with the existing `docker` confidence.
5. **Remove the five `@pytest.mark.xfail(strict=True, reason="issue #55: ...")`
   markers** in `tests/unit/test_skill_extractor.py`, as `docs/CONTRIBUTING.md`
   asks ("Working on a seeded bug: remove its xfail marker"). The tests
   themselves don't change.

## Not in scope

- Other databases' drivers (`pymongo`, `pymysql`, ...). The alias table makes
  them a one-line follow-up, but the issue's tests only cover PostgreSQL.
- Existing false positives that the failing tests don't touch: the Python
  annotation regex matching TS's `id: string`, the `import` pattern flagging
  Python text as JavaScript, and `"git"` matching inside `"github"`. I'll mention
  these on the thread rather than fix them here.
- The display-name quirk (`"postgresql".title()` gives `Postgresql`). Changing
  it could affect anything that reads skill names downstream.
- `agent/tools/skill_extractor.py` (a separate tool with the same name).
- The `var-annotated` mypy override in `pyproject.toml`. It covers several
  modules and isn't tied to #55.

## Test plan

Run from the repo root in the venv from the reproduction.

1. **Before (on `main`)**: `pytest tests/unit/test_skill_extractor.py -q`
   shows `13 passed, 5 xfailed`. With `--runxfail`: `5 failed, 13 passed`.
2. **After (on the branch)**: `pytest tests/unit/test_skill_extractor.py -q`
   shows `18 passed`, with no `xfailed` and no `XPASS(strict)`. The five
   tests pass on their own assertions now that the markers are gone.
3. **No new false positives**: a quick check through `SkillExtractor().extract_skills`:
   - `"SELECT id\nFROM users\nWHERE id = 1"` gives no `Docker`.
   - `"import os\ndef main(): pass"` gives no `TypeScript`.
   - `"const x = 1;"` gives `JavaScript`, not `TypeScript`.
4. **Rest of the suite and CI's checks**: `pytest tests/unit -q`
   (same pass count as `main` plus the five), then `ruff check .`,
   `black --check .`, and
   `mypy api/ core/ ingestion/ rag/ agent/ safety/ --ignore-missing-imports`,
   which are the commands in `.github/workflows/ci.yml`.

## Unknowns

- The Dockerfile rule needs `FROM` plus one other instruction. A Dockerfile
  that has only a `FROM` line won't be detected. I think that's fine because a
  one-line Dockerfile is rare, and matching any `FROM` would also match SQL.
- I haven't checked whether anything downstream relies on JS-only text being
  labelled `JavaScript` and not `TypeScript`. Nothing in the repo imports
  `ingestion.parsers.skill_extractor` except its test, so I expect no impact,
  but I'll grep again before opening the PR.

## Deviations

Nothing changed in the approach; the plan held. All five steps were built as written, and the test plan's results matched what it predicted. The only change is the branch name: I used `fix/55-skill-extractor-missing-languages` instead of the name I first wrote down. It describes the issue better and still follows CONTRIBUTING's `<type>/<issue>-<description>` format.
