# Federated Research Networks — Interactive Map

An interactive map of UK Trusted Research Environments (TREs) and their federation memberships, built as a single self-contained HTML file.

## Live site

Hosted via GitHub Pages at:
`https://<your-username>.github.io/<your-repo-name>/`

---

## What it does

- **Map view** — click any region (England EERs, Scotland, Wales, Northern Ireland constituencies) to see which TREs operate there
- **TRE detail panel** — select a TRE node or a region row to see federation membership, computational patterns, egress inspection model, and interface standard
- **Federation panel** — select a federation filter chip or click a federation tag to see member TREs, governance, and weave patterns
- **Analysis filter** — grouped, collapsible analysis type filter that highlights which TREs support a given analysis
- **Persona switcher** — two entry points: Researcher (analysis-first) and Infrastructure (federation/pattern-first)
- **Cluster nodes** — co-located TREs merge into a single badge; click to fan out into individual spokes

---

## TREs currently mapped

| TRE | Nation | Region | Federation(s) |
|-----|--------|--------|---------------|
| SAIL Databank | Wales | Wales | TETRE, UK-SG, SeRP |
| Dementia Platforms UK | Wales | Wales | SeRP |
| HIC | Scotland | Scotland | TETRE |
| Lancashire Teaching Hospital NHS Trust | England | North West | DataSHIELD |
| NUH Trust | England | East Midlands | UK-SG |
| GOSH | England | London | PHEMS |
| Eastern England SDE | England | Eastern | TETRE |

---

## Federations

| ID | Full name |
|----|-----------|
| TETRE | Trusted Environment & TRE Research |
| UK-SG | UK Secure Gateway |
| DataSHIELD | DataSHIELD |
| SeRP | Secure eResearch Platform |
| PHEMS | Pan-London Hospital Emergency Medical Service |

---

## Hosting on GitHub Pages

### Option 1 — Automatic (recommended)

The included GitHub Actions workflow (`.github/workflows/pages.yml`) deploys automatically on every push to `main`.

1. Push this repository to GitHub
2. Go to **Settings → Pages**
3. Under *Source*, select **GitHub Actions**
4. Push any change — the site deploys in ~30 seconds

### Option 2 — Manual

1. Go to **Settings → Pages**
2. Under *Source*, select **Deploy from a branch**
3. Choose `main` branch, `/ (root)` folder
4. Click **Save**

---

## Adding or editing TREs

All data lives in `index.html` inside the `const ORGS = [...]` array (search for `Organisation nodes`). Each entry follows this shape:

```js
{
  name: 'My TRE',
  lon: -1.23, lat: 52.45,   // WGS84 coordinates
  nation: 'england',         // england | scotland | wales | northern_ireland
  region: 'East Midlands',   // EER name for England, nation name for Scotland/Wales
  federation: ['TETRE'],     // must match a key in FEDERATION_INFO
  weave: ['Sovereign'],      // or ['TBC']
  patterns: {
    computational:    ['Traditional'],   // Traditional | Shared | Pooled
    egressInspection: ['Full local control'],
    interface:        ['Five Safes TES API'],
    contact:          ['email@example.com'],
  },
},
```

To add a new federation, add an entry to `const FEDERATION_INFO = {...}` with a `color` that doesn't clash with the existing nation colours (`#2a5f72`, `#3d6b4f`, `#5a4e72`, `#c9a227`) or federation colours.

---

## Geography

The map uses inline TopoJSON derived from ONS boundary data available (here)[https://martinjc.github.io/UK-GeoJSON/]:

| File origin | Coverage |
|-------------|----------|
| `topo_eer_eng.json` | England — Electoral Electoral Regions (EERs) |
| `topo_eer_wales.json` | Wales — single boundary |
| `topo_eer_scor.json` | Scotland — single boundary |
| `topo_wpc_north_ire.json` | Northern Ireland — Westminster Parliamentary Constituencies |

The TopoJSON is baked into `index.html` at build time — no external data fetches at runtime.

---

## Tech stack

- Vanilla HTML/CSS/JS — no build step, no dependencies
- TopoJSON rendered as inline SVG paths via a custom arc-decoding implementation
- All assets self-contained — works offline once loaded

---
