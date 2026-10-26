# Dynamic Layer and Temporal Fusion

The intended dynamic layer includes dynamic probability \(P_{dyn}\) and planar velocity \(\mathbf V=(V_x,V_y)\). These values require evidence across time; a single point cloud cannot independently provide a reliable velocity estimate.

A complete implementation needs to document:
- Sensor timestamp and coordinate-frame synchronization.
- How detections/cells are associated between frames.
- Ego-motion compensation, if the platform moves.
- Velocity estimation method, confidence and outlier handling.
- Dynamic/static probability update and decay policy.
- Behavior during occlusion, missed detections and stale observations.

Until those steps are implemented and evaluated, dynamic probability and velocity are architectural fields/targets, not proof of moving-object tracking performance. Risk-, path-, or uncertainty-adaptive resolution is an advanced future extension; the baseline zoning described in this repository is fixed by radial range.
