# Sicherheit & Schwachstellen-Meldung

## Eine Schwachstelle melden

Wenn du eine Sicherheitslücke in WAWITranslator findest, melde sie bitte
**vertraulich** an:

**support@falkcons.de**

Bitte **keine** öffentlichen GitHub-Issues für Sicherheitsthemen anlegen
(Coordinated Vulnerability Disclosure).

Hilfreiche Angaben:
- Betroffene Komponente/Version (CLI `WTS.exe`, GUI `WAWITranslator.exe`,
  Installer)
- Beschreibung und – wenn möglich – Reproduktionsschritte
- Mögliche Auswirkung

## Ablauf

1. **Eingangsbestätigung** in der Regel innerhalb von 3 Werktagen.
2. **Triage & Bewertung** des Schweregrads.
3. **Fix** und – bei aktiv ausgenutzten Schwachstellen – zeitnahe
   Veröffentlichung einer neuen, signierten Version. Die Oberfläche weist
   beim nächsten Start auf die neue Version hin, sofern die Update-Prüfung
   eingeschaltet ist.
4. **Koordinierte Offenlegung** nach Bereitstellung des Fixes.

## Unterstützte Versionen

Sicherheitsupdates werden für die **jeweils aktuelle** Release-Version
bereitgestellt.

## Produkt-Sicherheitsmerkmale (Kurzüberblick)

- **Signierte Auslieferung:** Setup, Deinstallationsprogramm sowie
  `WTS.exe`, `wts-cli.exe`, Setup-Helfer und `WAWITranslator.exe` sind mit
  Authenticode signiert (Herausgeber FALKConS GmbH, Schlüssel in Azure Key
  Vault, mit Zeitstempel). Signiert wird nur ein Stand, dessen CI für den
  gebauten CLI- und GUI-Commit grün ist.
- **Update:** Die Oberfläche fragt beim Start bei GitHub nach einer neueren
  Version; das lässt sich unter Einstellungen → Lizenz („Beim Start nach
  Updates suchen“) abschalten. Auf Klick lädt sie das Setup – nur von der
  Release-Seite des Projekts bei GitHub, die Adresse wird geprüft – in den
  temporären Ordner. Vor dem Start prüft sie die Authenticode-Signatur samt
  Sperrlisten der Zertifikatskette: Gestartet wird nur ein Setup, das gültig
  von FALKConS GmbH signiert ist. Ist die Signatur ungültig oder die
  Sperrprüfung nicht möglich, startet das Setup nicht.
- **E-Mail über den eigenen Mailserver:** verschlüsselt über Port 465 oder
  STARTTLS. Bietet der Server STARTTLS trotz Einstellung nicht an, wird
  nicht gesendet; ein Passwort geht nie unverschlüsselt hinaus. Die Test-Mail
  aus den Einstellungen nimmt denselben Weg.
- **Zugangsdaten** (SQL, DeepL/OpenAI, Lizenzschlüssel, SMTP-Passwort,
  Brevo-API-Schlüssel) werden lokal in der `config.json` unter
  `%ProgramData%\WAWITranslator` gespeichert und dort **mit Windows DPAPI
  verschlüsselt** (Token `enc:…`, Scope `LOCAL_MACHINE`); API-Schlüssel
  erscheinen **nicht** in Log-Dateien. Die Verschlüsselung ist an die Maschine
  gebunden: eine mitkopierte oder gesicherte `config.json` ist auf einem
  anderen Rechner unbrauchbar. Bestehende Klartext-Configs werden beim nächsten
  Update bzw. beim nächsten Speichern in der GUI automatisch umgestellt.
  **Grenze:** Wer auf demselben Rechner Lesezugriff hat, kann die Werte
  entschlüsseln — der Schutz zielt auf Weitergabe/Backups, nicht auf einen
  Angreifer mit lokalem Zugriff.
- **Verbindungen ins Internet** laufen über HTTPS. Die Antworten des
  Lizenzservers sind Ed25519-signiert und werden vor der Verwendung geprüft
  (Signatur, Zufallswert, Installations-Fingerprint, Alter). Den
  OpenAI-Schlüssel sendet die Software nur an `https://`-Adressen bzw. per
  `http` nur an diesen Rechner; bei einer ungültigen `openai.baseUrl` bricht
  ein Lauf ab, statt eine andere Adresse zu nehmen. Bei Brevo und der
  OpenAI-Modellliste folgt die Software keinen Weiterleitungen.
- **Abhängigkeiten** werden per `govulncheck` (Go-Module) und
  `dotnet list package --vulnerable` (NuGet) geprüft – in der CI und vor
  jedem signierten Bau erneut.
- **Datenflüsse** beschreibt das Dokument „Datenschutz-Hinweise zum
  WAWITranslator“ (auf Anfrage über support@falkcons.de).

## Bekannte Grenzen

- **Datenbankverbindung ohne gültiges Server-Zertifikat:** Hatte der SQL
  Server beim Setup kein gültiges Zertifikat, ist die Verbindung
  verschlüsselt, das Zertifikat wird aber nicht geprüft. Das schützt gegen
  Mitlesen, nicht gegen einen vorgetäuschten Server im eigenen Netz.
  Abhilfe: dem SQL Server ein gültiges Zertifikat geben und das Setup erneut
  ausführen – dann wird das Zertifikat geprüft.
- **Programmordner:** Angemeldete Benutzer dürfen in
  `%ProgramData%\WAWITranslator` Dateien und Unterordner anlegen (nicht die
  Programmdateien ändern); die `config.json` ist für sie änderbar.
