# Security policy

## Reporting a vulnerability

Please do not disclose security vulnerabilities in a public issue.

Report them privately to:

- Sebastian Jeuck / DG8WA
- abusegame@kabanet.de

Include the affected version, reproduction steps, impact and any suggested
mitigation. Do not include real passwords, deletion keys, raw IP addresses or
private leaderboard data.

## Sensitive files

Never commit or publish:

- `admin/data/admin-auth.json`
- `admin/data/admin-audit.log`
- `api/data/leaderboard.sqlite`
- `admin/data/backups/`
- `admin/data/uploads/`
- `.env` files
- web-server access or error logs

## Supported version

Security fixes are applied to the current release line. Operators should keep
PHP, SQLite, ZipArchive, the web server and the operating system updated.

## Operational requirements

- Use HTTPS.
- Deny web access to `admin/data/` and `api/data/`.
- Keep runtime backups outside the webroot.
- Grant PHP write access to application files only while applying patches.
- Use strong admin passwords and remove unused accounts.
