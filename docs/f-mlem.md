# F-MLEM Cell Model

F-MLEM = **Foveated Multi-Layer Elevation Mapping**. Each active cell stores multiple complementary information groups instead of a single elevation or occupancy value.

| Group | Suggested fields | Notes |
|---|---|---|
| Ground | \(z_{ground}\), \(\sigma_z^2\) | Ground estimate and elevation variance; retain validity/support. |
| Obstacle | \(z_{min}\), \(z_{max}\), \(P_{occ}\) | Obstacle vertical interval and occupancy evidence. |
| Clearance | \(z_{free}\), \(z_{clearance}\) | Overhead/free-space constraints; represent unknown separately. |
| Terrain | slope \(S\), roughness \(R\), elevation variation \(E\) | Feature definitions and windows must be documented. |
| Semantic | majority class, confidence \(C\in[0,1]\) | Store class evidence and avoid treating unobserved cells as confidently classified. |
| Dynamic | \(P_{dyn}\), \((V_x,V_y)\) | Velocity requires temporal association; otherwise mark unavailable. |
| Temporal | timestamp \(t\), count \(N_{obs}\) | Define clock, decay and reset policies. |
| Hierarchical metadata | tier, spatial bounds, parent/coverage reference | Use explicit bounds and cross-resolution coverage; tiers are not strictly integer nested. |

## Data integrity rules
- Record sensor/map coordinate frame and units.
- Keep invalid and unknown states explicit.
- Define the aggregation/decay rules used for each field.
- Do not update a dynamic velocity from one frame alone.
- Keep timestamp and observation-count semantics consistent across sensor frames.
- Do not collapse clearance or obstacle height intervals into a 2D occupancy flag.
