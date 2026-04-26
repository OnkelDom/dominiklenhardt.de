# dominiklenhardt.de

Statische Website fuer `dominiklenhardt.de`.

Die Seite wird ohne Jekyll, Node.js oder sonstigen Build-Schritt betrieben. Das Repository enthaelt die produktiven HTML-, CSS- und Asset-Dateien direkt im Webroot. Dadurch ist der Betrieb bewusst einfach: Aenderung committen, nach `main` pushen, GitHub Actions synchronisiert den Stand per SFTP auf den Hetzner-Webspace.

## Architektur

- Quelle: GitHub Repository `OnkelDom/dominiklenhardt.de`
- Zielsystem: Hetzner Webspace per SFTP
- Zielpfad: Domain-Ordner `dominiklenhardt.de`
- Deployment: GitHub Actions Workflow `.github/workflows/deploy.yml`
- Auslieferung: statische Dateien, kein Server-seitiger Build

## Repository-Struktur

```text
.
├── .github/workflows/deploy.yml
├── assets/
│   ├── css/site.css
│   ├── favicon/
│   └── img/portfolio.png
├── datenschutz.html
├── impressum.html
├── index.html
└── README.md
```

## Lokale Bearbeitung

Dateien direkt bearbeiten, zum Beispiel:

```bash
vim index.html
vim assets/css/site.css
```

Lokale Vorschau:

```bash
python3 -m http.server 8080
```

Danach im Browser oeffnen:

```text
http://localhost:8080/
```

## Deployment

Der Deploy laeuft automatisch bei jedem Push auf `main`.

Manueller Start ist ebenfalls moeglich:

```bash
gh workflow run deploy.yml --repo OnkelDom/dominiklenhardt.de --ref main
```

Status pruefen:

```bash
gh run list --repo OnkelDom/dominiklenhardt.de --limit 5
```

## GitHub Secrets

Der Workflow erwartet folgende Repository-Secrets:

```text
HETZNER_HOST
HETZNER_PORT
HETZNER_USER
HETZNER_PASSWORD
HETZNER_TARGET_DIR
```

Die Secrets duerfen nicht im Repository abgelegt werden. Der Zielordner wird ueber `HETZNER_TARGET_DIR` gesteuert und ist fuer diese Seite `dominiklenhardt.de`.

## Betriebsnotizen

- `CNAME` wird nicht mehr verwendet, weil die Seite nicht mehr ueber GitHub Pages ausgeliefert werden soll.
- Jekyll-Konfigurationen, Layouts und Theme-Artefakte wurden entfernt.
- Der Deploy-Workflow spiegelt den lokalen Webroot in den Hetzner-Domainordner und loescht entfernte Dateien dort ebenfalls.
- DNS muss separat auf den Hetzner-Webspace zeigen; solange DNS noch auf GitHub Pages zeigt, ist der erfolgreiche SFTP-Deploy nicht oeffentlich sichtbar.
