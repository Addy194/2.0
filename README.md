# 🌊 Varuna Netra

### Satellite–AIS Intelligence Platform for Oil-Spill Detection, Vessel Correlation & Maritime Investigation Support

<p align="center">
  <b>Smart India Hackathon Project</b><br/>
  Turning satellite observations and vessel telemetry into an explainable maritime evidence workflow.
</p>

<p align="center">
  <img alt="Status" src="https://img.shields.io/badge/status-working%20prototype-19c2d1">
  <img alt="Frontend" src="https://img.shields.io/badge/frontend-React-61DAFB?logo=react&logoColor=white">
  <img alt="Backend" src="https://img.shields.io/badge/backend-FastAPI-009688?logo=fastapi&logoColor=white">
  <img alt="Database" src="https://img.shields.io/badge/database-MongoDB-47A248?logo=mongodb&logoColor=white">
  <img alt="Satellite" src="https://img.shields.io/badge/satellite-Sentinel--1-4C8BF5">
  <img alt="Scope" src="https://img.shields.io/badge/scope-decision%20support-orange">
</p>

> **Decision support — not a legal determination.** Varuna Netra ranks investigation candidates and preserves evidence/provenance. It does not automatically declare legal responsibility.

---

## 🚨 Problem

Oil-spill investigations are difficult because a satellite observation may occur after the responsible vessel has already moved away. Satellite imagery can reveal suspicious sea-surface anomalies, while AIS provides vessel movement history, but these sources are often reviewed separately.

**Varuna Netra connects them into one investigation workflow.**

It helps an operator ask:

- Where is a possible spill or SAR dark-spot anomaly?
- Which vessels were spatially and temporally relevant?
- How strong is the evidence for each candidate?
- Are there AIS gaps or weak vessel tracks?
- Could wind/current drift change the apparent spill position?
- Which maritime jurisdiction may be relevant?
- Can the result be reviewed as a traceable investigation case?

---

## ✨ Core Capabilities

- 🛰️ Sentinel-1 SAR scene discovery and analysis
- 🌊 Experimental SAR dark-spot candidate detection
- 📡 Live and historical AIS ingestion
- 🚢 Explainable vessel-correlation scoring
- 🧭 Drift-aware spatial reasoning
- ⚠️ AIS continuity and reliability checks
- 🗺️ EEZ / jurisdiction reference overlays
- 🧾 Evidence timeline and provenance tracking
- 🗃️ Historical oil-spill incident archive
- 👥 Analyst / supervisor / administrator RBAC
- 🎬 Dedicated SIH judge walkthrough

---

# 🖥️ UI Demo

## Spill Surveillance Dashboard

The main operational view exposes active investigations, review state, AIS fixes, candidate status, confidence information and alerts.

![Varuna Netra surveillance dashboard](docs/ui/surveillance-dashboard.jpeg)

## Live AIS Ingestion

Varuna Netra supports live AIS ingestion and exposes feed state, messages, stored positions, active vessels, coverage selection and vessel telemetry. The interface is designed to report unavailable coverage rather than fabricate vessels.

![Varuna Netra live AIS ingestion](docs/ui/ingestion-live-ais.jpeg)

## SIH Judge Walkthrough

The built-in **Run SIH Demo** flow presents the system as a short evidence story:

**Problem → AOI → Sentinel-1 Scene → Spill Candidate → AIS Vessels → Candidate Ranking → Evidence Timeline → Jurisdiction → Provenance → Investigation Case**

![Varuna Netra SIH demo](docs/ui/sih-demo.jpeg)

---

## 🧠 Explainable Vessel Correlation

Varuna Netra does not rely on a simplistic **“nearest vessel = responsible vessel”** rule. Candidate scoring is designed around interpretable evidence.

| Factor | Investigation purpose |
|---|---|
| Spatial proximity | How close the vessel track came to the anomaly/corridor |
| Temporal proximity | How close the AIS position was to satellite acquisition time |
| Track continuity | Whether the available AIS history is sufficiently continuous |
| Heading compatibility | Whether vessel movement is directionally plausible |
| Drift compatibility | Whether environmental drift supports the relationship |
| AIS reliability | Penalises weak, incomplete or implausible telemetry |

The platform can reduce or cap confidence when evidence is weak, insufficient or ambiguous. A high-ranked vessel is therefore an **investigation candidate**, not an automatic accusation.

---

## 🛰️ Satellite Analysis

Varuna Netra is designed around **Sentinel-1 SAR**, which is useful for maritime monitoring because radar imagery can operate through cloud cover and at night.

The current image-analysis component should be described as an **experimental SAR dark-spot candidate detector**, not as a fully validated oil-spill classifier. SAR look-alikes can include low-wind areas, biogenic films, wakes, upwelling and other oceanographic effects.

That is why detection is treated as the **start of an evidence workflow**, followed by AIS correlation, provenance and human review.

---

## 🧭 Workflow

