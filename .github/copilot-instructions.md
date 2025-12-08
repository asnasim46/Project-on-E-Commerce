<!-- Copilot instructions for Project-on-E-Commerce -->
# Copilot / AI Agent Instructions

Purpose
- Quickly orient an AI coding agent to this repository and provide concrete discovery and contribution steps.

What I found
- The repository currently contains only `README.md` at the project root. There are no source directories (`src/`), language manifests (`package.json`, `pyproject.toml`, `go.mod`, etc.), Dockerfiles, or CI workflows present.

Primary discovery steps (do these before making changes)
- Open and read `README.md` for any high-level project notes.
- Run a manifest search to detect project language and build tools:
  - `git status && git branch --show-current`
  - `rg -n "package.json|pyproject.toml|Pipfile|requirements.txt|go.mod|Cargo.toml|Gemfile|build.gradle|pom.xml|Dockerfile|Makefile|.github/workflows" -S || true`
- If a language manifest is found, follow that ecosystem's canonical build/test commands (see "If you find a manifest").

If you find a manifest
- Node: `package.json` present — run `npm ci` then `npm test` and follow `scripts` entries.
- Python: `pyproject.toml` or `requirements.txt` — create a venv, `pip install -r requirements.txt` or `pip install -e .` then `pytest`.
- Go/Rust/Java: follow `go build`, `cargo test`, `mvn test`/`gradle test` respectively.

Code and repository conventions to adopt
- When adding code, follow simple discoverable structure:
  - Put application code under `src/` or a language-idiomatic top-level folder (`app/`, `server/`, `frontend/`).
  - Put tests under `tests/` or `__tests__/` (Node/Python), matching the language's test runner expectations.
  - Add a top-level `README.md` updates describing run/build/test steps when introducing a new language.
- Branching: the default branch is `main`. Base PRs against `main` unless the user requests otherwise.

PR and commit guidance for the agent
- Create focused, single-purpose commits. Use imperative, concise commit messages (e.g., `feat: add product model and CRUD API`).
- Include a short PR description summarizing the change, the files touched, and any commands needed to exercise the change.

Integration and CI notes
- No CI workflows detected. If you add tests or linters, also add a `.github/workflows/ci.yml` that runs the project's canonical test command on Linux and reports status to PRs.

What to do when adding features
- Start by proposing a minimal change and list exact commands to run locally in the PR description.
- Add or update a `README.md` section explaining: how to install deps, how to run the app, how to run tests, and how to build a production artifact (Dockerfile or build script).

When uncertain, ask the repository owner
- If you need to pick a framework, language, or test runner, create a brief note in the PR describing alternatives and request a choice from the maintainer.

References (discovered)
- `README.md` — project root (very short: "Electronics sales, service and warranty").

If this file is outdated
- Merge intelligently: prefer the repository's existing instructions if present. If `.github/copilot-instructions.md` existed, preserve sections referencing concrete commands and only update discovery steps.

Feedback
- I added this initial guidance because the repository currently lacks code and manifests. Tell me what project's main tech stack should be (Node/Python/Rails/etc.) and I will revise instructions to include concrete build/test commands and examples.

Oracle Forms & Reports-specific notes
- This repository appears empty of source code; if you're building an Oracle Forms/Reports app, add a clear layout and document the required Oracle versions. Ask maintainer for:
  - Oracle Forms and Reports versions (e.g., 10g/11g/12c) and the Oracle DB version.
  - Whether builds run on a developer workstation, dedicated build host, or within CI.
- Recommended repo layout for Forms/Reports projects:
  - `forms/` — `.fmb` (Forms source), `.fmx` (compiled binaries), optional XML/text exports
  - `reports/` — `.rdf`/`.rep` (report source), compiled report binaries
  - `db/` — `.sql`, `.pkb`/`.pks` (PL/SQL packages), migration scripts
  - `build/` — Ant/Make/ shell scripts that wrap `sqlplus`, `frmcmp_batch`, `rwconverter`, WLST, etc.
  - `deploy/` — deployment scripts (WLST, shell wrappers) and config templates

- Common build/dev commands (examples; exact tools vary by Oracle release):
  - Run DB scripts via `sqlplus`:
    - `sqlplus user/pass@TNS < db/setup/schema.sql`
  - Compile Forms on a machine with Forms Builder installed (older suites):
    - `frmcmp_batch module=forms/myform.fmb userid=DBUSER/DBPASS@TNS module_type=form batch=yes compile_all=yes`
  - Compile Reports (where `rwconverter`/`rwclient` or `rwcompiler` are available):
    - `rwconverter userid=DBUSER/DBPASS@TNS source=reports/myreport.rdf dest=reports/myreport.rdf compile=yes`
  - PL/SQL unit tests with `utPLSQL` (recommended): run via SQL*Plus or a test runner in `build/`.

- CI and automation notes
  - Oracle Forms/Reports builders are proprietary and uncommon on CI images. Typical approaches:
    - Use a dedicated build host or VM with Oracle Developer tools installed and run builds via SSH from CI.
    - Use an internal Docker image if your company has licensed and containerized the tools.
    - Keep `build/` scripts as idempotent wrappers so CI only needs to invoke those scripts.
  - If you add CI, provide a `.github/workflows/ci.yml` that SSHes into an approved build host or triggers an internal pipeline.

- Version control and artifact strategy
  - Keep source artifacts under VCS: `.fmb`, `.rdf`, and PL/SQL source files in `db/`.
  - Where possible, include text-based exports (XML or other text dumps) alongside binaries to make diffs reviewable. If you cannot, document how to re-generate binaries.

- What I need from you
  - Oracle Forms/Reports and DB versions, and whether you have existing `.fmb`, `.rdf`, or DB scripts to add to the repo.
  - Preferred build host (local developer machines vs. centralized build server) so I can add example `build/` scripts and a CI recipe.

