# System design specification

Concept version: 24 September 2026. This defines interfaces and choices, not a finished hardware specification or software implementation. The rationale comes from the [published experiments](EXPERIMENTAL_BASIS.md).

## Bounded task and output

The first implementation target is an empty scene or one compact metallic target from a declared class in a declared volume and background. This restriction defines a tractable starting comparison, not the ultimate scope of every future implementation.

The output is no detected target, an estimated target-center location and uncertainty, or an unresolved/out-of-model result. Depth is referenced to a defined surface. Shape, material identification, multiple interacting targets and arbitrary ground conditions require additional models and validation.

The same acquisition/reconstruction interface can serve a stationary rig or a mobile carrier. Translating, turning in place and changing head orientation are permitted actions if their actual coil geometry is measured. Wheel-mounted coils are an optional mechanical concept without a demonstrated benefit here.

## Modules and interfaces

| Module | Required input or measurement | Output and responsibility |
|---|---|---|
| Excitation | Declared frequency/waveform, calibrated measured TX current and timing | Reproducible excitation record; drive power is not treated as the field at the target |
| Reception | Separate RX channels, transfer functions, background and noise information | Raw data plus calibrated observable, with saturation and quality flags |
| Geometry | Per-coil centers/orientations, tracking lever arms, acquisition time/window | Registered TX/RX geometry and uncertainty in a common frame |
| Calibration | Independent apparatus, timing, tracking and background measurements | Versioned gains, phases, offsets and shared-error model |
| Reference preparation | Known objects/conditions separate from test objects | Fixed library/model and its declared domain; no online retraining during surveying |
| Reconstruction | Unique measurements, forward model, fixed reference version, shared errors | Joint scene/error estimate, model-fit assessment and unresolved result where needed |
| Map update | New measurement IDs plus the existing measurement history | Updated target-center estimate; previous derived maps are not counted again as independent evidence |
| Carrier and display | Executable motion commands and registered estimates | Movement records and display of uncertainty; the display adds no sensing information |

Both reference-assisted and physical-only estimators use actual geometry and common calibration. A comparison must not give one method better tracking or extra measurements without stating it.

## Candidate apparatus

One large TX surrounds three separately read RX coils on one rotating carrier. The earlier 50/30/8-cm diameters remain provisional packaging values. Actual centers, normals, wiring, coil properties and direct coupling must be specified. Three parallel RX coils are not a triaxial vector sensor.

The initial acquisition convention is step, settle, acquire. Receiver timing must distinguish simultaneous from sequential channel reads. Continuous acquisition is an extension conditional on its motion and interference model. A rigid-head spatial route, one RX, RX tilt and whole-head tilt remain meaningful comparators; none is assumed inferior.

Frequency-domain complex transfer impedance `V_RX/I_TX` is one possible observable. Time-domain gated measurements are another and require their own forward operator, current history and gate definition. No excitation mode, frequency set, current level or electronics design is locked by this concept.

## Measurement record

Each observation needs a unique ID, scene/session/pass IDs, channel, time and window, excitation configuration, measured current, raw RX data, calibrated observable with units, full TX/RX poses, calibration/background versions and uncertainties. If pose changes within an excitation/receive window, retain the necessary trajectory or apply a validated rejection rule.

Store SI units and declare coordinate and rotation conventions. One possible convention uses `z` upward and a reference surface at `z = 0`, so below-surface depth is `-z_target`. Independent reference measurements are needed to evaluate absolute location. A common translation of the scene and coil coordinates cannot be resolved by unanchored relative measurements alone.

The stored result identifies the exact observations and model/library versions used, the decision and estimates, uncertainty, fit/convergence diagnostics and resource use. Shared nuisance parameters and residual covariance must not double-count the same error source.

## Reconstruction commitment and open choices

[Reconstruction](RECONSTRUCTION.md) gives a bounded statistical formulation with explicit scene hypotheses, shared nuisance variables and a fixed library. It does not prescribe a trained neural architecture. A first implementation may use a calibrated response prior with a physical forward model; that prior and the fitting method must be specified before calling it a working algorithm.

Adaptive action selection is optional. The initial route can be predefined. Target-focused information, nuisance marginalization and movement cost are known design ideas, not an established project invention. A chosen policy needs a defined objective, feasible actions, cost measurements and appropriate baselines.

## Parameters that remain open

Target class and depth range; background; frequencies or pulse/gate settings; actual coil electrical properties; dynamic range/noise/coupling; pose and clock accuracy; allowed motion; reference model; computation and energy budgets; acceptable localization error and false-alarm rate.

These are implementation inputs, not omissions that published studies can fill with another instrument's numbers. Their absence limits performance claims but does not prevent completion of the current design record. If an instrument or suitable recorded dataset becomes available, the [bounded comparison](BENCHMARK.md) provides the next validation step.
