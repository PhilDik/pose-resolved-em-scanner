# Validation status and required tests

This document separates structural results from experimental claims. No completed hardware prototype, measured depth specification or validated end-to-end probability volume is established by this package.

## Published physical evidence

The project need not repeat established experiments in order to document a plausible design. The [experimental basis](EXPERIMENTAL_BASIS.md) records published physical evidence for tracked induction imaging, multiaxis reference-based interpretation and sequential use of additional measurements. This is evidence for specified components and workflows under the original conditions; it is not validation of this project's complete apparatus or a source of transferable performance numbers.

In particular, Song et al.'s real MPV examples in section 4.2 are indoor experiments. ALLTEM reports separate blind field evaluations. These settings must remain distinct. Patents and the local synthetic calculations do not substitute for either kind of physical experiment.

## Established within the stated model

For a compact linear target at a known position and a fixed TX excitation vector, measurements have the form `y_i = a_i^T M_H h`. Regardless of how many RX positions are used, the unknown tensor enters through one three-component vector `M_H h`. The operator therefore has rank at most three with respect to six unrestricted symmetric tensor components. Frequency diversity alone does not supply missing directions if each frequency has its own otherwise unconstrained tensor. Additional TX positions or orientations can supply independent excitations, subject to conditioning. See [Smith & Morrison](https://doi.org/10.1109/TGRS.2005.846869).

For identical RX coils that are rotational copies, let their full poses be `g_k(theta) = g_0(theta + 2 pi k/3)`. The union over `k = 0,1,2` and `0 <= theta < 2 pi/3` equals the single-RX full-turn pose set. For discrete sampling, choose matching grids and omit duplicate endpoints. This identity requires a fixed TX, static scene and equivalent calibrated channels. The three-channel arrangement can collect the poses with less carrier travel; it does not triple distinct geometries.

Spinning an ideal circular coil about its own normal at a fixed center does not change its magnetic geometry. Moving an off-center coil around a carrier changes its position even when its normal stays vertical. These are different operations.

For an isotropic compact target on the common axis of a circular TX and a horizontal RX ring at one station and frequency, the ideal response is independent of carrier azimuth and has the form `y = alpha K(d)`. An unknown scalar response `alpha` can trade off against depth `d` within the allowed target class. Additional angular samples do not resolve that ambiguity by themselves. This is a restricted structural counterexample, not a prohibition on localization using additional stations, excitation geometries or justified constraints.

## Completed exploratory checks and their limits

Local calculations, separate from this concept package, checked finite-loop fields, known-location tensor observability, and synthetic unknown-position inversion. The earlier inversion used one TX and one RX with fixed, RX-tilted or whole-head-tilted geometries. It used a single real response component for one known-present compact target, without ground response, direct coupling, hardware drift or material/shape reconstruction. Lengths were normalized, not a prediction in centimeters.

A reference-derived Gaussian tensor prior was tested against joint physical inversion of the same data. Benefits were conditional on the geometry and response family; performance could worsen on a changed response family. Additional optimizer starts changed the difficult fixed-geometry comparison, with some remaining convergence and boundary issues. This is not validation of all reference learning or a complete neural reconstruction.

A separate structural check confirmed the symmetric three-RX pose identity for horizontal and equally radially tilted receivers. It did not simulate the proposed 50/30/8-cm apparatus with electronics, ground and coupling. The full three-RX unknown-scene comparison remains undone. These qualitative records are preliminary checks, not a reproducible performance benchmark supplied by this documentation-only package.

## Compare map refinement at matching acquisition stages

Use independent held-out scenes and a declared sequence of measurement stages: initial scan, additional angles or positions, and repeated passes. For each stage compare:

- physical reconstruction without training on target examples, using the full registered geometry, calibration and suitable physical constraints;
- a specified pretrained reconstruction model;
- a physics/learning combination, if selected as an implementation candidate.

Each method receives the same accumulated data and may refine its map. The pretrained parameters remain fixed during the survey. When testing a learned prior specifically, hold the remaining forward model and fitting procedure constant. Separate the benefit of more measurements from the benefit of the processing method.

Measure position/depth error, false detections and uncertainty calibration against independent truth; evaluate response-class performance separately if it is included. More confident or visually sharper maps are not automatically more accurate. Retain ambiguous, empty and out-of-family scenes. Report computation time and, for acquisition-efficiency claims, total movement, settling, acquisition time and energy.

No experiment comparing complete learned and physical map refinement under this protocol has yet been completed. The exploratory Gaussian-prior result is a narrower test and cannot settle this broader question.

The [future comparison protocol](BENCHMARK.md) specifies a bounded two-acquisition/two-processing experiment, including failures, shared errors, equal resource limits and an inconclusive outcome. It is a prospective test, not a prerequisite for completing this documentation or a report of measurements already taken.

## Required work before performance claims

| Stage | Required comparison or evidence |
|---|---|
| Specification | Declare target sizes and depth range, background, height, RX normals, frequency/time-domain convention, TX current, turns, wire, transfer function, receiver dynamic range, noise and pose/timing errors |
| Hardware feasibility | Measure primary leakage, saturation, recovery/settling, RX loading/crosstalk, motor and cable noise, thermal and energy limits; evaluate any shield with and without it |
| Independent modelling | Check finite targets and relevant conducting/magnetic backgrounds against an independent solver or measurements, not only the same forward model used in fitting |
| Geometry | Compare one RX on the same orbit, the three-RX carrier, RX tilt, whole-head tilt and a stronger fixed-head spatial route; separate equal-data and equal-total-time/energy comparisons |
| Learning | Specify the learned component and compare using the staged protocol above; hold out physical objects, scenes and backgrounds; include deliberately out-of-family targets |
| Map refinement | Keep learned parameters fixed and compare accuracy, false detections and uncertainty after the same accumulated measurements |
| Probability validation | Include empty scenes, ambiguous targets and later validated combinations; measure false positives, localization errors and interval coverage; account for common coordinate/calibration errors |
| Practical benefit | Measure complete cycle time, signal quality, energy and reliability; report detection, localization, classification and shape separately |
| Optional modes | Validate continuous motion and adaptive scanning separately before claiming their benefit |

Repeated measurements may reduce independent noise. Shared pose and calibration errors can persist and make confidence misleading. More angles, higher power, lower frequency and more training examples do not guarantee a particular depth or resolution. Skin depth is not detection depth; scan pitch is not spatial resolution.

## Publication boundary

The defensible status is a research concept with explicit hypotheses and negative/conditional preliminary findings. The current evidence does not support a new physical principle, a demonstrated overall invention, a universal material identifier, an optimal mechanical layout, or a threefold speed/accuracy claim. A measurable implementation benefit remains a research question.
