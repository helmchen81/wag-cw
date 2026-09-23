# PHP Server Leaderboard

## Requirements

- PHP 8.1 or newer
- PDO SQLite extension
- HTTPS web hosting
- Write access for the PHP/web-server user on `api/data/`

## Installation

Upload the complete trainer directory to one HTTPS website. The frontend uses the relative API endpoint:

```text
api/leaderboard.php
```

On a typical Debian or Ubuntu Apache installation, set the ownership and permissions as follows:

```bash
chown -R www-data:www-data api/data
chmod 750 api/data
```

Depending on the hosting environment, the web-server user may differ from `www-data`.

## Apache protection

The package includes:

```text
api/.htaccess
api/data/.htaccess
```

These files disable directory listings and block direct browser access to the SQLite data directory. Apache must permit `.htaccess` overrides for these rules to take effect.

## nginx protection

nginx does not process `.htaccess` files. Add a rule comparable to the following to the server configuration:

```nginx
location ^~ /api/data/ {
    deny all;
    return 403;
}
```

Reload nginx after validating its configuration.

## Security notes

The API validates submitted values, uses PDO prepared statements, and applies a simple rate limit based on a daily-changing IP hash.

Scores submitted by a browser can still be manipulated. The leaderboard is therefore intended as a friendly training leaderboard, not as tamper-proof contest adjudication.


### Ranking model since 3.3.0
The API returns individual sessions rather than aggregating by callsign. It calculates two ranks with SQLite window functions: TOP 10 per callsign and TOP 50 overall. The default frontend view shows 10 entries and can be expanded to 100. Clicking a callsign requests its complete submission history (up to 500 rows).
