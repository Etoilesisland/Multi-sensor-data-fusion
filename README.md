# 🧩 Multi-sensor Data Fusion Dataset

This repository contains multi-source sensor datasets designed for **indoor environment perception and human activity recognition** research.  
The data come from light, temperature, and occupancy sensors collected under different indoor conditions.

---

## 📁 Dataset Structure

| File Name | Description |
|------------|--------------|
| `light_01`, `light_02`, `light_03` | Indoor light environment data (illuminance sensor readings, unit: lux) |
| `temp_01`, `temp_02`, `temp_03` | Indoor thermal environment data (temperature sensor readings, unit: °C) |
| `occupancy_01`, `occupancy_02`, `occupancy_03` | Human occupancy and localization data (binary occupancy or position coordinates) |

Each group of files corresponds to synchronized data collected during the same experimental session, suitable for **multi-sensor fusion and model validation**.

---

## 🧠 Data Description

- **Sampling rate**: Refer to each file’s timestamp column (in seconds or milliseconds).  
- **Format**: CSV or TXT, each line contains a timestamp and corresponding sensor readings.  
- **Time synchronization**: All sensors were time-calibrated, enabling direct timestamp-based alignment.  
- **Data fields**:
  - *Light environment*: Illuminance intensity  
  - *Thermal environment*: Temperature (and possibly humidity)  
  - *Occupancy data*: Occupied status (`0` = unoccupied, `1` = occupied) or spatial coordinates  

---

## ⚙️ Example Usage (Python)

```python
import pandas as pd

light = pd.read_csv("light_01.csv")
temp = pd.read_csv("temp_01.csv")
occupancy = pd.read_csv("occupancy_01.csv")

# Merge datasets by timestamp
merged = pd.merge_asof(light, temp, on="timestamp")
merged = pd.merge_asof(merged, occupancy, on="timestamp")

print(merged.head())
