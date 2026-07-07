# KVB-Analytik

Browser-Dashboard für die quartalsweisen KVB-PDFs der Gemeinschaftspraxis Gollierplatz & Romanplatz:

- **AM-Trendmeldung WSV** (Modell-Trendmeldung Wirtschaftliche Verordnungsweise 3.0, KVB Bayern)
- **Häufigkeitsstatistik** (HW0021 GOP-Detail + HW0024 Gesamtübersicht der KVB-Prüfungsstelle)

PDF wird lokal im Browser gelesen (pdf.js), es geht nichts an einen Server. Auswertungen werden im localStorage gespeichert und – optional per GitHub-PAT – zusätzlich in `data.json` in diesem Repo abgelegt, damit sie über Notion für das Team sichtbar sind.

## Setup (einmalig)

### 1. Repo anlegen

1. Auf GitHub eingeloggt: **New repository**
2. Name: `kvb-analytik`
3. Sichtbarkeit: **Private** (Praxisdaten!)
4. „Add a README" **nicht** ankreuzen
5. Create repository

### 2. Dateien hochladen

Diese drei Dateien in das Repo pushen:
- `index.html`
- `data.json` (leer, mit `{"wsv":[],"hf":[]}`)
- `README.md`

Per Web-UI: „uploading an existing file" → alle drei per Drag & Drop.

### 3. GitHub Pages aktivieren

1. Repo → **Settings** → **Pages**
2. Source: „Deploy from a branch" → `main` / root
3. Save

Nach ~1 Minute ist die Site erreichbar unter:
`https://shnsn.github.io/kvb-analytik/`

### 4. Personal Access Token (PAT) erstellen

Für Schreibzugriff (Speichern auf GitHub, damit das Team die Daten sieht):

1. GitHub → **Settings → Developer settings → Personal access tokens → Fine-grained tokens**
2. **Generate new token**
3. Repository access: **Only select repositories** → `shnsn/kvb-analytik`
4. Permissions → Repository permissions → **Contents: Read and write**
5. Expiration: 1 Jahr (wird jährlich erneuert)
6. Token kopieren (`ghp_…`) — wird nur einmal angezeigt!

Im Dashboard oben rechts auf **⚙ GitHub** klicken → Token einfügen → **Speichern & verbinden**.

Der PAT wird ausschließlich im Browser gespeichert (localStorage). Ohne PAT läuft das Dashboard rein lokal (jeder sieht nur seine eigenen Daten).

### 5. In Notion einbetten

Für die Mitarbeiter-Ansicht:

1. In Notion die gewünschte Seite öffnen
2. `/embed` eingeben und Enter drücken
3. URL: `https://shnsn.github.io/kvb-analytik/?readonly`

Der Parameter `?readonly` blendet die Upload-Zonen und den GitHub-Button aus. Die Mitarbeiter sehen nur die Auswertung.

## Nutzung

### Häufigkeitsstatistik hochladen

Beide PDFs (HW0021 + HW0024) desselben Quartals gemeinsam ins Drop-Feld ziehen. Das Dashboard erkennt den Typ automatisch, zeigt eine Vorschau mit Stichprobe zur Plausibilitätskontrolle, dann per Klick übernehmen.

### WSV-Trendmeldung hochladen

Das WSV-3.0-PDF der KVB Bayern (Format „Modell-Trendmeldung") ins Drop-Feld ziehen. Auswertung wird generiert.

### Datenkorrektheit

Nach jedem Upload zeigt das Dashboard eine **Vorschau** vor dem Speichern. Bei ungewöhnlich niedriger GOP-Zahl (<30 bei HW0021) wird gewarnt. Wenn ein Wert falsch aussieht, im Chat Bescheid geben — der Parser lässt sich anpassen.

## Farben & Design

Das Dashboard nutzt dieselbe warm-lila Praxispalette wie die Schwester-Dashboards
[Vorhaltepauschale](https://shnsn.github.io/vorhaltepauschale/) und
[Fallzahl-Prediktion](https://shnsn.github.io/fallzahl-dashboard/):

- `--praxis` `#924E7D` (Praxis-Purpur)
- `--primary-dark` `#6B3860`
- `--primary-bg` `#EDD8E8`

## Datenschutz

- PDFs werden ausschließlich im Browser gelesen (pdf.js im Client)
- Auswertungen liegen in `localStorage` und optional in `data.json` im **privaten** GitHub-Repo
- Der PAT wird nur im lokalen Browser gespeichert (`localStorage`), nicht in URLs oder auf GitHub
- Kein Server, kein Tracking, kein Analytics

## Bekannte Grenzen

- Die WSV-Klartext-Zusammenfassung ist eine maschinelle Auswertung, keine Pharmakotherapieberatung. Für konkrete Empfehlungen: [kvb.de/mitglieder/beratung](https://www.kvb.de/mitglieder/beratung)
- Die Ampel-Heuristik der WSV-Auswertung (grün/gelb/rot) berücksichtigt Wirtschaftlichkeitsfaktoren nur näherungsweise — die echte KVB-Bewertung kann leicht abweichen
- HW0021/HW0024-Parser wurden gegen das Layout Quartal 3/2025 entwickelt; bei KVB-Layoutänderungen ggf. anpassen

## Verwandte Ressourcen

- KVB Wirkstoffvereinbarung: [kvb.de/verordnungen/arzneimittel](https://www.kvb.de/verordnungen/arzneimittel)
- KVB Prüfungsstelle: [aerzte-bayern.de/pruefungsstelle](https://www.aerzte-bayern.de/pruefungsstelle)
- pdf.js (Client-side PDF-Extraktion): [mozilla/pdf.js](https://github.com/mozilla/pdf.js)
- Chart.js (Visualisierungen): [chartjs.org](https://www.chartjs.org/)
