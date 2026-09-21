# Pose-Resolved EM Scanner / Rover

**Author: Philipp Dik**  
**Status: proposed architecture; implementation-specific validation required.**

Pose-Resolved EM Scanner / Rover proposes an electromagnetic induction (EMI) scanning workflow in which measurements are interpreted using the actual positions and orientations of the transmitting (TX) and receiving (RX) coils. Measurements from different configurations are combined to estimate target location and electromagnetic response, with uncertainty. A subsequent measurement may be selected from the current estimate to improve information about an unresolved region or target.

A rover is one possible carrier. Handheld and airborne configurations are alternative embodiments of the same sensing concept. This description does not establish a fixed coil count, a completed hardware configuration, numerical performance, or novelty of the complete combination. The physical sensing mechanisms and several constituent methods have substantial [prior art](PRIOR_ART.md).

## Intended acquisition workflow

1. Acquire preliminary EMI measurements with their TX/RX geometry and acquisition settings.
2. Update a joint estimate of the scene or target parameters and their uncertainty using an applicable forward model.
3. Identify an unresolved region or ambiguity and evaluate feasible additional measurements.
4. Perform the selected supported movement or acquisition change, acquire the additional data, and update the estimate.
5. Continue within the available measurement budget, reporting unresolved ambiguity where the data do not support a reliable conclusion.

The architecture permits independently variable TX/RX geometry where supported by the implementation. Both controlled movements and measured incidental motion may contribute usable measurement diversity. Continuous acquisition is an intended option within a validated motion and error envelope; stationary step-scanning remains an optional mode.

Candidate controls discussed for the adaptive workflow are excitation frequency, TX current or power, TX/RX position and angle, integration time, and scan density. Only controls physically supported by the selected apparatus are available actions. This list does not imply that every embodiment implements every control.

Repeated passes contribute to the same reconstruction. They must not be treated as independent evidence when they share systematic errors. A spatial or AR display presents the model's estimates and uncertainty; it does not itself add subsurface information.

## Geometry, timing and measurement definition

An implementation must define the recorded observable: for example, a calibrated receiver-coil voltage, a filtered or gated flux derivative, or a magnetic-field estimate. It must also specify the excitation waveform or frequency, measured TX current, receiver transfer function, coil geometry, coordinate conventions and applicable units. Frequency-domain and time-domain acquisition are alternative implementations unless both have actually been implemented and validated.

Absolute acquisition time, delay after transmitter shutoff, and acquisition-window duration are distinct quantities. Measurements must be synchronized with the geometry relevant to excitation and reception. Coil centers and axes require calibration relative to the tracking sensors, including lever arms and any relative TX/RX motion.

