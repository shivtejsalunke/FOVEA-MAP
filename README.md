# 🛰️ FOVEA-MAP
### 👁️ Foveated Multi-Layer Elevation Mapping for Autonomous UGVs

<p align="center">
  <img src="https://img.shields.io/badge/Project-SIH%202026-2563eb?style=for-the-badge" alt="SIH 2026"/>
  <img src="https://img.shields.io/badge/Theme-Smart%20Vehicles-0f766e?style=for-the-badge" alt="Smart Vehicles"/>
  <img src="https://img.shields.io/badge/Platform-Autonomous%20UGV-374151?style=for-the-badge" alt="Autonomous UGV"/>
  <img src="https://img.shields.io/badge/Status-Prototype%20Development-f59e0b?style=for-the-badge" alt="Prototype development"/>
</p>

**Team INFERNO** · **SIH2026-26053** · Adaptive Variable-Resolution 3D Environment Mapping

FOVEA-MAP gives autonomous ground vehicles **fine detail nearby and broader terrain awareness at distance**—while retaining elevation, obstacles, overhead clearance, terrain, semantics, motion and observation history.

> ⚠️ **Status:** Architecture and prototype direction are documented here. Planned features and performance targets are not claimed as validated results.

---

## 🎯 Core idea

| 🔎 Near field | 🧭 Mid field | 🌄 Far field |
|---|---|---|
| **0–10 m** | **10–30 m** | **30–100 m** |
| 🟩 5 cm cells | 🟨 15 cm cells | 🟦 50 cm cells |
| Curbs · potholes | Obstacles · routes | Macro-terrain |

`r = √(x² + y²)` · Tier is selected by **horizontal range**, not height. Points beyond 100 m are excluded by default.

## 🧱 F-MLEM: 8 information layers

| Layer | What it retains |
|---|---|
| ⛰️ **Ground** | Ground height + elevation variance |
| 🧱 **Obstacle** | Vertical interval + occupancy probability |
| 🌳 **Clearance** | Free vertical space + overhead ceiling |
| 📐 **Terrain** | Slope, roughness, elevation variation |
| 🏷️ **Semantic** | Class + confidence |
| 🚙 **Dynamic** | Dynamic probability + planar velocity |
| 🕒 **Temporal** | Last observation + hit count |
| 🗂️ **Hierarchy** | Tier, spatial bounds, parent/coverage reference |

**Layers = information. Levels = resolution.** The 5/15/50 cm tiers are not strict integer-nested grids, so cross-tier coverage must use explicit spatial bounds.

## 🔄 Pipeline

<p align="center">
  <b>📡 LiDAR</b> → <b>🧹 Preprocess</b> → <b>🎯 Foveate</b> → <b>🧱 F-MLEM</b><br/>
  ↓<br/>
  <b>⛰️ Terrain & Clearance</b> → <b>⚠️ Hazard Analysis</b> → <b>🗺️ Layered Map</b>
</p>

Optional/model-dependent components: 🧠 semantic inference · 🔁 temporal association · 💨 motion estimation.

## 🏷️ Semantic classes

🛣️ Drivable surface · 🪨 Rough traversable · ⛔ Non-drivable · 🌿 Vegetation · 🧱 Static obstacle · 🪧 Pole/thin structure · 🚗 Vehicle · 🚶 Pedestrian · ❔ Unknown

## ⚠️ Terrain & hazard reasoning

- **Traversability:** (T=f(S,R,E,O)\in[0,1])
- **Negative obstacles:** inspect ground-height discontinuities to help identify potholes, ditches and drop-offs.
- **Overhead clearance:** retain vertical free-space constraints for branches, canopies and overhangs.
- **Unknown ≠ safe:** missing observations must not be treated as free or traversable.

Color thresholds are illustrative until calibrated for the target vehicle and evaluated against reference data.

## 🛠️ Technology direction

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white" alt="PyTorch"/>
  <img src="https://img.shields.io/badge/Open3D-555555?style=flat-square" alt="Open3D"/>
  <img src="https://img.shields.io/badge/ROS_2-22314E?style=flat-square&logo=ros&logoColor=white" alt="ROS 2"/>
  <img src="https://img.shields.io/badge/CUDA-76B900?style=flat-square&logo=nvidia&logoColor=white" alt="CUDA"/>
</p>

Prototype direction: Python, NumPy, Open3D, PyTorch and Plotly/Streamlit. Deployment targets: ROS 2, C++20, CUDA and TensorRT, subject to implementation and hardware validation.

## 📊 Evaluation plan

Compare **foveated 5/15/50 cm** against a **uniform 5 cm baseline** using the same scenes and hardware.

- 💾 Active cells and measured memory
- ⏱️ Map latency, throughput and FPS
- 📏 Ground-elevation error
- ⚠️ Negative-obstacle precision/recall
- 🧠 Semantic metrics when labels/model are available
- 🔁 Motion error when multi-frame ground truth is available

No memory-saving percentage, real-time claim or field-validation result is asserted without reproducible measurements.

## 🗺️ Development status

- [📋 Implementation status](STATUS.md)
- [🧭 Roadmap](docs/roadmap.md)
- [🏗️ Architecture](docs/architecture.md)
- [🧱 F-MLEM model](docs/f-mlem.md)
- [🎯 Foveation & hierarchy](docs/foveation.md)
- [⛰️ Terrain & negative obstacles](docs/terrain-analysis.md)
- [🏷️ Semantic ontology](docs/semantic-ontology.md)
- [🔁 Dynamic layer](docs/dynamic-layer.md)
- [📊 Benchmarking protocol](docs/benchmarking.md)

### 🚀 Next milestone
Make the core foveation and F-MLEM mapping pipeline reproducible, add boundary tests and publish real demo outputs before claiming end-to-end capability.

<sub>Student engineering project for SIH 2026, Smart Vehicles theme. No DRDO endorsement or field validation is implied.</sub>
