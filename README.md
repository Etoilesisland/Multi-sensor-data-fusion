# 🧩 Multi-sensor Data Fusion Dataset

This repository provides multi-source indoor sensing data for **environment perception** and **human occupancy/localization** studies.  
Data come from **light** and **temperature** sensors plus **occupancy/localization** traces.

---

## 📁 Dataset Structure & Formats

| File names | Modality | Format | Notes |
|---|---|---|---|
| `light_01.pkl`, `light_02.pkl`, `light_03.pkl` | Indoor light (illuminance) | **PKL** (pandas DataFrame) | Illuminance (lux) with timestamps |
| `temp_01.pkl`, `temp_02.pkl`, `temp_03.pkl` | Indoor thermal (temperature) | **PKL** (pandas DataFrame) | Temperature (°C) with timestamps |
| `occupancy_01.geojson`, `occupancy_02.geojson`, `occupancy_03.geojson` | Occupancy / localization | **GeoJSON** (vector) | Point/LineString geometries with timestamps and IDs |

> **Time alignment:** All streams are time-calibrated. Use the timestamp fields to align modalities.

---

## 🧠 Data Fields (typical)

- **Light PKL**: `timestamp` (UTC ISO or epoch), `lux`
- **Temp PKL**: `timestamp`, `temp_c`
- **Occupancy GeoJSON**: geometry (`Point` or `LineString`), `timestamp`, `agent_id` (or `room`, `state`)

> Exact column names may vary slightly by file; inspect the DataFrame/GeoDataFrame to confirm.

---

## ⚙️ Example Usage (Python)

### 1) Load the modalities
```python
import pandas as pd
import geopandas as gpd

# Load PKL sensor streams
light = pd.read_pickle("light_01.pkl")
temp = pd.read_pickle("temp_01.pkl")

# Ensure timestamps are datetime and sorted
for df in (light, temp):
    df["timestamp"] = pd.to_datetime(df["timestamp"], utc=True)
    df.sort_values("timestamp", inplace=True)

# Load occupancy/localization (GeoJSON)
occ = gpd.read_file("occupancy_01.geojson")

# Ensure datetime & CRS
occ["timestamp"] = pd.to_datetime(occ["timestamp"], utc=True)
if occ.crs is None:
    # Set a known CRS if applicable (e.g., WGS84)
    occ.set_crs("EPSG:4326", inplace=True)  # change if your data uses another CRS
occ = occ.sort_values("timestamp")
