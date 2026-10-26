# Roadmap

This roadmap separates architecture intent from implementation completion. Update [STATUS.md](../STATUS.md) when code and tests actually land.

## Phase 1 — Reproducible spatial prototype
- [ ] Project packaging and configuration validation
- [ ] Vectorized point filtering and radial tier assignment
- [ ] Cell index with explicit spatial bounds and cross-tier references
- [ ] F-MLEM typed data model and deterministic aggregation
- [ ] Synthetic/demo point cloud and baseline visualization
- [ ] Unit tests for radial boundaries and invalid inputs

## Phase 2 — Geometry and terrain
- [ ] Ground estimation with confidence/support
- [ ] Obstacle height intervals and overhead clearance estimation
- [ ] Slope, roughness and elevation-variation features
- [ ] Negative-obstacle detection with tunable thresholds
- [ ] Evaluation against labeled/reference scenes

## Phase 3 — Semantic perception
- [ ] Select and document one model and licence/weight source
- [ ] Implement preprocessing and nine-class label mapping
- [ ] Evaluate per-class metrics on a documented dataset
- [ ] Keep semantic inference results separate from unvalidated labels

## Phase 4 — Temporal/dynamic layer
- [ ] Timestamp synchronization and ego-motion policy
- [ ] Multi-frame association and motion estimation
- [ ] Confidence, occlusion and stale-data handling
- [ ] Evaluate velocity and dynamic-class performance

## Phase 5 — Benchmark and dashboard
- [ ] Uniform 5 cm baseline and reproducible measurements
- [ ] Active-cell, memory and latency reports
- [ ] Seven-view dashboard backed by real outputs
- [ ] Publish configuration and raw measurements

## Phase 6 — Robotics and edge deployment
- [ ] ROS 2 integration and bag replay
- [ ] C++20 implementation/profiled hot paths
- [ ] CUDA/TensorRT only where profiling shows benefit and hardware supports it
- [ ] Edge target validation on a named device

## Advanced research direction
- [ ] Risk-, uncertainty- or path-aware dynamic foveation
- [ ] Compare against fixed radial zoning under the same workload
- [ ] Demonstrate that increased resolution is allocated where it improves task metrics
