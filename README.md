# 🛢️ Cement Bond Log (CBL) Interpreter

> **A browser-based, zero-dependency tool for acoustic cement evaluation of oil & gas wells.**  
> Upload a LAS file → configure parameters → get instant CBL interpretation with exportable reports.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-e6a817?style=for-the-badge&logo=github)](https://geoharkat.github.io/Cement_Quality_CBL/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![No Install](https://img.shields.io/badge/No%20Install-Runs%20in%20Browser-blue?style=for-the-badge&logo=googlechrome)](https://geoharkat.github.io/Cement_Quality_CBL/)

---

## 📋 Overview

The **CBL Interpreter** is a fully client-side web application designed for petrophysicists, cementing engineers, and well-log analysts. It parses industry-standard LAS files, computes a **Bond Index (BI)** from CBL amplitude data, and classifies cement quality into Good / Partial / Poor zones — all within the browser with no server, no login, and no data upload to any external service.

---

## ✨ Features

| Feature | Details |
|---|---|
| 📂 **LAS File Parser** | Drag-and-drop or browse — parses `~W`, `~C`, and `~A` sections automatically |
| 🔍 **Auto Curve Detection** | Recognises 25+ common CBL, GR, TT, and depth mnemonics from different vendors |
| 📐 **Casing Presets** | Built-in Schlumberger-reference free-pipe amplitudes for 4½ in to 18⅝ in casing |
| 🔢 **Configurable Resampling** | Bin-averaging at any depth interval (0.1 – 5 m) before interpretation |
| 〰️ **Smoothing Filter** | Adjustable moving-average window applied post-resampling |
| 📊 **Multi-track Log Plot** | Interactive Plotly display: GR | CBL | Transit Time | Bond Index |
| 🏷️ **Zone Classification** | Contiguous depth zones with user-defined Good/Partial/Poor BI thresholds |
| 📈 **Statistics Bar** | Instant summary: logged interval, % Good / Partial / Poor, zone count |
| 📥 **Excel Export** | `.xlsx` with a Summary sheet (parameters + statistics) and a Zones sheet |
| 📄 **PDF Report** | Multi-page dark-themed PDF: cover page, log image, zones table, disclaimer |
| 🔒 **100% Client-Side** | No data leaves your machine — fully offline-capable after first load |

---

## 🚀 Quick Start

### Option 1 — Use the Live App
Open the hosted GitHub Pages link in any modern browser:

```
https://geoharkat.github.io/Cement_Quality_CBL/
```

No installation required.

### Option 2 — Run Locally

```bash
git clone https://github.com/geoharkat/Cement_Quality_CBL.git
cd Cement_Quality_CBL
# Open index.html directly in your browser
open index.html      # macOS
start index.html     # Windows
xdg-open index.html  # Linux
```

> **No build step, no Node.js, no dependencies to install.** All libraries are loaded from CDNs at runtime.

---

## 🖥️ How to Use

**Step 1 — Load a LAS File**  
Drag and drop your `.LAS` file onto the upload zone, or click to browse. The parser will read the curve headers and auto-detect the sample rate.

**Step 2 — Map Curves**  
The tool auto-selects the most likely curves for Depth, CBL Amplitude, Gamma Ray, and Transit Time. Override any selection manually if needed.

**Step 3 — Set Interpretation Parameters**

| Parameter | Description |
|---|---|
| Casing Size | Selects the free-pipe reference amplitude from Schlumberger chart values |
| Free Pipe CBL max (mV) | Amplitude corresponding to 0 % bond (free pipe) |
| 100% Bond CBL min (mV) | Amplitude corresponding to 100 % bond (typically 2–5 mV) |
| Sampling Rate (m) | Depth interval for bin-averaging the raw data |
| Smoothing Window (m) | Moving-average half-window applied after resampling |
| Good / Partial BI Thresholds | Bond Index cutoffs for zone classification |

**Step 4 — Run & Export**  
Click **Run Interpretation** to generate the log plot and zones table. Export results as Excel (`.xlsx`) or PDF.

---

## 🔬 Methodology

### Bond Index Calculation

For each depth sample, the Bond Index (BI) is calculated as:

```
BI = (CBL_max − CBL_amplitude) / (CBL_max − CBL_min)
```

Where:
- `CBL_max` = free-pipe amplitude (0 % bond reference)
- `CBL_min` = fully bonded amplitude (100 % bond reference)
- BI is clamped to [0, 1]

### Zone Classification (default thresholds)

| Class | Bond Index |
|---|---|
| ✅ **Good** | BI ≥ 0.80 |
| 🟡 **Partial** | 0.30 ≤ BI < 0.80 |
| 🔴 **Poor** | BI < 0.30 |

Thresholds are fully adjustable in the UI.

### Zone Boundary Logic

Each resampled point represents the **center** of a depth bin of width `sampleInterval`. Zone top and base are therefore:

```
Zone Top  = first_point_depth − (sampleInterval / 2)
Zone Base = last_point_depth  + (sampleInterval / 2)
```

This ensures that even a single-sample zone carries a non-zero thickness equal to one sample interval.

---

## 📦 Dependencies (CDN)

All loaded remotely — no local installation needed:

| Library | Version | Purpose |
|---|---|---|
| [Plotly.js](https://plotly.com/javascript/) | 2.27.0 | Interactive multi-track log display |
| [SheetJS (xlsx)](https://sheetjs.com/) | 0.18.5 | Excel `.xlsx` export |
| [jsPDF](https://github.com/parallax/jsPDF) | 2.5.1 | PDF report generation |
| [Tailwind CSS](https://tailwindcss.com/) | CDN | Utility class scaffolding |
| [Google Fonts](https://fonts.google.com/) | — | IBM Plex Mono · Barlow · Barlow Condensed |

---

## 🗂️ Supported LAS Curve Mnemonics

The auto-detection engine recognises the following mnemonics (case-insensitive):

**CBL Amplitude** — `CBL`, `CBLA`, `AMP`, `AMPL`, `AMPLITUDE`, `CBLAMP`, `A3FT`, `E3`, `ATTN`, `FIRST_AMP`, `CBL3`, `FA`, and 12 others

**Depth** — `DEPT`, `DEPTH`, `MD`, `TVD`, `RKB`, `MDEPTH`, `MEASURED_DEPTH`

**Gamma Ray** — `GR`, `GRGC`, `GAMMA`, `GRD`, `GAMMA_RAY`, `NGR`, `HCGR` and others

**Transit Time** — `TT`, `TT3`, `TTCO`, `TRANSIT`, `DT3`, `DTC`, `TRANSIT_TIME` and others

---

## 🎨 UI Design

The interface uses a dark engineering aesthetic inspired by modern log analysis software:

- **Primary palette** — Deep navy background (`#0d1117`) with amber accent (`#e6a817`)
- **Monospace typography** — IBM Plex Mono for data values; Barlow Condensed for labels
- **Color-coded zones** — Green (Good) / Amber (Partial) / Red (Poor) throughout the plot, table, and PDF
- **Fully responsive** — sidebar collapses below 900 px for tablet/laptop use

---

## 📄 Export Formats

### Excel Report (`.xlsx`)
Two sheets are generated:
- **Summary** — well name, all parameters, total lengths and percentages per class
- **Zones** — one row per zone with Top, Base, Thickness, CBL Min/Max, Avg BI, Classification

### PDF Report (`.pdf`)
Four pages:
1. **Cover page** — well metadata, parameter table, colour-coded percentage statistics
2. **Log display** — high-resolution Plotly plot image
3. **Zones table** — full colour-coded zone summary
4. **Disclaimer** — license and liability notice

---

## ⚠️ Disclaimer

This software is provided **"as is"**, without warranty of any kind, express or implied. Results must be verified against raw log data and applicable engineering standards before use in any decision-making context. The author accepts no liability for damages arising from the use of this tool.

---

## 👤 Author

**Ismail Harkat**  
📧 [geoharkat@gmail.com](mailto:geoharkat@gmail.com)  
🐙 [github.com/geoharkat](https://github.com/geoharkat)

---

## 📜 License

Released under the [MIT License](https://opensource.org/licenses/MIT) — free to use, modify, and distribute with attribution.

---

*© 2026 Ismail Harkat*
