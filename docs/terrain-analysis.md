# Terrain Analysis and Negative Obstacles

## Traversability
The intended score is \(T=f(S,R,E,O)\in[0,1]\), where slope \(S\), roughness \(R\), elevation variation \(E\), and obstacle evidence \(O\) contribute to a vehicle-specific traversability estimate. The function, feature windows, confidence treatment, and thresholds must be calibrated against the target UGV and evaluation data.

Illustrative display anchors: 1.0 green (drivable), 0.7 yellow (moderate risk), 0.4 orange (difficult), 0.0 red (dangerous/non-drivable). These are presentation conventions, not verified safety limits.

## Negative-obstacle cues
Compare neighboring ground estimates:

\[
\Delta z=z_{current}-z_{neighbour}
\]

A sufficiently negative value can indicate a step down, pothole, ditch or trench. A configured drop threshold is only a starting parameter; validate it by scene type and sensor geometry. Account for occlusion, missing returns, steep but traversable slopes, ground-fit errors, and false positives before escalating a cell to a hazard.

## Safety-related requirements
- Carry confidence/support alongside derived ground estimates.
- Do not classify missing data as safe/free.
- Preserve obstacle vertical intervals and clearance constraints.
- Evaluate precision, recall, detection range and false alarms against labeled/reference data.
- Report the vehicle assumptions behind any drivable/non-drivable decision.
