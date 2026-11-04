# 0002. This repository is a frozen snapshot of the flat, pre-restructure PHP backend ("Legacy" naming)

## Status
Historical — the snapshot state is verified from commits. The meaning of the repository name "HRPAuth-Legacy" is not explained anywhere in repository contents or commit messages; that part awaits author confirmation.

## Date
2026-04-11 (code import); last maintenance 2026-04-21. When the repository received the name "HRPAuth-Legacy": not verified.

## Context
The repository name itself contains "Legacy". Its content history: created 2026-02-17 with only `LICENSE` and a two-line README (`c4c55bd`); through March 2026 it received a full ecosystem-style README (`ba95370`, `f163ea6`); on 2026-04-11 it received the entire PHP backend in a single commit (`1e5ef24`, 37 files, 4543 insertions). After that, only a copy-artifact cleanup (`c0abcf7`) and README edits (`4bf227a`, `ea2efa7`). Meanwhile the sibling channel kept evolving (MVC restructure April 2026, TOTP/texture features May 2026) and on 2026-08-14 its README gained a NOTICE stating the PHP backend "has been marked as legacy" and pointing to the Go backend (`19460cd`).

## Decision
Record what "Legacy" corresponds to in the evidence: this repository preserves the flat, pre-restructure PHP backend as a snapshot, with no code-level changes after 2026-04-11.

## Consequences
- The word "legacy" never appears in this repository's contents except as the unrelated Yggdrasil feature flag `legacy_skin_api` (config/zggdrasilapi.php:36). Neither README nor any commit message explains the repository name; when and why it got this name awaits author confirmation (a GitHub rename would not be visible in git history).
- This repository's own README contains no deprecation notice; as of the last commit it still says "The project is currently **experimental** and under active development" (README.md:33). The latest commit `ea2efa7` (2026-04-21) updated the PHP version requirement in README — i.e., maintenance activity continued after the code freeze. This knowledge base therefore records no deprecation decision for this repository.
- The only deprecation-style statement in the family is the sibling channel's README NOTICE (sibling README.md:4-7, added 2026-08-14 by `19460cd`), which is sibling-repo evidence, not a statement in this repository.
- Cost: readers can mistake this repository for the currently maintained backend; its frozen code lacks all later sibling fixes (TOTP login flow, texture storage, registration SQL fix).
- The snapshot was assembled manually and imperfectly: `1e5ef24` accidentally imported files named "LICENSE copy" and "README copy.md" that `c0abcf7` ("1") had to delete, and README.md:123 still contains a pasted tool-artifact placeholder string instead of a license link.

## Evidence
- commit `c4c55bd3c02773884df98f081ee59ac32a63e8bf` — Initial commit (2026-02-17 21:44:13 +0800; only `LICENSE` + 2-line README "# HRPAuth …")
- commit `ba95370ea1e75be99b3943977000717a687e04c1` — Update README.md (2026-03-26; +125/−10, adds Requirements/Quick start/architecture sections)
- commit `1e5ef2490028d4251d899eab05e106538d388d47` — feat: implement HRPAuth backend with authentication and profile management (2026-04-11; all 37 code/doc files)
- commit `c0abcf71a362fe412773e36316a9f8c1582ecf74` — "1" (2026-04-11; deletes "LICENSE copy" and "README copy.md")
- commit `4bf227abb684c950cdbe848fb542734a53295dd9` — modify readmee (2026-04-14; README tech-stack/roadmap edits)
- commit `ea2efa75cf15ae58fd6efa2cbfc2140958337f31` — Update PHP version requirement in README (2026-04-21; README.md:54 ">=8.2" → ">=8.1")
- README.md:33 — "experimental and under active development" still present at HEAD
- config/zggdrasilapi.php:36 — only in-repo occurrence of the word "legacy" (feature flag, unrelated to repo name)
- README.md:123 — placeholder artifact "[Looks like the result wasn't safe to show…]" in the license section
- Sibling repo (read-only reference): commit `19460cd386bdefa0acfe038280db8486b125d7e6` — Update README.md (2026-08-14), adding NOTICE "This PHP-based backend has been marked as legacy … [HRPAUTH](https://github.com/CoreMatch/HRPAuth)" (sibling README.md:4-7)
- Repository name "HRPAuth-Legacy" itself: visible only in the remote URL (`origin https://github.com/CoreMatch/HRPAuth-Legacy.git`); naming rationale not verified