```text
Sentinel-1 SAR
      │
      ▼
Dark-spot candidate detection
      │
      ▼
Space-time investigation corridor ◀──── Wind / current context
      ▲
      │
AIS vessel telemetry
      │
      ▼
Candidate scoring + AIS reliability
      │
      ▼
Ranked investigation candidates
      │
      ▼
Evidence timeline + provenance + jurisdiction
      │
      ▼
Human-reviewed investigation case
```

---

## 🏗️ High-Level Architecture

```text
React Frontend
      │
      ▼
FastAPI REST API
      │
      ├── Authentication / RBAC
      ├── Satellite scene services
      ├── AIS live ingestion
      ├── Spill / case ingestion
      ├── Correlation engine
      ├── Drift modelling
      ├── Jurisdiction services
      ├── Evidence / provenance
      ├── Historical archive
      └── Reporting
      │
      ▼
MongoDB + supporting storage/services
```

---

## 🧰 Technology Stack

**Frontend:** React, Tailwind CSS, Leaflet / geospatial UI  
**Backend:** Python, FastAPI, async workers, REST APIs  
**Database:** MongoDB / Motor  
**Satellite:** Sentinel-1 SAR, Microsoft Planetary Computer STAC  
**Maritime data:** AISStream and imported/historical AIS  
**Geospatial:** GeoJSON, jurisdiction/EEZ reference data, trajectory/drift reasoning  
**Security:** RBAC, server-side secret handling, explicit CORS configuration, audit-oriented workflows

---

## 🔐 Security & Evidence Integrity

The repository is configured to keep environment files, runtime uploads, internal test outputs and credentials out of source control. The application uses server-side environment variables for secrets and role-based authorization for privileged operations.

**Never commit real API keys, passwords, access tokens or production `.env` files.** If a secret was ever committed, rotate/revoke it because deleting it from the latest commit does not remove it from Git history.

See [SECURITY.md](SECURITY.md) for the repository security policy.

---

## ⚙️ Local Development

### Backend

```bash
cd backend
python -m venv .venv

# Windows
.venv\Scripts\activate

# Linux/macOS
source .venv/bin/activate

pip install -r requirements.txt
```

Copy the safe configuration template and replace placeholders locally:

```bash
# Windows PowerShell
Copy-Item .env.example .env

# Linux/macOS
cp .env.example .env
```

Then start the API:

```bash
uvicorn server:app --reload
```

### Frontend

```bash
cd frontend
yarn install
yarn start
```

---

## 🔑 Main Environment Variables

The sample file is provided at [`backend/.env.example`](backend/.env.example). Important values include:

```env
MONGO_URL=mongodb://localhost:27017
DB_NAME=varuna_netra
JWT_SECRET=replace-with-a-long-random-secret
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=replace-with-a-strong-password
FRONTEND_URL=http://localhost:3000
CORS_ORIGINS=http://localhost:3000
AISSTREAM_API_KEY=
```

Use deployment secrets for real values.

---

## 🎯 Recommended SIH Demo Flow

1. **Problem** — satellite detection alone cannot identify a responsible vessel.
2. **AOI** — establish the maritime area being investigated.
3. **Sentinel-1 scene** — show acquisition metadata and source.
4. **Spill candidate** — show the experimental SAR dark-spot result.
5. **AIS vessels** — show telemetry around the acquisition window.
6. **Why this vessel?** — explain ranking factors rather than a black-box score.
7. **Evidence timeline** — reconstruct the sequence of events.
8. **Jurisdiction** — show the relevant reference maritime zone.
9. **Evidence & provenance** — show where the inputs/results came from.
10. **Investigation case** — open the full evidence-backed case for human review.

---

## ✅ Validation Status

The codebase contains backend tests under `backend/tests/`. For judge-facing claims, the project deliberately separates software verification from scientific detector validation.

See **[docs/VALIDATION.md](docs/VALIDATION.md)** for the current validation position and the next benchmark steps.

---

## 🚧 Current Limitations

- SAR dark-spot detection is experimental and needs broader labelled validation.
- AIS availability varies by region, provider and time window.
- A vessel with missing/disabled AIS cannot be reconstructed using AIS alone.
- Drift estimates depend on the quality and availability of environmental data.
- Maritime-zone data is reference information, not a legal boundary ruling.
- Candidate ranking indicates investigative relevance, not legal liability.

---

## 🔭 Future Work

- Validated ML-based SAR oil-spill classification
- Sentinel-1 + optical multisensor fusion
- Stronger historical AIS coverage
- Dark-vessel reasoning using SAR ship detections
- Higher-fidelity wind/ocean-current reanalysis
- Expanded benchmark datasets and precision/recall/F1 reporting
- Secure attestation / trusted-compute support for high-integrity deployments

---

## 🌍 Intended Impact

Varuna Netra aims to reduce the manual effort required to combine **satellite observation + vessel history + environmental context + jurisdiction + evidence review** into a traceable investigation workflow for maritime surveillance and environmental-response teams.

---

<p align="center">
  <b>From detection to investigation — with uncertainty made visible.</b>
</p>
