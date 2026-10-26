# Implementation Status

This file is the source of truth for what is implemented versus what remains architectural intent. Update it alongside code, tests and reproducible evidence.

| Component | Status | Evidence required to mark complete |
|---|---|---|
| Foveation specification (5/15/50 cm) | Defined | N/A — design specification |
| Correct radial formula (r=\sqrt{x^2+y^2}) | Defined | Boundary tests in code |
| Cross-resolution spatial index | Planned / verify repository code | Bounds, deterministic ownership and overlap tests |
| F-MLEM eight-group schema | Specified | Typed implementation and serialization/update tests |
| Point-cloud loader/preprocessing | Verify repository code | Runnable example and error-handling tests |
| Ground estimation and uncertainty | Planned / verify repository code | Evaluation against reference ground |
| Obstacle intervals and clearance | Planned / verify repository code | Scene tests including overhangs |
| Terrain and traversability | Planned / verify repository code | Documented features, calibrated score and evaluation |
| Negative-obstacle detection | Planned / verify repository code | Precision/recall and false-alarm evaluation |
| Nine-class semantic inference | Ontology specified; model selection required | Model, label mapping, weights/licence and benchmark |
| Dynamic association and velocity | Planned | Multi-frame evaluation with ground truth |
| Seven-view dashboard | Target | Runnable views generated from real pipeline output |
| Memory/latency benchmark | Protocol specified | Reproducible scripts and raw results |
| ROS 2 integration | Target | Runnable node and bag-replay test |
| C++20/CUDA/TensorRT deployment | Target | Hardware-specific profiling and correctness checks |
| Risk/uncertainty/path-aware foveation | Future research | Baseline comparison and measured task benefit |

**Important:** “Planned / verify repository code” is intentionally conservative until each component can be inspected and run. Do not report a component as complete based only on the architecture description. No measured memory-reduction, FPS, safety, or field-validation result is asserted here.
