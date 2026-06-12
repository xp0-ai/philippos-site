# philippos-site

Produkt-Landingpage **PHILIPPOS** — live unter <https://philippos.xp0.ai/>.

Statische Single-Page-Site (eine `index.html`, keine Build-Schritte, keine Abhängigkeiten außer den lokal gehosteten Schriften).

## Struktur

```
public/                # ← die deploybare Website (das ist der Webroot)
  index.html           # die Seite (HTML + CSS + JS inline)
  impressum.html
  datenschutz.html
  fonts/               # Fraunces + Inter, lokal gehostet (DSGVO)
.github/workflows/
  deploy.yml           # Auto-Deploy bei Push auf main
```

## Deploy — automatisch

**Jeder Push auf `main` geht sofort live.** Der Webroot auf dem Server wird zum
exakten Git-Stand von `public/` gemacht (`rsync --delete`). Es gibt keinen
manuellen Schritt mehr.

Ablauf:
1. `public/index.html` (oder andere Dateien) bearbeiten.
2. Commit + Push auf `main`.
3. GitHub Actions (`deploy.yml`) kopiert `public/` per SSH auf den Prod-Server
   und prüft danach, dass <https://philippos.xp0.ai/> mit HTTP 200 antwortet.

### Wie der Deploy technisch abgesichert ist
- Ein **dedizierter SSH-Deploy-Key** (GitHub-Secret `PHILIPPOS_DEPLOY_KEY`) ist
  serverseitig per **forced command** auf das Skript
  `/usr/local/bin/philippos-deploy.sh` beschränkt. Der Key kann ausschließlich
  die Website deployen — keine Shell, kein anderer Befehl, kein Root-Zugriff.
- Der Zielhost steht im Secret `PHILIPPOS_DEPLOY_HOST`.

## Lokal ansehen

`public/` ist eine reine statische Site — einfach `public/index.html` im Browser
öffnen oder `python3 -m http.server` aus dem `public/`-Ordner.

## Regeln für die Inhalte
- Keine echten Kunden-/Mitarbeiterdaten (alle Darstellungen schematisch).
- Impressum-/Datenschutz-Links nie entfernen.
- Schriften bleiben lokal gehostet (keine Einbindung externer Fonts — DSGVO).
