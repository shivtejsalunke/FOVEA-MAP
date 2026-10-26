# Benchmarking Protocol

## Purpose
Measure the trade-off between spatial representation cost and retained perception quality. Do not publish savings or real-time claims without repeatable measurements.

## Baselines
At minimum, compare the foveated 5/15/50 cm configuration with a uniformly fine 5 cm representation. Use the same input frames, range, filtering, hardware, coordinate conventions, and evaluation masks. Consider a uniform coarse baseline where useful.

## Metrics
- Number of active cells and occupancy/coverage distribution by range tier.
- Memory consumption, clearly distinguishing theoretical payload size, language-runtime object overhead, and measured process memory.
- Map-update latency (median, p95/p99), throughput/FPS and CPU/GPU utilization.
- Ground elevation error and uncertainty calibration.
- Negative-obstacle precision/recall, false-alarm rate and detection distance.
- Semantic mIoU/per-class IoU and confusion matrix when labels are available.
- Dynamic association and velocity error when multi-frame ground truth is available.
- Boundary behavior at 10 m, 30 m and 100 m.

## Reproducibility record
Record commit, configuration, dataset/sequence, preprocessing, hardware, OS/runtime versions, warm-up policy, number of runs, statistic definition and uncertainty. Include scripts and raw machine-readable outputs where possible.

## Data caveats
Public on-road datasets can support initial method development but do not automatically establish off-road, adverse-weather or defence-specific validation. Do not state that the system is DRDO-tested or field-proven without evidence.
