# Contributing

Thank you for contributing to DG8Wa(stl's) WAG/CWA cw test trainer.

## Repository

https://github.com/helmchen81/wag-cw

## Before opening a change

1. Search existing issues and pull requests.
2. Use a dedicated branch.
3. Keep changes focused and avoid unrelated formatting changes.
4. Do not commit passwords, SQLite databases, audit logs, IP information,
   server backups, uploaded patches or `.env` files.
5. Preserve the WAG and CWA training flows unless the change intentionally
   updates their documented behavior.

## Validation

Run at least:

```bash
node --check app.js
node --check help-content.js
node --check privacy-content.js
node --check calls-data.js
node --check foreign-calls-data.js
find admin api -name '*.php' -print0 | xargs -0 -n1 php -l
```

Also test:

- desktop and smartphone layouts
- compact layout
- WAG Style and CWA Style
- ESM and manual F-key operation
- partial-call handling
- leaderboard submission and deletion
- admin authentication and CSRF protection
- moderation and GDPR deletion
- patch validation and backup creation

## Pull requests

Describe:

- the problem
- the implemented change
- affected files
- test steps and results
- compatibility or migration impact

By submitting a contribution, you agree that your contribution is licensed
under the project's MIT License.
