# Future comparison of a concrete implementation

Prospective protocol, 24 September 2026. No measurements under this protocol have been taken. Published experiments support the [design rationale](EXPERIMENTAL_BASIS.md); a new project-specific test is needed only for claims about this implementation's benefit.

## One paired experiment, four outputs

Restrict the first test to empty scenes and single compact targets from a declared class. Acquire each unchanged scene with two predetermined strategies, then process each dataset in two ways:

| Acquisition | P: strong physical reconstruction | H: same reconstruction plus a fixed reference component |
|---|---|---|
| A: spatial route with fixed relative TX/RX geometry | A–P, primary baseline | A–H |
| B: spatial route with additional RX-carrier rotation | B–P | B–H, primary candidate |

Use the same head and available whole-head motions for A/B. A may spend the time saved from carrier rotation on additional useful stations. P and H receive identical data within each acquisition strategy, full registered geometry, common calibration and a shared-error model. P retains suitable physical constraints and adequate solver/convergence checks. H must have a specified implementation and frozen trained parameters.

The principal comparison is B–H versus A–P. B–P versus A–P and H versus P within each acquisition explain possible causes. These diagnostic comparisons cannot replace the principal comparison after seeing the results. This experiment evaluates a bounded implementation, not every possible scanner design or three RX versus one.

## Readiness and separation

Before the test, characterize excitation, current, RX transfer functions, noise, direct coupling, saturation, settling, pose, clock errors and full resource costs. Check the forward model against independent measurements, including an ambiguous/axial configuration. A channel that fails readiness needs correction before a performance test.

Separate physical objects used for reference training, development/pilot and held-out testing. A new orientation of an old training object is not an independent test object. Include empty scenes and report deliberately out-of-domain objects separately from unfamiliar objects within the declared class.

Randomize and balance A/B order on unchanged scenes. Keep independent ground truth inaccessible to inference and parameter tuning until outputs are saved. Declare limits on operator blinding. Preserve difficult cases, misses, refusals and timeouts.

Repeated samples and passes are not independent scenes. Reuse of an object or calibration session creates dependence; define independent blocks or an appropriate grouped analysis before testing. Choose the required number of independent blocks using a separate pilot and a predefined practically important effect. A small available sample may warrant only an exploratory result.

## Resources and map stages

Set equal limits on total time and energy. Include movement, settling, reception, per-survey calibration, inference and permitted retries. Report one-time training/development costs separately.

One simple implementation reserves the same computation allowance for P/H and uses the remaining allowance for acquisition. Count actual costs as well; unequal processing times under a common reserve are not evidence that each system is globally optimized. Choose intermediate and final time checkpoints in advance and save all four maps from the observations available by each checkpoint. The final checkpoint is primary.

Reprocessing the same IDs adds no evidence. Shared errors persist across passes. Both estimators update the scene while the reference model stays fixed.

## Success, failures and uncertainty

Choose a spatial error tolerance `r` and minimum useful improvement `delta_min > 0` for the intended use before the main test. Do not invent universal centimeter or percentage thresholds without an application and instrument.

For every target-present test scene, success requires exactly one reported target within 3D distance `r` of independent truth, inside the time/energy limits. Misses, extra targets, unresolved outputs, failures and budget overruns count as unsuccessful. The estimator is not told whether the scene contains a target.

The primary effect is the paired difference in success rate, B–H minus A–P. Use declared scene/block weights and a preselected 95% interval that respects the dependence structure. Also report:

- false detections on empty scenes with uncertainty and a predefined acceptable limit;
- misses, refusals, solver failures and budget overruns;
- localization/depth errors among reported targets, alongside the number of missing estimates;
- coverage and volume of joint spatial uncertainty regions, with their emission rate;
- all four methods at the fixed checkpoints and actual time/energy;
- separate results on out-of-domain objects and backgrounds.

An uncertainty region covering the entire search volume is not an informative map. If calibrated map uncertainty is part of the claim, define its coverage and informativeness requirements before testing.

## Decision and later work

- Evidence for the desired benefit: the primary interval lies above `delta_min`, and declared false-alarm/resource requirements pass. A map-probability claim also requires its uncertainty checks.
- Required benefit excluded in these conditions: the interval lies below `delta_min`.
- Inconclusive: the interval crosses the threshold or auxiliary evidence is insufficient to assess the required limits.
- Failed requirement: a compulsory operational limit is violated, even if mean localization improves.

Lack of statistical significance is not proof of no possible benefit. A positive result shows a particular implementation's usefulness; it does not by itself establish novelty. Freeze code/library versions, route definitions, data splits, thresholds and the analysis plan before unblinding. Changes after unblinding require new held-out data for confirmation.

Existing recorded measurements may test a compatible processing hypothesis if geometry, raw observations, calibration, independent truth and held-out cases are available. They cannot establish the speed, energy or coupling performance of hardware they did not use. Published papers alone cannot be treated as such a raw dataset.
