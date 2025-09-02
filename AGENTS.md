# Repository Guidelines

## Project Structure & Module Organization
- `mem0/`: Python core library (packaged via Hatch).
- `server/`: FastAPI REST API (Docker-ready). OpenAPI at `/docs`.
- `mem0-ts/`: TypeScript/Node SDK (pnpm + Jest).
- `tests/`: Python tests (pytest).
- `docs/`, `examples/`, `cookbooks/`: Usage and reference materials.

## Build, Test, and Development Commands
- Python setup: `make install` (creates Hatch env). Optional: `pip install -e .[dev]`.
- Format/Lint: `make format` (ruff fmt), `make sort` (isort), `make lint` (ruff check).
- Test (Python): `make test` or `make test-py-3.11` (per-version).
- Build (Python pkg): `make build`.
- API server: `cd server && make build && make run_local` (Docker) or `uvicorn server.main:app --reload`.
- TypeScript SDK: `cd mem0-ts && pnpm i && pnpm build && pnpm test` (Node ≥18).

## Coding Style & Naming Conventions
- Python: 4-space indent, type hints; Ruff line length 120; isort `--profile black`.
  - Naming: `snake_case` for modules/functions, `PascalCase` for classes, `UPPER_SNAKE` for constants.
- TypeScript: Prettier enforced by `pnpm build`; favor `camelCase` for functions/vars and `PascalCase` for types/classes.

## Testing Guidelines
- Python: pytest in `tests/` with `test_*.py` naming. Cover new public functions and critical paths; add regression tests for bug fixes.
- TypeScript: Jest in `mem0-ts/tests`. Use focused unit tests; mock network calls.
- Run tests locally before opening a PR: `make test` and `pnpm -C mem0-ts test`.

## Commit & Pull Request Guidelines
- Commits: use conventional prefixes (e.g., `feat:`, `fix:`, `docs:`, `refactor:`). Example: `docs: add branching strategy`.
- Branching: create topic branches from `custom` (e.g., `feature/<short-topic>`). Do not commit directly to `main`.
- PRs: target `custom`. Include a clear summary, motivation, and testing notes; link related issues; add screenshots for UI-related changes (if any).
- CI/readiness: code formatted and lint-clean; tests pass.

### Create PRs with your fork as base (gh CLI)
- Preferred: create PRs from your fork targeting your `custom` branch.
- One‑liner (replace placeholders):
  - `gh pr create --repo <your_github>/<repo> --base custom --head <your_github>:<feature-branch> --fill`
- Notes:
  - `--repo` fixes the base repository to your fork (avoids defaulting to upstream).
  - `--base` is the target branch in your fork (usually `custom`).
  - `--head` explicitly points to your feature branch in your fork.
  - If you run the command inside your fork’s local clone, `--repo` can be omitted and it will still target your fork by default.

## Branching & Upstream Sync
- `main`: mirror of `upstream/main` (fast-forward only).
- `custom`: long-lived branch carrying our private patches; default PR target.
- Topic branches: `feature/<short-topic>` from `custom`.
- Sync flow: `git fetch upstream && git checkout main && git pull --ff-only upstream main && git push origin main` then `git checkout custom && git rebase main` (or `git merge --no-ff main`). Force-push to `custom` after rebase.
- Full policy: see `docs/branching-strategy.md`.

### Start a topic branch from `custom`
- Ensure `custom` is up to date:
  - `git checkout custom && git pull --ff-only origin custom`
- Create your topic branch:
  - `git checkout -b feature/<short-topic>`

### Quick diffs
- Working tree changes (unstaged): `git diff`
- Staged changes: `git diff --staged`
- Current branch vs `custom` (summary): `git diff --stat custom...HEAD`
- Current branch vs `custom` (full): `git diff custom...HEAD`
- File list vs `custom`: `git diff --name-only custom...HEAD`

## Security & Configuration Tips
- Do not commit secrets. Use environment variables; see `server/.env.example`.
- For local server, set `OPENAI_API_KEY` and backing store credentials (Postgres/Neo4j) via `.env`.
