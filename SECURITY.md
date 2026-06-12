# Sicherheit & Schwachstellen-Meldung

## Eine Schwachstelle melden

Wenn du eine SicherheitslÃ¼cke in WAWITranslator findest, melde sie bitte
**vertraulich** an:

**security@falkcons.de**

Bitte **keine** Ã¶ffentlichen GitHub-Issues fÃ¼r Sicherheitsthemen anlegen
(Coordinated Vulnerability Disclosure).

Hilfreiche Angaben:
- Betroffene Komponente/Version (CLI `WTS.exe`, GUI `WAWITranslator.exe`,
  Installer)
- Beschreibung und â€“ wenn mÃ¶glich â€“ Reproduktionsschritte
- MÃ¶gliche Auswirkung

## Ablauf

1. **EingangsbestÃ¤tigung** in der Regel innerhalb von 3 Werktagen.
2. **Triage & Bewertung** des Schweregrads.
3. **Fix** und â€“ bei aktiv ausgenutzten Schwachstellen â€“ zeitnahe
   Auslieferung Ã¼ber den integrierten Update-Mechanismus (signierte Releases).
4. **Koordinierte Offenlegung** nach Bereitstellung des Fixes.

## UnterstÃ¼tzte Versionen

Sicherheitsupdates werden fÃ¼r die **jeweils aktuelle** Release-Version
bereitgestellt.

## Produkt-Sicherheitsmerkmale (KurzÃ¼berblick)

- Auslieferung Ã¼ber **signierte** Installer (Authenticode, Azure Key Vault).
- Integrierter, signaturgeprÃ¼fter **Auto-Update**-Mechanismus.
- PasswÃ¶rter werden **nicht** in Dateien gespeichert (Windows Credential Vault).
- AbhÃ¤ngigkeiten werden per `govulncheck` / NuGet-Scan Ã¼berwacht.
- DatenflÃ¼sse sind in `docs/DATENSCHUTZ-DATENFLUSS.md` dokumentiert.
