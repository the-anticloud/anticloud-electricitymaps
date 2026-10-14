# ELECTRICITYMAPS

![license](https://img.shields.io/badge/license-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-carbon_capture-lightgrey)

> Anticloud-hardened packaging of the upstream project `ELECTRICITYMAPS` in category **CARBON CAPTURE**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** CARBON CAPTURE · **Upstream:** https://github.com/the-anticloud/anticloud-electricitymaps.git · **Upstream pin:** `c371cd38b6e5191a0b3ba147d339f44d50861399` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

# Electricity Maps Carbon Intensity

Electricity Maps provides historical, real-time, and forecastsed electricity data for more than 150 countries worldwide. Electricity Maps' API is used by leading companies around the world to power carbon-aware decisions. [Read more...](https://www.electricitymaps.com/product-features)

# Overview

The `ElectricityMapsCarbonIntensity` plugin uses [the Electricity Maps API](https://portal.electricitymaps.com/docs/getting-started#geolocation) to compute the average carbon intensity of electricity consumption for a given time period.

Electricity Maps' carbon intensity calculation follows a peer-reviewed methodology to trace back all electricity flows and calculate the carbon intensity of the electricity grid mix adjusted for electricity imports and exports. This provides the most accurate calculation of emissions from electricity consumption.

This plugin can be used to monitor or account for emissions resulting from electricity consumption. The intensity / emissions are aggregated over the duration of the event using hourly data from [the API](https://portal.electricitymaps.com/docs/api#carbon-intensity-past-range).

## Prerequisites

This plugin requires a [Commercial API token](https://www.electricitymaps.com/pricing) from Electricity Maps for authentication.

**Environment Variables**: Set the `EMAPS_TOKEN` environment variable to your API token.

```txt
EMAPS_TOKEN: <your_token>
```

# Inputs

The plugin requires the following inputs:

* `timestamp`: Timestamp of the recorded event (2021-01-01T00:00:00Z) RFC3339
* `longitude`: Longitude of the software system connected to a grid (12.5683). Required if `zone` is not provided.
* `latitude`: Latitude of the software system connected to a grid (55.6761). Required if `zone` is not provided.
* `zone`: Zone identifier of the grid (e.g. `PJM`). Required if `latitude` and `longitude` are not provided.
* `duration`: Duration of the recorded event in seconds (e.g. 3600 for one hour). A single event can last at most 10 days, so the maximum value of this parameter is 864000.
* (optional) `power_consumption`: You can provide the average power consumption in `kWh` for the duration of the event. This will allow the plugin to calulate the total emissions for the event. If this input is not provided, the plugin will calculate the average carbon intensity of the grid for the duration of the event and return the value in units of `gCO2eq`.

# Usage

### In Typescript

Configure the API token environment variable before running the code.

```bash
export EMAPS_TOKEN=your_token
```

```typescript
const plugin = ElectricityMapsCarbonIntensity();
const inputs = [
  {
    timestamp: '2024-03-18T01:36:00Z',
    longitude: 12.5683,
    latitude: 55.6761,
    duration: 3600,
  },
];
const outputs = await plugin.execute(inputs);
```

### Manifest usage

Configure the API token environment variable before running the code.

```bash
export EMAPS_TOKEN=your_token
```

#### Example Input

```yaml
name: electricitymaps-demo
description: example usage of electricitymaps model
tags:
initialize:
  plugins:
    if-electricitymaps:
      method: ElectricityMapsCarbonIntensity
      path: 'https://github.com/electricitymaps/if-electricitymaps'
tree:
  children:
    child:
      pipeline:
        compute:
          - if-electricitymaps
      inputs:
        - timestamp: '2024-03-18T01:36:00Z'
          longitude: 12.5683
          latitude: 55.6761
          duration: 3600
        - timestamp: '2024-03-18T01:36:00Z'
          longitude: 12.5683
          latitude: 55.6761
          duration: 3600
          power_consumption: 100
```

# Outputs

The model returns the average carbon intensity of the grid for the duration of the event. If the power consumption is provided, the model will also return the total emissions for the event.

In JSON (from Typescript):

```json
[
  {
    "carbon_intensity": 0.3,
    "unit": "kgCO2eq/kWh",
  },
  {
    "carbon_intensity": 0.3,
    "unit": "kgCO2eq",
  }
]
```

In YAML (from Manifest):

```yaml
outputs:
  - timestamp: '2024-03-18T01:36:00Z'
    longitude: 12.5683
    latitude: 55.6761
    duration: 3600
    carbonIntensity: 64.4
    unit: gCO2eq
  - timestamp: '2024-03-18T01:36:00Z'
    longitude: 12.5683
    latitude: 55.6761
    duration: 3600
    power_consumption: 100
    carbonIntensity: 6440.000000000001
    unit: gCO2eq/kWh
```

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **Node.js / npm** (manifests: package.json, package-lock.json; scanned in UPSTREAM_CLONE)
- Top-level source layout: `src/`
- Snapshot size: **27 files**, **607 lines of code** (measured; see Benchmarks)
- Primary languages: `.ts` (11), `.json` (7), `(none)` (5), `.js` (3), `.md` (1)
- Upstream commit pinned for this packaging: `c371cd38b6e5191a0b3ba147d339f44d50861399`

---

## Installation

No installation section was found in the upstream readme, so the commands below are generated from the manifests detected in this project directory.

```sh
# from this project directory
npm install        # or: npm ci
npm run build      # if a build script is declared
```

Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

No usage section was found in the upstream readme. Entry points detected in this project directory:

```sh
npm start          # declared start script, when present
npm run dev        # declared dev script, when present
```

Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

The upstream API surface is defined by the `ELECTRICITYMAPS` source tree vendored in `UPSTREAM_CLONE/` (Node.js / npm ecosystem). Public entry points:

- Source modules: `src/`
- The snapshot declares 19 dependency references across 1 ecosystem(s); see Dependencies below.
- Overlay API: `anticloud/cli.py` exposes 13 subcommands with JSON stdout; `anticloud/bench/runner.py` runs the 16-check suite; `anticloud/provenance/chain.py` exposes the SHA3-256 + Ed25519 provenance chain.

---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | Node.js / npm |
| Manifests detected | package.json, package-lock.json |
| Files in snapshot | 27 |
| Lines of code | 607 |
| Dependency references | 19 |
| Dependencies by ecosystem | npm: 19 |
| Upstream license | MIT |
| Overlay license | Anticommons 0.1.0 |

Top dependency references recorded in the benchmark snapshot:

| Ecosystem | Name | Version | Source file |
|-----------|------|---------|-------------|
| npm | @grnsft/if-core | ^0.0.25 | package.json |
| npm | axios | ^1.7.9 | package.json |
| npm | dayjs | ^1.11.13 | package.json |
| npm | dotenv | ^16.4.7 | package.json |
| npm | typescript | ^5.7.3 | package.json |
| npm | zod | ^3.24.2 | package.json |
| npm | @babel/core | 7.22.10 | package.json |
| npm | @babel/preset-typescript | ^7.22.5 | package.json |
| npm | @jest/globals | ^29.6.1 | package.json |
| npm | @types/jest | ^29.5.7 | package.json |
| npm | @types/node | ^20.4.5 | package.json |
| npm | fixpack | ^4.0.0 | package.json |
| npm | gts | ^5.0.0 | package.json |
| npm | husky | ^8.0.0 | package.json |
| npm | jest | ^29.7.0 | package.json |
| ... | (4 more) | | |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

No configuration section was found in the upstream readme. Configuration-relevant files detected in this project directory:

- `package.json`

Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

Upstream contributions: fork the `ELECTRICITYMAPS` project, create a feature branch, and open a pull request against upstream. Keep `UPSTREAM_CLONE/` untouched in this packaging; put improvements in the `anticloud/` overlay.

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: MIT** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
MIT License

Copyright (c) 2023 Green Software Foundation

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original MIT terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `MIT` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `ELECTRICITYMAPS` (category: CARBON CAPTURE)
- **Upstream URL:** https://github.com/the-anticloud/anticloud-electricitymaps.git
- **Pinned commit (SHA):** `c371cd38b6e5191a0b3ba147d339f44d50861399`
- **Branch:** main
- **Pin provenance:** resolved during the second documentation pass. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`c6007bd15865851ec328b3987e8b42d0d2f813232d4e67efa9713ed632367000`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

