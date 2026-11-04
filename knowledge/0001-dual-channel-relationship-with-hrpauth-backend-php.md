# 0001. HRPAuth-Legacy is a second channel of the same PHP codebase as HRPAuth-Backend-PHP

## Status
Accepted — the dual-channel relationship is per author statement; the byte-level tree match below is verified from repository contents. Which channel was "development" and which was "test" has no evidence in either repository and awaits author confirmation.

## Date
2026-04-11 (dates of the code-import commits on both channels)

## Context
Two GitHub repositories under the same owner carry the PHP-era HRPAuth backend: this repository (`CoreMatch/HRPAuth-Legacy`, default branch `main`) and `CoreMatch/HRPAuth-Backend-PHP` (default branch `525`). The author states that they are two channels (development/test) of the same codebase. Nothing in either repository's files states this relationship explicitly, so it must be established by comparing the trees and histories.

## Decision
Treat HRPAuth-Legacy as a second channel of the same PHP backend codebase as HRPAuth-Backend-PHP, carrying a snapshot of the flat, pre-restructure layout plus bundled deployment artifacts.

## Consequences
- Verified tree match: of the 33 files in HRPAuth-Backend-PHP commit `f68bb72`, 31 are byte-identical in this repository (all PHP sources under `config/` and `zggdrasilapi/`, `api-doc.json`, `docs/sql.json`, `docs/yggdrasilapi.json`, `LICENSE`); `README.md` differs and root `sql.json` is absent here.
- Timing: this repository's code arrived in `1e5ef24` (2026-04-11 13:03 UTC), ~54 minutes after the sibling's `f68bb72` (12:09 UTC) and ~74 minutes before the sibling's MVC restructure `9d42c81` (14:17 UTC). This channel therefore froze the flat layout immediately before the sibling repo restructured into `app/` + `modules/` + `public/`.
- Differences versus the sibling at `f68bb72`: this repo adds a prebuilt Vite frontend (`index.html`, `vite.svg`, `assets/index-CEKpakTy.js`, `assets/index-CIKa6TRH.css`) and a phpMyAdmin schema dump (`db.sql`), and carries the ecosystem "entry point" README instead of the sibling's two-line backend README. The tree therefore looks like a deployable bundle (backend + built frontend + schema); this is an inference from tree contents, not confirmed by the author.
- Cost: fixes made on one channel never reached the other. Post-snapshot sibling commits (TOTP login flow, local texture storage, registration SQL fix, MVC refactor) are absent here, and neither repository contains any mechanism to sync the two.
- Limit: branch names differ (`main` vs `525`), READMEs differ, and the sibling kept evolving; only the shared 2026-04-11 snapshot is identical. Tooling and readers must not treat the channels as interchangeable.

## Evidence
- Per author statement: the two repositories are two channels (development/test) of the same codebase; which is dev and which is test is not verifiable from repository contents.
- Tree comparison performed against the read-only sibling worktree: for every path in `git ls-tree -r f68bb7200fe90f2d7eb00d4a5755c8fce275c7ca` (CoreMatch/HRPAuth-Backend-PHP), `git show f68bb72:<path>` was diffed against this worktree — result: 31 identical, `README.md` differs, `sql.json` missing here.
- commit `1e5ef2490028d4251d899eab05e106538d388d47` — feat: implement HRPAuth backend with authentication and profile management (2026-04-11 13:03:56 +0000; 37 files, 4543 insertions)
- Sibling repo (read-only reference): commit `f68bb7200fe90f2d7eb00d4a5755c8fce275c7ca` — feat: implement zggdrasil API server with authentication and profile management (2026-04-11 12:09:44 +0000)
- Sibling repo (read-only reference): commit `9d42c8119293878a8feb7aab581865d690aad545` — feat: restructure project with MVC architecture and implement database singleton (2026-04-11 14:17:25 +0000)
- README.md:1-6 — ecosystem "entry point" README (sibling's README at `f68bb72` is only "# HRPAuth-Backend-PHP / The PHP-based backend of HRPAuth.")
- index.html:1-14; assets/index-CEKpakTy.js; assets/index-CIKa6TRH.css; vite.svg — bundled Vite build output with no counterpart in the sibling tree
- db.sql:1-8 — phpMyAdmin dump header, no counterpart at the sibling repo root
- Remotes/branches: `origin = https://github.com/CoreMatch/HRPAuth-Legacy.git`, `origin/HEAD -> origin/main` (this repo) vs `origin/HEAD -> origin/525` (sibling worktree, `git branch -a`)
