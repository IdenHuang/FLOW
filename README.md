<div align="center">
  <img src="https://github.com/user-attachments/assets/c33bd05a-6a49-4d3b-a755-c5ae5cdff7a1" alt="FLOW Title Page" width="100%">
</div>

# Flow: First-Responder Logistics Over Water

**A real-time disaster navigation platform that transforms raw satellite imagery into safe routing for emergency rescue teams.**

Built for the StormHacks 2026 ALEASAT track, FLOW identifies active flood boundaries using Sentinel-1 Synthetic Aperture Radar (SAR) data. By overlaying this geospatial data onto standard city grids, the platform maps out navigable channels, allowing first responders to bypass submerged hazards and route directly to stranded civilians or critical infrastructure.

## 🚀 Key Features
<img width="1600" height="800" alt="flowSecond" src="https://github.com/user-attachments/assets/cf53a4df-a580-40d7-86bf-d7fba939814c" />
<img width="100%" alt="flowThird" src="https://github.com/user-attachments/assets/48fbcb17-c0db-4ca4-a77e-4b13e54a90ec" />

* **Satellite-Powered Flood Detection:** Pulls free, high-quality SAR GeoTIFFs via the Copernicus API to map water levels through heavy storm clouds and darkness.
* **Interactive Intelligence:** A custom MapLibre interface providing street-level flood coverage percentages and satellite observation timestamps.
* **Dynamic Rescue Routing:** Breaks drivable roads into 20-meter segments and scores them by water pixel density to generate hazard-aware rescue paths
* **Critical Infrastructure Tracking:** Automatically maps hospitals, shelters, and police stations with real-time water-detection statuses.



## 🛠️ Tech Stack

* **Backend & API:** Python, Django, Django Ninja
* **Geospatial Data Pipeline:** Rasterio, GeoPandas, Shapely, OSMnx, NetworkX
* **Frontend:** MapLibre, HTML/CSS (Custom thematic token system), Vanilla JS
* **Data Sources:** Copernicus Data Space Ecosystem, OpenStreetMap, Photon Geocoder

## ⚙️ Architecture & Data Pipeline

1. **Trigger & Fetch:** A user searches a location, triggering a background job that fetches the latest Sentinel-1 radar image and a pre-flood reference image.
2. **Geospatial Processing:** The Python pipeline performs change detection, isolating pixels that turned dark as water and filtering out radar "speckle" (false readings under 40 meters) and permanent bodies of water.
3. **Network Scoring:** OSMnx fetches drivable city grids. The pipeline maps the flood mask against these roads and critical buildings, scoring them for safety.
4. **Delivery:** The scored segments are converted into optimized GeoJSON and served to the MapLibre frontend via Django Ninja REST endpoints.

## 💻 Quick Start (Local Development)

### Prerequisites

* Python with `uv` installed.
* A free Copernicus Data Space account.

### Setup

1. Clone the repository and add your Copernicus credentials to a `.env` file in the root:
```env
CDSE_USERNAME=you@example.com
CDSE_PASSWORD=yourpassword

```


2. Install dependencies and start the development server:
```bash
uv sync
uv run python manage.py runserver

```


3. Open `[http://127.0.0.1:8000](http://127.0.0.1:8000)` in your browser. API documentation is auto-generated at `/api/docs`.

### Common Commands

* **Linting & Formatting:** `uv run ruff check .` / `uv run ruff format .`
* **Run Tests:** `uv run python manage.py test`
* **Rebuild Demo Data (Abbotsford):** `uv run python -m pipeline.detect_flood`
