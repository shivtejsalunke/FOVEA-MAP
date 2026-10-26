# Architecture

## Design objective
FOVEA-MAP represents a UGV environment using three radial resolution tiers and a multi-field F-MLEM cell schema. It aims to retain geometry and vertical context while limiting the density of far-field spatial representation.

## Logical components
1. **Sensor input:** LiDAR points with coordinates and optional intensity/timestamps.
2. **Preprocessing:** input validation, range filtering, coordinate-frame checks and optional outlier filtering.
3. **Semantic inference (optional):** class scores are attached only when a model is selected and available.
4. **Foveation / spatial index:** horizontal radial distance selects a resolution tier; cells carry bounds and explicit cross-resolution references.
5. **F-MLEM aggregation:** ground, obstacle, clearance, terrain, semantic, dynamic and temporal fields.
6. **Terrain reasoning:** ground support, slope, roughness and elevation variation.
7. **Hazard reasoning:** obstacle extents, overhead clearance and negative discontinuities.
8. **Temporal processing:** when multiple frames are available, associate observations and estimate planar velocity.
9. **Outputs:** layered map, diagnostics, visualization and benchmark records.

## Data and control flow

```text
LiDAR frames
  -> Validation / range filtering
  -> [Optional semantic inference]
  -> Radial tier selection: 5 cm / 15 cm / 50 cm
  -> F-MLEM cell aggregation + spatial bounds/references
  -> Terrain / clearance / obstacle / negative-drop reasoning
  -> [Temporal association when multi-frame data exists]
  -> Layered map + diagnostics + visualizations + benchmark metrics
```

## Critical distinctions
- **Layer** means a type of information stored within each cell.
- **Level/tier** means a resolution band in the spatial representation.
- Resolution selection uses horizontal range \(r=\sqrt{x^2+y^2}\), not \(z\).
- The 5/15/50 cm resolutions are not a strict integer-nested grid. Do not assume exact one-to-many cell relationships; store actual bounds and define overlap/coverage queries.
- Unknown observations remain unknown; no-data must not be interpreted automatically as free, traversable or safe.

## Performance validation
The representation is intended to lower spatial-state costs compared with a uniformly fine map, especially at distance. That hypothesis must be tested with the same scenes and explicit memory/timing methods. Mapping-stage savings do not imply that semantic inference or the whole pipeline has lower compute unless those stages are measured too.
