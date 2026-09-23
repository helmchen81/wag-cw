# Admin-Bereich

Aufruf: `/admin/`

## Ersteinrichtung
Beim ersten Aufruf wird ein Rufzeichen und ein Kennwort abgefragt. Zulässig sind ausschließlich `DG8WA` und `DF9ZV`. Das Kennwort muss mindestens 12 Zeichen lang sein. Gespeichert wird ausschließlich ein Hash in:

`admin/data/admin-auth.json`

Zum Zurücksetzen des Admin-Zugangs diese Datei per SSH löschen. Danach startet beim nächsten Aufruf erneut die Ersteinrichtung.

## Schreibrechte
Der PHP-Benutzer benötigt Schreibrechte auf:
- `admin/data/`
- `admin/data/backups/`
- `admin/data/uploads/`
- die App-Dateien nur dann, wenn Patches über die Weboberfläche installiert werden sollen
- `api/data/leaderboard.sqlite`

Empfehlung: Patch-Schreibrechte nur temporär aktivieren und nach dem Update wieder entfernen.

## Patch-Format
Ein Patch ist ein ZIP mit `manifest.json` und ausschließlich erlaubten App-Dateien. Jeder Dateieintrag benötigt seine SHA-256-Prüfsumme. Pfade mit `..`, absolute Pfade, Symlinks und Änderungen an `admin/`, `.htaccess` oder Datenbanken werden nicht akzeptiert.

## Datenschutz
Die DSGVO-Seite löscht alle Leaderboard-Zeilen eines Calls und protokolliert nur einen Hash des Calls, die Anzahl gelöschter Datensätze, den Zeitpunkt und den Löschgrund.

## Sicherheitsnotizen
- Nur über HTTPS betreiben.
- `display_errors` in Produktion deaktivieren und PHP-Fehler serverseitig protokollieren.
- Admin-Sessions laufen nach 30 Minuten Inaktivität ab.
- Alle ändernden Formulare verwenden CSRF-Tokens.
- Patch-Uploads sind größenbeschränkt, auf erlaubte Pfade begrenzt und prüfsummengesichert.
- Backups befinden sich unter `admin/data/backups/` und sind per `.htaccess` gesperrt. Für nginx muss dieser Pfad mit einer entsprechenden `deny all`-Regel geschützt werden.


## Version 3.4.1 Login-Fix
- Der Login ist zweistufig: zuerst Call, danach Kennwort.
- Der Hinweis auf ein neues Kennwort erscheint ausschließlich, wenn `admin/data/admin-auth.json` noch nicht existiert.
- Der Session-Cookie-Pfad ist `/`, damit der CSRF-Token auch bei Installationen in einem Unterverzeichnis zuverlässig zur `/admin/`-Anfrage gehört.
- Ein CSRF-Ablauf führt zurück zum Formular statt zu einer unformatierten Fehlerseite.
- Hinweise auf zugelassene Calls und das Löschen der Kennwortdatei stehen ausschließlich in dieser Dokumentation.

## Version 3.5.0
- Admin-Link im öffentlichen Footer öffnet `/admin/` in einem neuen Fenster.
- Admin-Oberfläche unterstützt Deutsch und Englisch; Umschaltung über DE/EN im Header.
- Admin-Oberfläche und Tabellen wurden kompakter und konsistenter formatiert.
- Die Patch-Allowlist umfasst jetzt kontrollierte Admin-UI-Dateien. Sicherheitskritische Dateien wie `admin/config.php`, `admin/bootstrap.php`, `.htaccess`, Kennwort-, Audit- und Datenbankdateien bleiben ausgeschlossen.


## Open source release 3.6.0

The project source is published at https://github.com/helmchen81/wag-cw under the MIT License. Runtime credentials, databases, logs, uploads and backups remain excluded from Git. Future documentation patches may update open-source policy files, but authentication and runtime data remain protected from web patches.
