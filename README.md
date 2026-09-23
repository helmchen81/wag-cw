# DG8Wa(stl's) WAG/CWA cw test trainer

[GitHub repository](https://github.com/helmchen81/wag-cw) · [MIT License](LICENSE) · [Security policy](SECURITY.md) · [Contributing](CONTRIBUTING.md)

## Open source

The original software code and original project documentation are open source under the **MIT License**. You may use, copy, modify and distribute the software under the conditions in `LICENSE`. Third-party contest information, trademarks, callsign/DOK data and externally sourced content remain subject to their respective rights; see `NOTICE.md`.

Lokale Web-App für WAG-Contesttraining mit 17.377 bereinigten Call/DOK-Datensätzen aus der bereitgestellten `calls.txt`.

## Start

Web Serial und einige Browser-Audiofunktionen benötigen einen sicheren Kontext. Starte deshalb lokal einen kleinen Webserver:

```bash
cd wag-cw-trainer-src
python -m http.server 8080
```

Dann in **Microsoft Edge** oder **Google Chrome** öffnen:

```text
http://localhost:8080
```

## Enthalten

- Einstellbarer Contest-Zeitraum, CW min/max und CQ-Speed
- 1 bis 10 gleichzeitige Caller
- WAG-typischer Call/DOK-Austausch, historische Aktivitätsgewichtung
- White Noise, QRN-Impulse, QSB/Fading, Pitch-Streuung, optionales QRM
- Cut Numbers in drei Profilen
- N1MM-ähnliche F1 bis F12-Belegung und Tastatur-Workflow
- Web-Serial-Anbindung für WinKeyer, ASCII-Senden und optionales Echo als CW-Eingabe
- Live-Rate, Genauigkeit, Multis, Trainingsscore
- Cabrillo-artiger Export plus CSV-Daten
- Responsive Oberfläche und LocalStorage-Konfiguration

## WinKeyer-Hinweis

Die serielle Schnittstelle ist absichtlich konservativ implementiert. WinKeyer-Versionen und Firmware-Konfigurationen unterscheiden sich. Standard ist 1200 Baud und ASCII-Ausgabe. Vor echtem Sendebetrieb ausschließlich mit deaktiviertem Transceiver beziehungsweise Dummy Load testen. Für vollständige Paddle-Dekodierung kann je nach Gerät eine Anpassung an dessen Host-Protokoll nötig sein.

## Daten und Simulation

Die Call/DOK-Datei ist die primäre Stationsbasis. Häufige Stationen aus veröffentlichten WAG-Ergebnislisten werden mit erhöhter Wahrscheinlichkeit ausgewählt. Die App greift im Betrieb nicht auf das Internet zu.

## Version 1.1

- Caller antworten erst nach dem vollständigen CQ plus realistischer Reaktionspause.
- QRN wurde weich ein- und ausgeblendet; harte Knackimpulse wurden entfernt.

## Version 1.2

- Eigenes Sendesignal hat eine konstante Lautstärke und kein QSB.
- Antwortende Stationen erhalten deutlich größere Tonhöhenabstände und 140 Hz Default-Streuung.
- Neue Defaults: DG8WA, F74 und 600 Hz.
- F1 bis F11 können bei bestehender Verbindung direkt über WinKeyer gesendet werden. F1 ruft CQ, F4 gibt das eigene Rufzeichen und F5 das aufgenommene Rufzeichen.
- WinKeyer-Echo/Paddle-Eingabe unterstützt CALL, DOK, Backspace und Enter.

## Version 1.3

- English user interface with visible version, DG8WA attribution and "Created with AI Support".
- Browser and mobile detection with touch-friendly responsive controls.
- DARC Contest Hub import through direct fetch where CORS permits, plus reliable saved-HTML fallback.
- F7 and partial-call handling: three or more entered characters repeat all matching active callers; fuzzy matching accepts up to two differing characters.

## Version 1.4

- Correct WAG RUN QSO state machine: CQ, callers send callsigns only, operator sends selected call + 5NN + F74, caller returns 5NN + DOK, QSO is logged, TU follows, then CQ resumes.
- RST sent and received are explicitly recorded as 599 in the log.
- DARC import simplified to Open Archive, save the selected results page as HTML, then Import Saved HTML. Direct loading was removed because browser CORS rules make it unreliable.

## Version 1.5

- Cut numbers are applied only to purely numeric serial-number exchanges, never to DOKs that start with a letter.
- Partial calls are recognized from two entered characters.
- F7 repeats matching callers, F8 repeats all calls or the selected station's full exchange, and F9 makes the selected station repeat only its DOK.
- N1MM-style ESM added: Enter sends CQ, then the exchange, then logs and sends TU according to QSO state. The next function-key action is highlighted.
- The equals key repeats the last locally sent message.

## Version 1.6

- QSB is slower and smoother, with one continuous 8 to 16 second fading cycle per received transmission.
- CQ scheduling is guarded so callers cannot begin until the complete CQ and reaction pause have ended; repeated CQ timers cancel older pending responses.
- After a logged QSO, F3 sends the shorter `TU DG8WA TEST`, then callers respond without another full CQ.
- F8 and F9 now transmit `AGN` or `DOK?` first; the selected station answers only after that transmission ends.
- WinKeyer settings moved into a compact modal dialog.
- Built-in Help dialog added.
- UI language selector added for English, German, French and Spanish, with the selection stored locally.

## Version 1.7

- WinKeyer can be explicitly enabled, disconnected, and disabled; disabling closes the serial port.
- White noise fades out over 1.5 seconds when training stops and fades back in when restarted.
- Translation now walks every visible text node; help content is expanded and language-aware.
- Incorrect calls are never auto-corrected. A close match answers with its correct call twice, followed by RST and DOK, so the operator can correct CALL manually.
- After a valid QSO, CALL is cleared, focused and visually highlighted.

## Version 1.8

- Function keys moved above the CALL and DOK entry row.
- A prominent result panel shows correct or incorrect copying immediately after logging.
- The DARC Contest Hub panel moved below the contest log.
- Incomplete calls trigger callsign-only repetition and keep focus in CALL without changing user input.
- Incorrect calls remain untouched, receive a two-times callsign correction over CW, and keep focus in CALL.
- Operational hints no longer reveal callsigns or DOKs before the QSO is logged; explicit Reveal remains user-controlled.

## Version 1.9

- A partial call now transmits only the characters entered by the operator, for example `DL8`, without RST or DOK. The matching station answers with its callsign only, and focus stays in CALL.
- Added Debug mode to show or hide the operational hint panel. It is off by default and stored locally. The post-log result panel remains visible.
- Help is now a prominent yellow `? Help` button in the header.

## Version 2.0

- Debug hints immediately show whether ESM is enabled or disabled when the ESM checkbox changes.
- An incorrect logged QSO no longer sends NIL; the trainer starts a fresh full CQ call instead.
- Help button styling is now visible but restrained.
- Added optional F74 mode. When enabled, approximately 65 percent of caller selections come from calls associated with DOK F74, while the remainder still uses the complete database.

## Version 2.1

- First QRN event is delayed and white noise fades into its steady level, eliminating the short fading burst at the initial CQ.
- Optional PHP/SQLite leaderboard added. See `SERVER-LEADERBOARD.md`.

## Version 2.2

- F8 sends AGN fully before callers answer.
- Defaults: 2 minutes, Cut Numbers on, all band conditions at zero.
- QSB and QRM are sliders.
- Multiple HTML imports accumulate unique calls and show the rising total.

## Version 2.2.1

- Hotfix: removed references to the obsolete `fade`, `qsb`, and `qrm` controls that prevented contest startup after the controls were converted to sliders.
- Startup now reads `qsbLevel` and `qrmLevel` correctly and updates band audio safely.

## Version 2.3

- Band Conditions now use a full-width responsive auto-fit grid.
- Caller pitch spread is a numeric field like the WPM inputs.
- Every completed QSO sends `TU DG8WA TEST`, even when CALL or DOK was copied incorrectly.
- Changelog is available from a header button and modal dialog.

## Version 2.4

- Added 1,600 generated international training calls across a wide range of prefixes.
- International callers send `5NN` plus a random serial number from `001` to `2500`, generated anew for each QSO.
- Added `German only calls`; when enabled, only the German and imported Call/DOK database is used.
- In mixed mode, approximately 28 percent of callers are international. F74 mode remains German-focused.

## Version 2.4.1

- All operational CW messages use the value from `My callsign`. For example, if `DL1ABC` is entered, CQ, F3, F4, TU and TEST messages use `DL1ABC` rather than the default call.

## Version 2.4.2

- Hotfix: restored initialization of the international caller pool and German/DX station metadata. A missing `foreignPool` variable had stopped JavaScript initialization, so the database count and caller responses were unavailable.
- Added a defensive fallback and explicit DE/DX database count in the status badge.

## Version 2.4.3

- DARC HTML import recognizes both German calls with DOK and international calls. Imported DX calls are stored cumulatively in `wagImportedDxCalls` and receive a random serial from 001 to 2500 during training.
- Cut Numbers are applied when a DX station sends or repeats a numeric serial exchange. DOKs remain unchanged.
- Any matched input of at least two characters that is shorter than the actual callsign is treated as a partial call. The station repeats only its callsign; no RST or exchange is sent.

## Version 2.5

- Active caller count is visible in the operating information area.
- A full F1 CQ creates a new pileup at the configured size. After each QSO, the worked station leaves and occasionally one additional waiting station loses interest. `TU [MY CALL] TEST` only recalls surviving stations and does not refill the pileup.
- Repeat and partial-call attempts increase a per-station miss counter. From the third unsuccessful attempt onward, a station has an increasing chance of giving up and leaving the pileup.
- When no callers remain, the operator must send a new full CQ with F1.

## Version 2.5.1

- Hotfix: replaced the CQ response scheduling block with a dedicated guarded scheduler. After the complete F1 CQ and a reaction pause, `spawn()` reliably creates the configured pileup.
- Caller count is now a small pill directly above CALL and displays `Callers: n`.

## Version 2.5.2

- When the timer reaches zero, a prominent time-up banner, highlighted clock, and status badge clearly indicate that training has finished. Manual Stop remains visually quieter.
- Debug mode is enabled by default. On the version 2.5.2 migration it is switched on once, but users can still turn it off afterwards.

## Version 2.6

- Added an adjustable `DX caller share` setting with 15, 30, 45, 60 and 75 percent choices. Default is 45 percent. `German only calls` overrides the ratio.
- When session time expires, the end message is written to both the large time-up banner and the Debug hint panel. The Debug message remains visible until the next session or another explicit operation updates it.

## Version 3.0 Final Release

- First final release. All versions before 3.0 are classified as Beta.
- Changelog button removed. Click the version number beside the title or in the header badge to open the changelog.
- The time-up state is now locked. The large notice remains until clicked. Enter, F-keys, ESM and Start Contest cannot begin a new contest while the notice is active.

## Version 3.0.1

- Neuer Name: **DG8Wa(stl's) WAG/CWA cw test trainer**, mit kleiner Kennzeichnung `by DG8WA` und kleiner, anklickbarer Versionsnummer.
- Vollständiger CQ-Ruf verkürzt auf `CQ WAG [MY CALL] TEST`.
- Einheitliche Funktion `exchangeForAir()` stellt sicher, dass Cut Numbers bei allen numerischen DX-Aussendungen angewendet werden.
- Pileup-Abbau: Normal mit halbierter Zusatzabgangswahrscheinlichkeit, WAG rush hour mit nochmals halbierter Abgangswahrscheinlichkeit und höherer Geduld bei Wiederholungen.
- Neuer Filter `DX only calls`, gegenseitig verriegelt mit `German only calls`; F74-Modus deaktiviert DX-only automatisch.

## Version 3.0.2

- Versionsschema ohne den Zusatz `Final`; zukünftige Versionen werden normal numerisch weitergezählt.
- Die Bereiche Station & Session, Band Conditions, Contest Log, DARC Import und Session Summary können einzeln minimiert werden. Der Zustand bleibt im Browser gespeichert.
- `Auto Hide` minimiert Station & Session und Band Conditions automatisch, sobald F1 beziehungsweise der automatische CQ-Ruf beginnt.
- `Compact Layout` reduziert Abstände, Höhen und Schriftgrößen und verwendet kürzere Beschriftungen, ohne Funktionen auszublenden.

## Version 3.0.3

- `api/.htaccess` wieder ergänzt; `api/data/.htaccess` schützt das Datenverzeichnis zusätzlich.
- `SERVER-LEADERBOARD.md` mit Installations-, Rechte-, Apache- und nginx-Hinweisen hinzugefügt.

## Version 3.0.4

- Neue erste Kachel `Settings` für Compact Layout, Auto Hide, Cut Numbers einschließlich Format, ESM, DL-/DX-Rufzeichenwahl, F74 und Debug.
- Settings, Station & Session sowie Band Conditions sind beim ersten Aufruf minimiert. Ein vom Benutzer gespeicherter geöffneter Zustand bleibt erhalten.
- Statistik-Kacheln auf Smartphones sind in der kompakten Ansicht schmaler, mit kleineren Abständen und kleinerer Typografie.

## Version 3.0.5

- Die Kachel `Settings` nutzt jetzt die volle Inhaltsbreite.
- Der zentrale Bedienbereich heißt jetzt `Logging`.
- Neue Reihenfolge: Session-Buttons, Statistik, aktive Caller, CALL und DOK/Serial mit Log QSO, Ergebnisinfo, Debug-Fenster und abschließend die F-Tasten.

## Version 3.0.6

- Minimierbare Kacheln reagieren jetzt auf einen Klick auf die gesamte Überschrift sowie per Enter/Leertaste bei Tastaturfokus.
- Neue Farbschemata: System, Dark, Gray, Matrix, White und Forest. `System` folgt automatisch der Hell-/Dunkel-Einstellung des Betriebssystems.
- Das gewählte Farbschema wird im Browser gespeichert.

## Version 3.0.7
- Sprachumschaltung, Hilfe, WinKeyer und Leaderboard sind in einer gemeinsamen Kachel in der Titelleiste gruppiert.
- Die Logging-Aktionsbuttons werden auf Smartphones kompakt über zwei Zeilen verteilt.
- CALL und DOK/Serial bleiben auch auf schmalen Displays innerhalb der Logging-Kachel.

## Version 3.0.8
- ZIP-Struktur korrigiert.
- `leaderboard.php` liegt wieder unter `api/leaderboard.php`.
- `.htaccess`-Schutzdateien für `api/` und `api/data/` wieder enthalten.

## Version 3.0.9
- Cache-Busting für CSS und JavaScript ergänzt.
- Titelleisten-Kachel und zweizeilige mobile Logging-Buttons verstärkt.
- Harte Breitenbegrenzung für CALL und DOK/Serial auf Smartphones.

## Version 3.0.10
- Sprachumschaltung, Hilfe, WinKeyer und Leaderboard aus der Titelleiste entfernt.
- Neue minimierbare Kachel `Tools` am unteren Ende der Anwendung.
- Die Tools-Kachel unterstützt Überschriften-Klick, `+`/`−`, Farbschemata und Compact Layout wie die übrigen Kacheln.

## Version 3.0.11
- Tools-Kachel nach oben vor Settings verschoben.
- Settings und Tools auf Smartphones auf die exakt gleiche Kachelbreite wie alle anderen Bereiche normiert.
- Callers als sechste Statistik-Kachel in Logging integriert.
- Smartphone-Statistik zeigt TIME LEFT, QSOs, RATE/h, ACCURACY, MULTIPLIERS und CALLERS zweizeilig in einem 3×2-Raster.

## Version 3.0.12
- Alle Kacheln verwenden jetzt dieselbe volle Breite, Innenabstände, Rahmen, Rundungen und Überschriftenstruktur wie Tools und Settings.
- Logging ist nun ebenfalls minimierbar und speichert seinen Zustand.
- Mobile Breiten und Abstände wurden für alle Bereiche vereinheitlicht.

## Version 3.0.13
- Sendesperre: ESM und F-Tasten können keine überlappenden eigenen Aussendungen mehr erzeugen.
- Jede eigene Aussendung stoppt sofort alle gerade rufenden Stationen; deren bereits geplante Audioereignisse werden verworfen.
- Partial Calls werden als Fragment an jeder Position erkannt, z. B. `DG`, `8W` oder `WA` für `DG8WA`.

## Version 3.0.14
- Einstellbare minimale und maximale Antwortzeit der Stationen in Millisekunden.
- Gilt für Antworten nach CQ, TU/TEST, Partial Calls, F7, AGN sowie DOK?/NR?.
- Standard: 350 bis 1200 ms; bei vertauschten Werten werden Min und Max beim Start automatisch korrigiert.

## Version 3.0.15
- Leuchtender Rahmen von CALL und DOK/Serial wird nicht mehr links oder unten abgeschnitten.
- Fokus- und Aktiv-Markierung verwenden einen sichtbaren Innenrahmen mit leichtem Glow.
- Overflow-Clipping der Logging-Eingabe wurde entfernt, die Smartphone-Breitenbegrenzung bleibt bestehen.

## Version 3.1.0
- Tools und Settings zu `Tools & Settings` zusammengeführt und in vier übersichtliche Gruppen gegliedert.
- Cut Numbers vollständig in die Auswahlliste integriert; `Off` ersetzt die separate Checkbox.
- `+`/`−`-Schaltflächen entfernt; Kacheln werden ausschließlich über ihre Überschrift geöffnet und geschlossen.
- Kacheln lassen sich per Drag & Drop sortieren, auf Touch-Geräten über den Griff `⋮⋮` und per Tastatur mit Pfeil hoch/runter. Die Reihenfolge wird gespeichert.
- Neue Standardreihenfolge: Logging, Contest Log, Station & Session, Band Conditions, Tools & Settings, DARC Import, Session Summary.

### Version 3.2.0
- Neuer Battle Score mit Gewichtung von QSOs, Genauigkeit, Laufzeit, Gegengeschwindigkeit, Pileup, Bandbedingungen, Response Time und DX-Anteil.
- Leaderboard zeigt Style, QSOs, Accuracy, Average WPM, Pileup, Minuten und Difficulty.
- Umschalter WAG Style / CWA Style im Header. CWA-Preset: 80 m, 12–22 WPM, 18 WPM CQ, 60 Minuten und anfängerfreundliche Einstellungen.
- CWA folgt dem Prinzip des DARC CW Ausbildungscontests: RST+DOK, DX-Stationen mit laufender Nummer.

### Version 3.2.1
- Header-Button für Datenschutz, lokale Speicherung und Quellen.
- Hinweisbanner für ausschließlich technisch notwendige lokale Browserspeicherung; kein Werbe- oder Analyse-Tracking enthalten.
- Quellenangaben für DARC WAG, DARC CWA, WAG-Ergebnisarchiv, BfDI, DSGVO und ePrivacy-Richtlinie.
- Dezente Emojis in Kacheln und Bedienaktionen.
- Nach Zeitablauf zeigt der Abschlussbanner Battle Score, QSOs, Accuracy, Average WPM, Difficulty und Rate.

**Vor öffentlicher Bereitstellung:** Verantwortliche Stelle, Kontakt, Hostinganbieter, Serverstandort, Logfile-Aufbewahrung und Löschkontakt in der Privacy-Information ergänzen.

### Version 3.2.2
- Speicher-/Cookie-Hinweis kann nun über `OK` oder `×` geschlossen werden.
- Script-Laden auf `defer` umgestellt; dadurch wird die komplette Seite vor Initialisierung geparst und die Browsererkennung bleibt nicht mehr auf `Detecting Browser`.
- Datenschutzangaben ergänzt: Sebastian Jeuck / DG8WA, 65620 Waldbrunn, abusegame@kabanet.de, self-hosted.
- `Delete my call from Leaderboard` mit privatem, browsergebundenem Löschschlüssel ergänzt. Löschbar sind aus Sicherheitsgründen nur Ergebnisse, die ab v3.2.2 aus demselben Browser eingereicht wurden.
- Im Compact Layout werden dekorative Interface-Emojis ausgeblendet.

### Version 3.2.3
- Neuer Name: `DG8Wa(stl's) WAG/CWA cw test trainer`.
- Datenschutzdialog, Speicherhinweis und Datenschutzaktionen folgen jetzt Deutsch, Englisch, Französisch und Spanisch.
- Allgemeine Übersetzungen für Header, Style-Schalter, Battle-Leaderboard und aktuelle Bedienfelder erweitert.
- WAG/CWA-Schalter, Hilfe und Datenschutz sind im Header als hervorgehobene Primärgruppe gestaltet.

### Version 3.2.4
- `Submit to Leaderboard` direkt im TIME-IS-UP-Ergebnisfeld; der Button schließt den Hinweis nicht.
- QSB-Zyklus auf 2,2 bis 4,6 Sekunden verkürzt. Bis einschließlich 50% QSB bleibt ein Mindestpegel von mindestens 42% erhalten.
- DX-Seriennummern: 92% im Bereich 001–999, 8% im Bereich 1000–1999. Cut Numbers werden weiterhin erst bei der CW-Ausgabe angewendet; geloggt wird die Originalnummer.
- 17 deutsche Rufzeichen/DOK-Kombinationen ergänzt beziehungsweise aktualisiert.


#### Version 3.3.0
- Leaderboard without callsign aggregation: every row is an individual submission.
- Default TOP 10 can be expanded to 25, 50 or 100 entries.
- Clicking a callsign shows all submissions for that station.
- The API calculates an internal per-call TOP 10 and overall TOP 50 rank.


#### Version 3.4.0
- Geschützter `/admin/`-Bereich für DG8WA oder DF9ZV.
- Ersteinrichtung mit lokal gespeicherter Passwort-Hashdatei.
- Leaderboard-Moderation, DSGVO-Löschung, Audit-Log und manifestbasierte Patchinstallation.
- Siehe `ADMIN-README.md`.


#### Version 3.4.1
- Admin-Anmeldung zweistufig: Call zuerst, Kennwort danach.
- Neues Kennwort nur bei nicht vorhandener Admin-Hashdatei.
- CSRF-/Session-Pfad für Unterverzeichnisinstallationen korrigiert.


#### Version 3.5.0
- Admin-Link im Footer mit neuem Fenster.
- Kompakteres Leaderboard, insbesondere auf Smartphones.
- Admin-Backend vollständig DE/EN umschaltbar.
- Patchsystem für kontrollierte Admin-UI-Dateien erweitert.
- Code- und Formatierungsprüfung durchgeführt.


#### Version 3.5.1
- Admin-Panel-Patchworkflow mit Manifest und SHA-256-Prüfsummen validiert.

#### Version 3.6.0
- Projektcode und originale Dokumentation unter MIT-Lizenz veröffentlicht.
- GitHub-Repository öffentlich verlinkt.
- Open-Source-Dateien für Beiträge, Sicherheit, Verhalten, Hinweise und Zitation ergänzt.
