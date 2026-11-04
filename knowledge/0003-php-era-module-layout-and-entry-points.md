# 0003. Flat per-endpoint PHP layout of the bundled backend

## Status
Accepted — describes the module layout as it exists in this repository's tree (frozen since 2026-04-11).

## Date
2026-04-11 (commit `1e5ef2490028d4251d899eab05e106538d388d47`)

## Context
This repository is the PHP-era implementation of the HRPAuth backend. Unlike the sibling channel, which was restructured into an MVC layout (`app/controllers`, `modules/`, `public/index.php` front controller) on 2026-04-11 in commit `9d42c81`, this repository preserves the original flat layout: one PHP script per endpoint served directly from the web-root, plus a bundled frontend build.

## Decision
The backend is organized as flat, directly-addressable PHP entry points with shared config, rather than a routed MVC app.

## Consequences
- Web-root auth endpoints (each a standalone script, no shared router): `register.php`, `login.php`, `logout.php`, `user.php`, `send-verification-code.php`, `verify-code.php`, `send-test-email.php`, `totpgen.php`. Each reads JSON from `php://input` and emits JSON.
- Shared configuration in `config/`: `db.php` (PDO MySQL), `memcache.php` (verification-code store), `smtp.php` (SMTP settings), `zggdrasilapi.php` (Yggdrasil server metadata + feature flags). All read from environment variables with hard-coded fallbacks.
- The Yggdrasil/authlib-injector compatibility layer lives in `zggdrasilapi/` with its own front controller `zggdrasilapi/index.php` that switches on the request URI and dispatches to handlers under `zggdrasilapi/src/{auth,session,profile,texture}/`.
- Cost: no single entry point or framework means auth, rate-limiting, CORS and error handling are re-implemented per script; the sibling channel later consolidated this into controllers and a database singleton. This flat layout is exactly what the sibling's MVC restructure replaced.
- Limit: `totpgen.php` exposes TOTP generation over plain GET with a `secret` query parameter (totpgen.php:5-11); there is no access control on it in this snapshot.

## Evidence
- Root entry points: login.php:1-56, register.php:1-98, logout.php:1-56, user.php:1-57, send-verification-code.php:1-130, verify-code.php:1-77, send-test-email.php:1-134, totpgen.php:1-45
- config/db.php:2-8 (env-driven `$DB` with `hademo` fallbacks), config/db.php:10-27 (`getPDO()` with `PDO::ERRMODE_EXCEPTION`, native prepares)
- config/memcache.php:10-19 (`getMemcached()`), config/memcache.php:21-42 (`storeVerificationCode` with Memcached-then-filesystem fallback under `cache/verification_codes`)
- config/smtp.php:2-12 (`$SMTP` env-driven), send-verification-code.php:40-113 (hand-rolled SMTP socket client `sendSMTPMail`)
- config/zggdrasilapi.php:6-41 (server name/version 0.3.3, `skin_domains`, `signature_public_key`, `feature_flags`)
- zggdrasilapi/index.php:17-95 (URI switch dispatch to `src/auth`, `src/session`, `src/profile`, `src/texture`)
- zggdrasilapi/src/utils/database.php:7-39 (`Database` singleton wrapping `getPDO()`)
- commit `1e5ef2490028d4251d899eab05e106538d388d47` — feat: implement HRPAuth backend with authentication and profile management (2026-04-11)
- Sibling MVC restructure for contrast (read-only reference): CoreMatch/HRPAuth-Backend-PHP commit `9d42c8119293878a8feb7aab581865d690aad545` (2026-04-11)
