# Plan: Git Integration of Heureka GitHub Project with GitHub Repo

Date: 2026-10-08
Repo: https://github.com/AttieLab-Systems-Genetics/Heureka
Project: `~/Heureka Bench/Heureka GitHub`

## What was examined

- **Remote repo** (cloned to `/tmp/heureka-repo` for inspection): 200 KB, single commit `553c72a` ("add attie-data skill").
- **Repo contents:**
  - `README.md` — lab index of projects and people
  - `resources.md`, `files.md`, `tokens.md` — notes from the `tryout` project
  - `RATIONALE.md`, `MEMORY.md` — snapshots of the *tryout* project's Bench files (not this project's)
  - `attie-data/` — the `attie-data` skill (SKILL.md, data_map.md, README.md, create_skill.md) — same skill installed in this workspace
- **Auth state (verified on this machine, 2026-10-08):**
  - SSH: `ssh -T git@github.com` → `Hi byandell!` — works. Key: `~/.ssh/id_ed25519`.
  - HTTPS: read works anonymously; no `gh` CLI, no credential helper configured, so HTTPS *push* would prompt for a token each time (or fail).
  - Git identity: `byandell <byandell@wisc.edu>` — already set globally.

## Key findings

1. **SSH push access is already in place.** Use the SSH remote (`git@github.com:AttieLab-Systems-Genetics/Heureka.git`); it avoids all token prompts.
2. **The local project is currently not a git repo** and is essentially empty (Code/, Data/, Documents/, Experiments/ have no files). Merging is clean — no local changes to lose.
3. **`.heureka/` must be excluded from git.** It holds auto-generated Bench state (plans, experiment logs, figures, presentation drafts, todo files). The repo's `RATIONALE.md`/`MEMORY.md` are *hand-maintained snapshots of the tryout project*, which conflicts in spirit with auto-generated same-named files in this project's `.heureka/`. Keeping them separate is the clean boundary.
4. **The `attie-data` skill lives in the repo** (`attie-data/SKILL.md`) and is also installed in the workspace (`.heureka/skills/`). Committing edits from one place and syncing the other is the ongoing workflow to standardize on.

## Recommended approach: make the project folder itself the git working tree

Git directly in `~/Heureka Bench/Heureka GitHub`. No subfolder, no bare repo — the folder you browse in Bench *is* the checkout.

### One-time setup (5 steps)

1. **Initialize git in the project root**
   `cd ~/Heureka\ Bench/Heureka\ GitHub && git init`
2. **Write `.gitignore`** (excludes Bench-internal state; keeps the repo clean):
   `.heureka/` and `.DS_Store`
3. **Add the remote** (SSH, verified working):
   `git remote add origin git@github.com:AttieLab-Systems-Genetics/Heureka.git`
4. **Pull the remote history, keeping local untracked files**:
   `git fetch origin && git reset --mixed origin/main` (or `origin/master` — check with `git remote show origin`). This adopts the remote commit as HEAD and stages the repo's files (README, attie-data/, etc.) without touching the empty local folders.
5. **First commit + push of the integrated state**:
   `git add .gitignore Documents/git-integration-plan.md && git commit -m "Integrate Heureka GitHub project with remote repo" && git push origin main`

### Ongoing workflow (from within ARC)

Just ask: *"commit and push my changes"* / *"pull the latest"*. ARC runs:
- **Commit**: `git status` → stage named files → `git commit` (the `.gitignore` makes broad staging safe).
- **Push**: `git push` (SSH, no prompts).
- **Pull**: `git pull --rebase` (keeps linear history for a single-writer repo).
- **Conflict rule**: local uncommitted changes are never overwritten; ARC reports the conflict and you decide.

### Two decisions to standardize

1. **Skill sync**: the canonical `attie-data` skill source is the repo (`attie-data/`). After editing the installed workspace copy, copy it into `attie-data/` in the repo before committing — or, simpler, declare the repo copy canonical and reinstall from it. (One-line preference; say which you want.)
2. **What the repo is for**: today it mixes `tryout` notes (`resources.md`, `tokens.md`) with `attie-data`. Suggest organizing as one project per folder (e.g., `projects/tryout/`, `projects/attie-data/`) as it grows. Not urgent.

### What does *not* get synced (and why)

- `.heureka/` — experiment logs, plans, figures, to-dos, MEMORY.md, RATIONALE.md. Auto-generated and machine-specific. The repo's `MEMORY.md`/`RATIONALE.md` are the hand-maintained versions of the tryout project and stay at the repo root as-is.
- `Code/`, `Data/`, `Experiments/` — empty now; when they hold real work, you choose per folder whether it belongs in this repo (a second repo or submodule is the right tool for a data-heavy project; this repo should stay a notes/skills repo).

### Risks and mitigations

| Risk | Mitigation |
|---|---|
| Bench file browser chokes on `.git/` folder | Unlikely — it's a normal hidden dir; no action needed unless observed |
| Accidental commit of huge data files to a notes repo | Keep this repo for notes/skills; large data lives in S3 per the attie-data data map |
| Two writers (you + another machine) editing `attie-data/` | `git pull --rebase` before each commit; repo is small so conflicts are rare and cheap |
| SSH key rotation on this machine | Symptom: push fails auth. Fix: regenerate key, add to GitHub — the HTTPS token route is the fallback |

## Verdict

The integration is low-risk and mechanical because (a) the local folder is empty, (b) SSH auth to GitHub already works, and (c) the repo is small. The whole setup takes five shell commands; the durable value is in the workflow conventions (commit-on-ask, `.heureka/` ignored, repo as notes/skills home). Say the word and ARC will execute the one-time setup and do all future commits/pushes/pulls from chat.

The plan is saved at `Documents/git-integration-plan.md` in the project, and a memory entry was logged. The next step is yours: on your go-ahead, ARC executes the five setup commands.