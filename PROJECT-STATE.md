# DG8Wa(stl's) WAG/CWA cw test trainer

## Current state

- Current version: **3.6.0**
- Current full baseline: **3.6.0**
- Last applied patch: **none after the 3.6.0 full open-source release**
- Product identifier for patches: `dg8wastl-trainer`
- Application owner/contact: Sebastian Jeuck / DG8WA
- Contact email: abusegame@kabanet.de
- Hosting model: self-hosted
- Admin languages: German and English
- Public UI languages: German, English, French and Spanish

## Main features

- WAG Style and CWA Style CW training
- ESM and manual F-key operation
- Partial-call matching at every callsign position
- Transmit interlock and simulated pileups
- Automatic CQ when the active caller pool becomes empty
- White Noise, QRN, QSB and QRM
- Adjustable response time
- 599 Boost
- Cut Numbers and Cut DOK Numbers
- German and DX caller pools
- DARC HTML import
- WinKeyer / Web Serial support
- Battle Score
- PHP/SQLite leaderboard
- Individual leaderboard submissions
- Overall TOP 50 and per-call TOP 10 ranking
- Selectable TOP 10, 25, 50 and 100 views
- Privacy, storage and source information
- Browser-local deletion key for self-service leaderboard deletion
- Protected PHP admin area
- Leaderboard moderation
- GDPR callsign erasure
- Audit log
- Manifest- and SHA-256-protected patch installer
- Automatic backups before patching or destructive admin actions

## Important paths

- Public application: `/`
- Public leaderboard API: `/api/leaderboard.php`
- Runtime leaderboard database: `/api/data/leaderboard.sqlite`
- Administration: `/admin/`
- Admin credentials hash: `/admin/data/admin-auth.json`
- Admin audit log: `/admin/data/admin-audit.log`
- Admin backups: `/admin/data/backups/`
- Uploaded/staged patches: `/admin/data/uploads/`
- Admin documentation: `/ADMIN-README.md`
- Leaderboard documentation: `/SERVER-LEADERBOARD.md`

## Authorized admin calls

- `DG8WA`
- `DF9ZV`

The permitted admin calls are configured server-side in `admin/config.php`.

## Protected runtime data

The following files and directories must never be committed to Git or included in a public source archive:

- `api/data/leaderboard.sqlite`
- `api/data/*.sqlite-*`
- `admin/data/admin-auth.json`
- `admin/data/admin-audit.log`
- `admin/data/backups/`
- `admin/data/uploads/`
- `.env` files
- web-server logs

The project `.gitignore` excludes these paths.

## Patch rules

Normal admin patches can update only the allowlisted files defined in `admin/config.php`.

Security-sensitive files remain excluded from web patches, including:

- `admin/config.php`
- `admin/bootstrap.php`
- `admin/data/`
- `api/data/`
- `.htaccess` files
- password hashes
- audit logs
- SQLite databases

Every admin patch must contain a root-level `manifest.json` with:

- product identifier
- source version
- target version
- SHA-256 checksum for every included file

## Versioning convention

- Patch release: `3.5.1 → 3.5.2`
- Feature release: `3.5.x → 3.6.0`
- Breaking release: `3.x → 4.0.0`

Every release should update:

- visible version in `index.html`
- `APP_VERSION` in `app.js`
- cache-busting query strings
- changelog
- `README.md`
- this `PROJECT-STATE.md`

## Recommended development workflow

1. Create a feature branch.
2. Make and test changes.
3. Run JavaScript and PHP syntax checks.
4. Confirm all HTML IDs referenced by JavaScript exist.
5. Test desktop and smartphone layouts.
6. Test WAG and CWA modes.
7. Test leaderboard submission and deletion.
8. Test admin login, moderation, GDPR deletion and patch validation.
9. Update version and documentation.
10. Commit and tag the release.
11. Build a full release ZIP.
12. Build an admin-installable patch when possible.
13. Store full release and patch under `releases/` outside the webroot.

## Validation commands

```bash
node --check app.js
node --check help-content.js
node --check privacy-content.js
node --check calls-data.js
node --check foreign-calls-data.js
find admin api -name '*.php' -print0 | xargs -0 -n1 php -l
```

## Current known operational requirements

- HTTPS is required for the admin area and recommended for the complete application.
- PHP requires PDO SQLite.
- PHP requires ZipArchive for admin patch installation.
- Apache uses the included `.htaccess` files.
- nginx requires equivalent `deny all` rules for `/admin/data/` and `/api/data/`.
- PHP needs write access to runtime data directories.
- Application files need temporary PHP write access only while installing a patch.


## Open-source status

- License: MIT (`LICENSE`)
- Copyright: 2026 Sebastian Jeuck / DG8WA
- Canonical repository: https://github.com/helmchen81/wag-cw
- Contribution guide: `CONTRIBUTING.md`
- Security policy: `SECURITY.md`
- Community conduct: `CODE_OF_CONDUCT.md`
- Third-party and data notices: `NOTICE.md`
- Citation metadata: `CITATION.cff`

The MIT license applies to original material the copyright holder is entitled to license. Third-party contest resources, trademarks and callsign datasets are documented separately in `NOTICE.md`.
