# 0005. Historical position relative to the current Go core

## Status
Historical

## Date
2026-04-21 (last commit in this repository, `ea2efa75cf15ae58fd6efa2cbfc2140958337f31`); referenced Go-core commits range 2026-06-23 to 2026-11-04

## Context
The current core of HRPAuth is `CoreMatch/HRPAuth`, a Go service (go.mod:1 `module github.com/lnb/HRPAuth-Backend-Go`, Go 1.26, gin/gorm/redis stack). Its git history begins 2026-06-23 (`d509188` "Initial commit"), at which point its README was titled "# HRPAuth-Backend-Go" — i.e., it started as a Go rewrite of the PHP backend. It remains active: at the time of writing its latest commit is `8dd6759` (2026-11-04). This repository's code froze 2026-04-11, ~2 months before the Go repo's first commit. The author describes this repository as the PHP-era predecessor of the Go core; the two repositories share no git objects (verified with `git cat-file`), so the succession is product-level, not git-lineage-level.

## Decision
Record this repository's position: it preserves the PHP-era implementation that the Go core succeeded as the active HRPAuth backend.

## Consequences
- Timeline from evidence: this channel's code frozen 2026-04-11 (`1e5ef24`); README maintenance continued until 2026-04-21 (`ea2efa7`, PHP requirement ">=8.2" → ">=8.1"); the Go backend started 2026-06-23 (`d509188`); the sibling PHP channel's README NOTICE redirected users to the Go backend on 2026-08-14 (`19460cd`); on 2026-10-06 the Go core split Yggdrasil/skin features into separate microservices (Go README.md:7).
- Continuity is partial, not formal: the Go core still uses the `remember_token` concept (Go config/config.go:107-109) and the same problem domain (accounts, email verification codes, TOTP, MySQL), but no shared code or git history exists between the repositories.
- This repository contains no commit or document declaring itself superseded or deprecated; its README still says "experimental and under active development" (README.md:33) and its latest commit is itself a maintenance action. This entry therefore deliberately does not claim deprecation — only the sibling channel carries such a statement (see 0002).
- Cost/limit: using this repository as a guide to current behavior is wrong. Current external behavior is defined by HA-Contract (https://github.com/SamuelCheston/HA-Contract) and implemented by the Go core; this repository is reference material for history. Its README still points at the old `HRPAuth` GitHub org links (README.md:16-18) rather than the current `CoreMatch` org.

## Evidence
- commit `ea2efa75cf15ae58fd6efa2cbfc2140958337f31` — Update PHP version requirement in README (2026-04-21 10:51:06 +0800), last commit in this repository
- commit `1e5ef2490028d4251d899eab05e106538d388d47` — feat: implement HRPAuth backend with authentication and profile management (2026-04-11), last code change in this repository
- README.md:33 — "experimental and under active development" at HEAD; README.md:16-18 — links to `github.com/HRPAuth/*` org
- Go core (read-only reference, /home/lnb/Desktop/HA/HRPAuth): remote `origin = https://github.com/CoreMatch/HRPAuth.git`; commit `d509188dd66aa485abae27e7b33d7c6f089cf1e6` — Initial commit (2026-06-23), whose README line 1 reads "# HRPAuth-Backend-Go"; go.mod:1 `module github.com/lnb/HRPAuth-Backend-Go`
- Go core (read-only reference): commit `8dd67593bb04cb0efafed537b11f769d09328083` (2026-11-04) — latest commit at time of writing, evidence the Go core is the active implementation
- Go core (read-only reference): README.md:7 — Yggdrasil/skin features split out 2026-10-06 into HRPAuth-Yggdrasil-API / HASkinLib / HASkinProxy
- Go core (read-only reference): config/config.go:107-109 — Manage Token "is treated as a remtoken (remember_token)", continuity of the token concept
- No shared git lineage: `git cat-file -e 1e5ef2490028d4251d899eab05e106538d388d47` fails in the Go repository (object absent)
- Sibling repo (read-only reference): commit `19460cd386bdefa0acfe038280db8486b125d7e6` (2026-08-14) — README NOTICE pointing to the Go backend
- Predecessor relationship of this repo to the Go core: per author statement, corroborated by the timeline above; not verifiable from git lineage
