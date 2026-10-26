# FOVEA-MAP
### Foveated Multi-Layer Elevation Mapping for Real-Time Dynamic Environment Perception

**Project:** Adaptive Variable-Resolution 3D Environment Mapping for Autonomous Vehicles  
**Challenge context:** Smart India Hackathon 2026 · Problem Statement **SIH2026-26053** · Smart Vehicles  
**Team:** INFERNO

FOVEA-MAP is a perception and mapping framework designed for autonomous unmanned ground vehicles (UGVs). It allocates finer spatial resolution to nearby terrain and coarser resolution farther away, while retaining multiple kinds of information per spatial cell—ground elevation, obstacle intervals, overhead clearance, terrain difficulty, semantics, motion, observation history, and hierarchy metadata.

The design goal is to balance local hazard detail with wider-area context without representing the entire environment as either a flat 2D occupancy grid or a uniformly dense 3D voxel volume.

> **Project status:** This repository documents the proposed architecture and prototype direction. Features marked as planned or target are not claimed to be complete. Performance benefits must be demonstrated using reproducible benchmarks before being reported as measured results.

## Why this approach?

A UGV needs more than a binary obstacle map. It must reason about uneven surfaces, potholes and drop-offs, obstacles with vertical extent, overhead obstructions, moving agents, and changing observations. Uniformly fine 3D representations can have high memory and compute costs; coarse flat maps can discard important vertical structure.

FOVEA-MAP combines:
- **Foveated spatial resolution:** 5 cm near, 15 cm mid-range, and 50 cm far-field cells.
- **F-MLEM multi-layer cell model:** stores complementary geometric, terrain, semantic, dynamic, temporal, and hierarchy information.
- **Hazard-aware terrain reasoning:** intended to estimate traversability and flag negative elevation discontinuities.
- **Semantic extensibility:** a nine-class ontology for terrain, infrastructure, vehicles, people, vegetation, and unknown points.
- **UGV-oriented evaluation:** compare map efficiency and perception quality against uniform-resolution baselines.

## Foveation model

For a LiDAR point (P=(x,y,z,I)), compute horizontal radial distance:

[
r = \sqrt{x^2+y^2}
]

Select the spatial resolution from the range tier:

| Tier | Radial range | Cell resolution | Intended use |
|---|---:|---:|---|
| Level 0 — near field | (0 \le r < 10\,m) | 0.05 m | Curbs, potholes, local terrain and nearby obstacles |
| Level 1 — mid field | (10 \le r < 30\,m) | 0.15 m | Local path planning and obstacle avoidance |
| Level 2 — far field | (30 \le r \le 100\,m) | 0.50 m | Macro-terrain and distant obstacles |
| Out of range | (r > 100\,m) | Excluded by default | Outside the configured mapping range |

For a selected resolution \\(\Delta(r)\\), the local grid indices are:

[
i=\left\lfloor\frac{x}{\Delta(r)}\right\rfloor, \qquad
j=\left\lfloor\frac{y}{\Delta(r)}\right\rfloor
]

The tier is selected using **horizontal distance** (\sqrt{x^2+y^2}), not height (z). Any implementation that combines tiers must preserve cell spatial bounds and explicit parent/coverage references. The resolutions 0.05, 0.15, and 0.50 m are **not strict integer-nested grid sizes**, so the architecture should not assume that every fine cell maps to an exact fixed number of cells at the next level. Boundary ownership and cross-resolution queries must be handled explicitly.

## F-MLEM: eight information groups per active cell

F-MLEM means **Foveated Multi-Layer Elevation Mapping**. A cell is a multi-field record, not a single height or occupancy value.

| Information group | Representative fields | Purpose |
|---|---|---|
| 1. Ground layer | Ground estimate (z_{ground}), elevation variance \(\sigma_z^2\) | Represent support surface and uncertainty |
| 2. Obstacle layer | (z_{min}), (z_{max}), occupancy probability (P_{occ}) | Preserve vertical obstacle extent and occupancy evidence |
| 3. Clearance layer | Free vertical space (z_{free}), clearance ceiling (z_{clearance}) | Represent overhangs, branches, tunnels, canopies and other overhead constraints |
| 4. Terrain layer | Slope (S), roughness (R), elevation variation (E) | Describe local terrain difficulty |
| 5. Semantic layer | Majority semantic class, confidence (C\in[0,1]) | Attach class evidence to map cells |
| 6. Dynamic layer | Dynamic probability (P_{dyn}), planar velocity (mathbf V=(V_x,V_y)) | Represent estimated motion when temporal association is available |
| 7. Temporal layer | Last observation time (t), observation count (N_{obs}) | Track recency and support |
| 8. Hierarchical metadata | Resolution tier, spatial bounds, parent/coverage reference | Support multi-resolution queries and spatial relationships |

Fields should explicitly represent unknown or unavailable estimates; missing evidence must not be silently treated as free space, safe terrain, or a confident semantic prediction.

## Terrain traversability and hazards

The intended traversability score is:

[
T=f(S,R,E,O)\in[0,1]
]

where (S) is slope, (R) is roughness, (E) is elevation variation, and (O) summarizes obstacle evidence. A conceptual display convention is:

| Score / state | Interpretation |
|---|---|
| 1.0 — Green | Drivable under the configured vehicle model |
| 0.7 — Yellow | Moderate risk; evaluate vehicle limits and uncertainty |
| 0.4 — Orange | Difficult terrain; cautious planning required |
| 0.0 — Red | Dangerous or non-drivable |

