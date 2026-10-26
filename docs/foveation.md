# Foveation and Spatial Hierarchy

## Radial zoning
For point \(P=(x,y,z,I)\), use horizontal radial distance:

\[
r=\sqrt{x^2+y^2}
\]

| Condition | Tier | Resolution |
|---|---|---:|
| \(0\le r<10\,m\) | Level 0 / near | 0.05 m |
| \(10\le r<30\,m\) | Level 1 / mid | 0.15 m |
| \(30\le r\le100\,m\) | Level 2 / far | 0.50 m |
| \(r>100\,m\) | Out of range | excluded by default |

Given the selected resolution \(\Delta(r)\):

\[
i=\left\lfloor x/\Delta(r)\right\rfloor,\qquad j=\left\lfloor y/\Delta(r)\right\rfloor
\]

## Boundary behavior
The conditions are deliberately half-open so exactly 10 m belongs to the mid tier and exactly 30 m belongs to the far tier; exactly 100 m remains in range. Test just below, at and just above each boundary. A production implementation should state its floating-point tolerance policy.

## Cross-resolution references
0.05 m, 0.15 m and 0.50 m are not strict integer multiples of one another. A robust spatial index should keep each cell's world-space bounds and represent parent/coverage relationships explicitly. Queries near tier boundaries should resolve overlapping cell coverage deterministically and avoid gaps or double-counting. Do not model these tiers as a simple octree unless the geometric partition actually satisfies its invariants.

## Configuration
Keep range limits, radial thresholds, resolutions, and out-of-range policy configurable. Configuration changes should be recorded with benchmark results for reproducibility.
