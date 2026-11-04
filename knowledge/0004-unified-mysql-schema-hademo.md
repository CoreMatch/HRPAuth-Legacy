# 0004. Unified MySQL schema (`hademo`) for web accounts, Yggdrasil and Authme-style data

## Status
Accepted — describes the schema shipped in this repository (`db.sql`) and how the PHP code accesses it.

## Date
2026-04-11 (commit `1e5ef2490028d4251d899eab05e106538d388d47`; the dump header itself is dated 2026-04-10)

## Context
The PHP backend persists everything in one MySQL database. `db.sql` is a phpMyAdmin 5.2.3 dump of a database named `hademo` (db.sql:1-22, header "生成日期 2026-04-10", server 5.7.44, PHP 8.1.32). The code connects through `config/db.php`, whose default database name is also `hademo` (config/db.php:4).

## Decision
Use a single MySQL database and a single wide `users` table that carries web-account fields, Yggdrasil identity fields and Authme/blessing-skin-compatible Minecraft fields together, with separate tables for profiles, tokens and sessions.

## Consequences
- Tables shipped: `users`, `profiles`, `profile_properties`, `user_properties`, `tokens`, `sessions` (db.sql:30-170). Foreign keys cascade from `users.uuid` into profiles/tokens/properties (db.sql:258-287).
- The `users` table is deliberately wide: alongside `email`, `password`, `verified`, `remember_token`, `totp` it carries Minecraft/Authme-style columns `x,y,z,world,yaw,pitch,isLogged,hasSession,regdate,regip,lastlogin` (db.sql:113-142). This is what backs the README roadmap claim of "Authme and blessing-skin support" (README.md:90).
- Cost: one wide table couples web-auth concerns to Minecraft skin-server concerns; a deployment that only needs web login still inherits the game columns. Demo rows are baked into the dump (db.sql:43-44, 64-65, 104-105, 148-149, 169-170), so importing `db.sql` seeds test data (`demoplayer`, `user@samuelchest.com`) into a live database.
- Cost: credentials default to `hademo`/`hademo`/`hademo` unless overridden by environment variables (config/db.php:3-6); a deployment that forgets the env vars silently targets the demo database.
- Limit: `docs/sql.json` describes the schema for code agents and is not guaranteed to match `db.sql`; treat `db.sql` as the source of truth for this snapshot.

## Evidence
- db.sql:1-8 — phpMyAdmin dump header (version 5.2.3, generated 2026-04-10, MySQL 5.7.44, PHP 8.1.32); db.sql:20-22 — database `hademo`
- db.sql:113-142 — `users` table definition incl. `remember_token`, `totp`, and Minecraft columns `x`,`y`,`z`,`world`,`yaw`,`pitch`,`isLogged`,`hasSession`,`regdate`,`regip`
- db.sql:30-37 `profiles`; db.sql:52-58 `profile_properties`; db.sql:73-80 `sessions`; db.sql:88-98 `tokens`; db.sql:157-163 `user_properties`
- db.sql:258-287 — foreign keys from `profiles`/`tokens`/`user_properties`/`sessions` onto `users(uuid)` / `profiles(id)`
- db.sql:43-44, 104-105, 148-149 — seeded demo rows (`demoplayer`, demo token, `user@samuelchest.com`)
- config/db.php:2-8 — `$DB` defaults host/db/user/pass all `hademo`, overridable via `DB_HOST`/`DB_NAME`/`DB_USER`/`DB_PASS`
- login.php:30-33 — `SELECT uid, password FROM users WHERE email = ?` via `getPDO()`; login.php:46-49 — issues `remember_token`
- register.php:72-90 — `INSERT INTO users (...)` incl. `verification_token`
- README.md:90 — roadmap "Authme and blessing-skin support"
- commit `1e5ef2490028d4251d899eab05e106538d388d47` — feat: implement HRPAuth backend with authentication and profile management (2026-04-11)
