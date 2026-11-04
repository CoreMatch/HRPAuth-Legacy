# HRPAuth-Legacy Knowledge Base

This knowledge base is a memo the project writes to itself. It is self-hosted and maintained inside this repository (`CoreMatch/HRPAuth-Legacy`, `knowledge/` directory). It records internal design decisions and history; it does **not** define or override external behavior. External behavior is defined exclusively by [HA-Contract](https://github.com/SamuelCheston/HA-Contract). If any entry here conflicts with HA-Contract, fix the entry, not the contract.

## Entries

| ID | Title | Status | Summary |
|----|-------|--------|---------|
| [0001](0001-dual-channel-relationship-with-hrpauth-backend-php.md) | HRPAuth-Legacy is a second channel of the same PHP codebase as HRPAuth-Backend-PHP | Accepted | Per author statement the two repos are dev/test channels of one codebase; verified tree comparison shows this repo is byte-identical to sibling commit `f68bb72` except README and a few added/missing files. |
| [0002](0002-frozen-pre-restructure-snapshot-and-legacy-naming.md) | This repository is a frozen snapshot of the flat, pre-restructure PHP backend ("Legacy" naming) | Historical | All code arrived in one commit on 2026-04-11; only README edits afterwards; the repo name "Legacy" is not explained anywhere in repo contents — pending author confirmation. |
| [0003](0003-php-era-module-layout-and-entry-points.md) | Flat per-endpoint PHP layout of the bundled backend | Accepted | One PHP script per web-root endpoint, env-driven `config/`, a `zggdrasilapi/` Yggdrasil front controller, direct PDO access, plus a bundled Vite frontend build. |
| [0004](0004-unified-mysql-schema-hademo.md) | Unified MySQL schema (`hademo`) for web accounts, Yggdrasil and Authme-style data | Accepted | `db.sql` ships one `users` table mixing web-auth and Minecraft/Authme columns, plus profiles/tokens/sessions tables; it disagrees with `register.php` and `docs/sql.json` on some columns. |
| [0005](0005-historical-position-relative-to-go-core.md) | Historical position relative to the current Go core | Historical | PHP-era predecessor; the Go core (`CoreMatch/HRPAuth`) started 2026-06-23 and is active; last maintenance here was 2026-04-21; no deprecation statement exists in this repo. |

## Maintenance

- New entries use the next sequential number: `knowledge/NNNN-<slug>.md`, starting from 0001. Each entry uses the fixed structure: `# NNNN. <title>` followed by `## Status`, `## Date`, `## Context`, `## Decision`, `## Consequences`, `## Evidence`.
- Every claim must be verified against repository contents before writing it. Evidence bullets cite `path/to/file:Lstart-Lend` and/or a full commit hash plus subject. Anything not verifiable is written as `not verified`.
- Information confirmed only verbally by the author is marked `per author statement; not verifiable from repository contents`.
- Status values: `Accepted`, `Historical`, `Superseded`. Value judgments such as "deprecated" are forbidden unless a commit or document in this repository explicitly says so.
- Sibling repositories (`CoreMatch/HRPAuth-Backend-PHP`, `CoreMatch/HRPAuth` Go core) are read-only references; cite them with full commit hashes and paths, and label them as sibling-repo evidence.
- When an entry drifts from reality or conflicts with HA-Contract, update or retire the entry in a normal commit and keep the numbering stable.

## Open questions (pending author confirmation)

- Which of the two channels (HRPAuth-Legacy vs HRPAuth-Backend-PHP) was the development channel and which was the test channel — no repo evidence distinguishes them (see 0001).
- When and why this repository was named "HRPAuth-Legacy" — neither commit messages nor README explain it; a GitHub rename would not be visible in git history (see 0002).
- Whether this channel was intended as a deployable bundle (backend + built frontend + schema dump) — inferred from tree contents only (see 0001).
