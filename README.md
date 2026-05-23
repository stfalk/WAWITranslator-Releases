# WAWITranslator – Downloads

Öffentlicher Download-Spiegel für [**WAWITranslator**](https://falkcons.de/wawitranslator/) von [**FALKConS GmbH**](https://falkcons.de/).

> ℹ️ Der Quellcode ist proprietär und liegt in privaten Repositories.
> Dieses Repo enthält **ausschließlich die ausführbaren Installer-Setups** und Begleit-Dokumentation, damit der Download stabile, öffentliche URLs hat.

## Aktuelles Release herunterladen

Stabile URL (zeigt immer auf das jüngste Release):

```
https://github.com/stfalk/WAWITranslator-Releases/releases/latest/download/WAWITranslator-Setup.exe
```

Oder die [Releases-Seite](https://github.com/stfalk/WAWITranslator-Releases/releases) öffnen und die gewünschte Version wählen.

## Was ist WAWITranslator?

Übersetzt JTL-Wawi-Produktdaten (Artikel, Kategorien, Merkmale, Medien …) automatisch per DeepL – im Batch und zeitgesteuert.

- Trial: voller Funktionsumfang testen, 1 Datensatz pro Lauf
- Pro: unbegrenzte Läufe, zeitgesteuerter Scheduler, E-Mail-Benachrichtigungen

Mehr Infos und Bestellung: <https://falkcons.de/wawitranslator/>

## Support

Fragen oder Probleme? <https://falkcons.de/support/>

---

## Maintainer-Hinweis: Kein Source-Code hier!

Dieses Repo ist öffentlich. Damit **niemals** versehentlich Quellcode aus den privaten Source-Repos hier landet, gibt es mehrere Schutzschichten:

1. **`.gitignore`** – blockt typische Source-Extensions (`*.cs`, `*.go`, `*.xaml`, `*.sln`, …) schon beim `git add`.
2. **GitHub Action `source-code-guard`** ([.github/workflows/no-source-code.yml](.github/workflows/no-source-code.yml)) – läuft bei jedem Push, scannt alle Tracked-Files und schlägt rot fehl, falls eine verbotene Extension auftaucht.
3. **Branch Protection auf `main`** (empfohlen, manuell im GitHub-Settings-UI zu aktivieren):
   - Settings → Branches → Add rule für `main`
   - ☑ „Require status checks to pass before merging"
   - Required check: `guard` (aus dem source-code-guard-Workflow)
   - ☐ „Include administrators" **deaktivieren**, damit Releases via Maintainer-Push trotzdem durchgehen, falls der Check mal hängt
4. **Release-Workflow:** Setup-EXE über `gh release create v<version> ...` oder das Releases-UI hochladen. Nie über `git push` von Binaries (besser im Workspace gar nicht erst checken).

Wenn versehentlich doch Source landet: lokal `git rm --cached <pfad>` + commit + push. Bei sensiblem Inhalt (API-Keys etc.) zusätzlich `git filter-repo` für History-Bereinigung und Rotation der Secrets.