These values are **illustrative display anchors**, not a validated vehicle-specific safety model. A real score needs calibrated thresholds, uncertainty handling, and evaluation for the target platform.

### Negative-obstacle detection

For adjacent ground estimates:

[
\Delta z=z_{current}-z_{neighbour}
]

A sufficiently negative discontinuity can indicate a pothole, ditch, trench, ledge, or drop-off. The threshold must be configurable and evaluated against ground-truth labels; the initially suggested threshold is not a validated universal value. Missing returns, occlusion, sensor viewpoint, and ground-estimation errors should be considered before declaring a hazard.

## Semantic point-cloud ontology

The planned/proposed mapping ontology contains nine classes:

1. Drivable surface
2. Rough but traversable terrain
3. Non-drivable terrain
4. Vegetation
5. Static obstacle (walls, barriers, infrastructure)
6. Pole or thin vertical structure
7. Vehicle
8. Pedestrian
9. Unknown

A semantic model is a separate component from the map data structure. Candidate sparse-convolution or point-cloud models may be evaluated, but the repository should identify a model as selected only once a concrete implementation, weights/licence, label mapping, and evaluation path are committed.

## Reference processing pipeline

1. **Input:** LiDAR point cloud with (x,y,z) and optional intensity/attributes.
2. **Preprocessing:** validate data, filter invalid/out-of-range points, and normalize coordinate conventions.
3. **Semantic inference (optional / model-dependent):** attach class scores when a trained model is available.
4. **Foveation:** compute horizontal radius and assign resolution tier.
5. **F-MLEM aggregation:** update cell statistics and spatial metadata.
6. **Terrain analysis:** estimate ground support, slope, roughness, and elevation variation.
7. **Hazard reasoning:** evaluate obstacle extent, clearance and negative elevation discontinuities.
8. **Temporal update (when multi-frame data is available):** associate observations and estimate motion.
9. **Outputs:** layered map products, diagnostics, visualizations and benchmark measurements.

The exact execution order may depend on the model and sensor. In particular, applying foveation after a dense semantic network does not by itself prove a reduction in semantic-inference compute.

## Planned visualization views

The target dashboard contains seven synchronized views:

1. Raw LiDAR point cloud
2. Semantic LiDAR
3. Foveated multi-layer map
4. Elevation and elevation-uncertainty view
5. Traversability cost view
6. Dynamic probability and velocity field
7. Runtime/performance telemetry

These are target views; only views backed by runnable code and actual data should be presented as implemented.

## Technology direction

| Stage | Technologies under consideration |
|---|---|
| Prototype / analysis | Python 3.10+, NumPy, SciPy, Open3D, PyTorch, Plotly or Streamlit |
| Robotics integration | ROS 2 Humble, C++20 |
| Edge optimization target | CUDA kernels and TensorRT INT8, subject to hardware/model compatibility |

This table indicates intended technology direction, not proof that all components have already been integrated.

## Evaluation plan

Benchmark FOVEA-MAP against a uniform-resolution baseline using the same input sequence, range, filtering rules, and machine. Report actual measurements and configuration.

Suggested metrics:
- Active cells and memory footprint (with the measurement method stated)
- Map update latency, throughput/FPS and CPU/GPU utilization
- Ground-elevation error and uncertainty calibration
- Terrain classification/traversability agreement
- Negative-obstacle precision, recall and detection distance
- Semantic quality by class (e.g., IoU and confusion matrix) when labels are available
- Dynamic association/velocity error when multi-frame ground truth is available
- Behaviour across near/mid/far transitions and around tier boundaries

**No memory-reduction percentage, real-time throughput, safety performance, or field-validation claim should be published without reproducible evidence.** Use public datasets such as KITTI/SemanticKITTI or nuScenes for suitable initial experiments, while clearly noting that these do not, by themselves, validate off-road or defence-specific operating conditions.

## Repository status

See [STATUS.md](STATUS.md) for the implementation ledger and [docs/roadmap.md](docs/roadmap.md) for staged development. Architecture specifications and target behavior are not the same as an implemented, tested feature.

## Getting started

At present, this README describes the architecture and evaluation plan. Follow the repository's committed package instructions and scripts once they are available. Do not assume that model weights, ROS 2 nodes, CUDA kernels, datasets, or a full end-to-end dashboard are included unless explicitly listed in the repository.

## Documentation

- [Architecture](docs/architecture.md)
- [F-MLEM cell model](docs/f-mlem.md)
- [Foveation and spatial hierarchy](docs/foveation.md)
- [Terrain and negative obstacles](docs/terrain-analysis.md)
- [Semantic ontology](docs/semantic-ontology.md)
- [Dynamic layer](docs/dynamic-layer.md)
- [Benchmarking protocol](docs/benchmarking.md)
- [Implementation status](STATUS.md)
- [Roadmap](docs/roadmap.md)

## Team and challenge context

Developed by **Team INFERNO** for the Smart Vehicles theme of Smart India Hackathon 2026, in response to problem statement **SIH2026-26053: Adaptive Variable Resolution 3D Environment Mapping for Autonomous Vehicles**.

This is a student engineering project/prototype. No affiliation, endorsement, or field validation by DRDO is implied by this repository.