Accelerometers alone do not determine absolute six-degree-of-freedom coil pose. A suitable tracking system must estimate the needed positions and orientations with a measured uncertainty. Pose error, clock offsets and calibration uncertainty belong in the reconstruction error budget. Previous EMI work has explicitly incorporated sensor orientation and uncertainty in measurement positions. [SERDP MM-1310](https://serdp-estcp.mil/projects/details/fc7d5880-1201-4eca-b283-662fb8c2b7c7), [Tantum et al., 2008](https://scholars.duke.edu/publication/711401).

When geometry changes appreciably during excitation or acquisition, a single instantaneous pose may be insufficient. The measurement model must account for the relevant trajectory and window, or a validated acceptance criterion must exclude unsuitable data. Tracking does not remove motion-induced voltage, variable primary coupling, saturation or excitation-history errors by itself. [WO2016124964A1](https://patents.google.com/patent/WO2016124964A1/en).

## Information available from changing geometry

Changing TX geometry can change the field exciting a target. Changing RX geometry changes the sensitivity to the target's response. These effects must be distinguished when determining whether additional measurements constrain additional unknowns.

In the compact-target linear model, an induced magnetic dipole can be written as `m = M_H H_T`, where `M_H` maps the incident magnetic field H [A/m] to magnetic moment [A·m²] and therefore has units of m³. Literature using B instead of H defines a differently scaled polarizability tensor; the conventions must not be mixed.

With a fixed target location and one fixed incident-field vector, varying only RX directions can determine at most the three components of the induced dipole vector. It does not determine all six components of an unrestricted symmetric polarizability tensor. General tensor recovery requires sufficient independent incident-field directions and receiver sensitivities, with adequate conditioning. TX relocation can provide different incident-field directions even without a dedicated TX rotation mechanism. [Smith & Morrison, 2005](https://doi.org/10.1109/TGRS.2005.846869).

Rotation of an ideal axisymmetric coil about its own symmetry axis at a fixed center provides no new magnetic geometry. More samples of an unchanged measurement operator can improve noise averaging but do not necessarily increase the rank of the inverse problem.

The compact dipole model is an approximation. Finite coil dimensions, extended or interacting targets, and nonuniform conducting backgrounds require an appropriate forward model. A tensor estimate characterizes a response; it is not a unique reconstruction of arbitrary object geometry.

## Reconstruction and interpretation

The intended outputs are model-dependent estimates of target location and electromagnetic response parameters, with uncertainty. Material class and shape are hypotheses within a declared model and reference set. Different combinations of material properties, dimensions, orientation, depth and background can produce similar data. A material classification must allow an unresolved or out-of-model result. Spectral polarizability classification is an established research direction, not a demonstrated project capability. [Wilson, Ledger & Lionheart, 2022](https://arxiv.org/abs/2110.06624).

The inverse problem must account for uncertainty in geometry, background and calibration as well as measurement noise. Displayed voxel probabilities require a defined statistical meaning and validation of their calibration. Repeated measurements can increase confidence in a wrong solution if shared errors or model mismatch are ignored.

Using actual geometry and compensating for orientation are not mutually exclusive. A known invertible rotation of complete vector data preserves information when covariance is transformed consistently. Information loss depends on the particular approximation, discarded channels or averaging procedure. A comparison must therefore specify the actual processing methods rather than assuming that all previous compensation discards useful motion.

## Performance limits and validation plan

The intended benefits are conditional: measured geometry may reduce errors caused by an incorrect geometry assumption; sufficiently diverse TX/RX configurations may reduce ambiguity; continuous acquisition may reduce stopping time; and adaptive selection may reduce the measurements needed for a specified estimation accuracy. None of these constitutes a demonstrated advantage of this project over existing geometry-aware or adaptive EMI systems. Tracking error, motion interference, direct coupling and reconfiguration time can reduce or eliminate a benefit. The corresponding established methods are compared in [PRIOR_ART.md](PRIOR_ART.md).

No project-specific detection depth, spatial resolution, material-identification accuracy, processing latency or adaptive-sensing benefit is established by this document. Skin depth is not detection depth; scan spacing is not spatial resolution; detection does not imply equally capable classification or shape reconstruction. Practical depth depends on the target, background, geometry and measurement errors. [Huang, 2005](https://geophex.com/wp-content/uploads/2023/11/Depth_of_Investigation_Geophysics_2005_Nov-Dec.pdf).

Lower frequency, higher current, additional angles and additional passes do not guarantee a specified improvement. Independent TX/RX motion also changes direct coupling. Receiver dynamic range, coil resonances, current and phase calibration, electronics, nearby metal, actuators, heating and vibration must be characterized for the actual apparatus.

The following work is required to validate the proposed workflow:

| Validation | Evidence needed |
|---|---|
| Hardware and tracking specification | Actual coil geometry, excitation, transfer functions, noise, pose accuracy, timing, energy and thermal limits |
| Observability | Jacobian rank, singular values and parameter sensitivity for the actual configurations and search region |
| Forward-model validity | Comparison with independent electromagnetic solutions for finite coils, targets and relevant backgrounds |
| Moving acquisition | Stationary-versus-moving measurements of the same controls, with separate checks of primary coupling, background, motors and synchronization |
| Reconstruction and classification | Blind tests on withheld targets and backgrounds, including ambiguous cases and calibrated uncertainty |
| Adaptive selection | Comparison with fixed, nonadaptive geometry-diverse and existing adaptive EMI acquisition at matched total time and energy, including movement and computation |
| Practical performance | Separate detection, localization, classification and shape-reconstruction metrics, plus measured latency and resource use |

Simulation should test model mismatch as well as agreement with the model used by the inversion. Numerical acceptance criteria must be specified for the chosen use and apparatus before collecting validation data. Low predicted information gain can reflect insufficient sensitivity or a model limitation; it does not certify correct identification.

## Relationship to existing work

Geometry-dependent EMI inversion, optically tracked free scanning, independently moving generator/receiver coils, pose-uncertainty treatment and sequential EMI experiment design predate this proposal. No new physical sensing principle or verified novelty of the overall combination is claimed here. The open research question is whether a specified implementation of the proposed workflow provides a measurable benefit under realistic errors and resource constraints.

See [PRIOR_ART.md](PRIOR_ART.md) for the closest scientific, instrument and patent comparisons, including the limitations of those comparisons.

Adaptive position and frequency selection for buried-target EMI was already studied by [Liao & Carin, 2004](https://scholars.duke.edu/publication/685904). A distinct technical contribution remains to be identified and tested.
