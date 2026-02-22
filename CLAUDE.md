# CLAUDE.md

## Project: WWDI — Would We Detect It?

WWDI is an open-source detection coverage testing platform. Users submit payloads, samples, strings, or any artifact from the cyber kill chain and run them against a community-maintained bank of detections. The results surface hits (coverage) and misses (gaps), giving defenders a clear picture of their detection posture.

Anyone can contribute detections in the language of their choosing (Sigma, YARA, Suricata, Snort, KQL, SPL, custom regex, etc.).

## Repository Info

- **Owner:** MalwareEngineer
- **Repo:** `MalwareEngineer/WWDI`
- **License:** MIT
- **Default branch:** main
- **Hosted at:** GitHub Pages (`.github.io`)

## Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                   GitHub Pages UI                   │
│            (Static SPA — modern, minimal)           │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌──────────────┐         ┌──────────────────────┐  │
│  │  Submit Panel │ ──────▸│  Detection Engine     │  │
│  │              │         │                      │  │
│  │ • Paste/upload│         │ • Loads detection bank│  │
│  │   payload     │         │ • Runs input against  │  │
│  │ • Select kill │         │   all rules           │  │
│  │   chain phase │         │ • Returns hits/misses │  │
│  └──────────────┘         └──────────────────────┘  │
│                                    │                │
│                          ┌─────────▼─────────┐      │
│                          │  Results View      │      │
│                          │                   │      │
│                          │ • Hit/miss summary │      │
│                          │ • Coverage heatmap │      │
│                          │ • Gap analysis     │      │
│                          └───────────────────┘      │
│                                                     │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│               Detection Bank (repo)                 │
│                                                     │
│  detections/                                        │
│  ├── sigma/        (Sigma YAML rules)               │
│  ├── yara/         (YARA rules)                     │
│  ├── suricata/     (Suricata rules)                 │
│  ├── snort/        (Snort rules)                    │
│  ├── kql/          (Kusto queries)                  │
│  ├── spl/          (Splunk queries)                 │
│  └── custom/       (Regex / other formats)          │
└─────────────────────────────────────────────────────┘
```

### Key Concepts

- **Detection Bank:** The central collection of community-contributed detection rules, organized by language/format.
- **Submit Panel:** Users provide a payload, sample, log line, command string, or any kill-chain artifact to test.
- **Detection Engine:** Client-side matching engine that evaluates the submitted input against all loaded detections.
- **Results View:** Displays which detections fired (hits) and which did not (misses), highlighting coverage gaps.

## Hosting & UI

- **Hosted on GitHub Pages** (`MalwareEngineer.github.io/WWDI`).
- **Static single-page app** — no backend required; all detection matching runs client-side.
- **Modern, comfortable theme** — clean layout, dark/light mode, readable typography.
- **Tech stack (planned):** HTML/CSS/JS (vanilla or lightweight framework). Keep dependencies minimal so the site loads fast and works on GitHub Pages without a build server.

## Planned Project Structure

```
WWDI/
├── LICENSE
├── CLAUDE.md
├── README.md
├── index.html                  # Entry point for GitHub Pages
├── css/
│   └── style.css               # Theme and layout
├── js/
│   ├── app.js                  # App initialization and routing
│   ├── engine.js               # Detection matching engine
│   └── ui.js                   # DOM helpers and result rendering
├── detections/                 # Community detection bank
│   ├── sigma/
│   ├── yara/
│   ├── suricata/
│   ├── snort/
│   ├── kql/
│   ├── spl/
│   └── custom/
├── data/
│   └── detections-index.json   # Auto-generated index of all detections
└── tests/                      # Tests for the matching engine
    └── engine.test.js
```

## Build & Test Commands

<!-- Update these as implementation begins -->
- **Local dev:** Open `index.html` in a browser, or use `python3 -m http.server` for local serving.
- **Tests:** TBD — likely a lightweight runner (e.g., `node tests/engine.test.js`).
- **Detection index rebuild:** TBD — script to regenerate `data/detections-index.json` from the `detections/` tree.

## Contributing Detections

Contributors can add detection rules in any supported format:

1. Place the rule file under the appropriate `detections/<language>/` directory.
2. Follow the naming convention: `<descriptive-name>.<ext>` (e.g., `mimikatz-credential-dump.yar`).
3. Include metadata (author, description, references) in the rule file where the format supports it.
4. Open a pull request — the detection index will be regenerated on merge.

## Code Style & Conventions

- **JavaScript:** ES modules, `const`/`let` (no `var`), descriptive function names.
- **CSS:** BEM-ish class naming, CSS custom properties for theming.
- **Detection rules:** Follow each language's community conventions (e.g., Sigma specification, YARA style guide).
- **Commits:** Short imperative subject line, body for context when needed.

## Cyber Kill Chain Mapping

The UI should allow tagging submissions and detections to kill-chain phases:

1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control
7. Actions on Objectives

This mapping powers the coverage heatmap — showing which phases have strong detection coverage and where gaps exist.

## Development Notes

- All detection matching must run **client-side** (GitHub Pages constraint — no server-side execution).
- The detection engine needs format-specific parsers/matchers for each supported rule language.
- Start with **Sigma** and **YARA** support, then expand to other formats incrementally.
- Keep the UI accessible and fast — detections are loaded once and cached in the browser.
- The detection index (`detections-index.json`) should be generated via a script or GitHub Action so the client can fetch a single manifest instead of crawling directories.
