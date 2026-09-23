# Backup information

This archive represents application source version **3.5.1**.

It intentionally excludes live secrets and runtime databases:

- admin password hash
- admin audit log
- leaderboard SQLite database
- uploaded patches
- generated backups

Before a server restore, copy those runtime files separately from the protected server backup.

Recommended server-side runtime backup command:

```bash
tar --create --gzip \
  --file dg8wastl-runtime-$(date +%Y%m%d-%H%M%S).tar.gz \
  api/data/leaderboard.sqlite \
  admin/data/admin-auth.json \
  admin/data/admin-audit.log \
  admin/data/backups
```

Store the resulting archive outside the webroot with restrictive permissions.
