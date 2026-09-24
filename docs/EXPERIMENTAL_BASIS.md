# Published experimental basis

Reviewed synthesis: 24 September 2026. Existing source records, extracted papers and the prior full-text audit were reused. No new experimental dataset was collected for this update.

## What can be concluded

Physical experiments support the feasibility of important parts of the proposed workflow: measuring induction response together with coil geometry, interpreting multiple configurations jointly, using calibration examples to estimate target properties, and obtaining useful additional measurements at selected positions. This is sufficient to motivate a concrete research design.

The papers use different sensors, targets, environments and estimators. Their combination does not constitute an experimental test of the proposed one-TX/three-RX instrument. Their accuracy, depth, speed and classification rates cannot be assigned to it. This document supplies the engineering rationale, while [Prior art](../PRIOR_ART.md) addresses overlap and [Validation](VALIDATION.md) identifies untested claims.

## Physical demonstrations and their scope

| Primary source and locator | Actual experiment or evidence | Reusable lesson | Limit of transfer |
|---|---|---|---|
| [Friedel, Asch & Oden, 2012 — ALLTEM](https://academic.oup.com/gji/article/190/2/960/650325), DOI 10.1111/j.1365-246X.2012.05522.x; sections 3.1 and 4 | Multiaxis measurements, calibration-trained self-organizing maps, comparison with physical inversion and separate blind field sites | Reference-based estimation of buried-target properties can be combined with multiaxis physical interpretation; calibration, training and independent evaluation must be distinguished | Three orthogonal TX directions and 19 TX/RX combinations differ from our one-TX carrier. This does not validate our geometry or a calibrated joint target-center probability volume |
| [Song et al., 2016](https://gif.eos.ubc.ca/sites/default/files/sdevriese/files/5856083.pdf), DOI 10.1155/2016/5856083; section 4.2, Figures 6–9 | Real MPV measurements in an indoor office setup: manually moved sensor over a wooden plate, standard metallic objects below it, two- and three-object cases | Updated estimates can guide another physical measurement; joint interpretation of additional soundings improved the reported recoveries in these cases | Not a field trial in soil. The MPV uses three orthogonal transmitters and five triaxial receiver sets. This is not evidence for our one-TX/three-RX geometry, general autonomy or a matched-time advantage |
| [Feldkamp & Quirk, 2017](https://www.tayoscorp.com/static/OpticallyTrackedMIT-62fab7f8c8dbe4c4f2e6b28bf4decdf7.pdf), DOI 10.1117/1.JMI.4.2.023504; sections 6–9, Figures 9–14 | Optically tracked, synchronized single-coil scans and 3D reconstruction of conductive inclusions in laboratory phantoms | Coil-center calibration, coordinate transforms, synchronization and sampling coverage are practical components of induction imaging | Inductive-loss imaging in a biomedical conductivity regime; it does not demonstrate arbitrary metal shapes in soil or independently moving TX/RX |
| [SERDP MM-1310, completed 2006](https://serdp-estcp.mil/projects/details/fc7d5880-1201-4eca-b283-662fb8c2b7c7); official project summary, technical approach and results | Gimbaled EM61 cart, orientation sensing, laboratory and survey tests; orientation incorporated into forward/inverse models | Both mechanical stabilization and measured orientation can support the acquisition/model workflow | Project-summary evidence, not a newly reviewed raw dataset or full quantitative replication. Its low-metal IMU worked on many bench tests but failed to provide reliable survey measurements |

The ALLTEM full publisher text was reviewed during the preceding focused audit. It must no longer be labelled abstract-only. Its 65,456 calibration records were divided between training and validation; that split alone does not establish independent-object generalization. Separate blind field evaluations are a distinct part of its evidence. Parameter histograms should not be relabelled as a calibrated joint spatial probability map.

Song's abstract uses broad field-test wording, but the actual real-measurement setup in section 4.2 is explicitly indoors. That section, rather than the broad label, governs the setting described here. Its background measurement and subtraction procedure also matters. The implemented example used a diagonal data-error covariance despite possible correlated environmental noise; it does not validate our proposed treatment of all shared pose and calibration errors. In the three-object example, combining the additional measurements required an appropriate three-object inversion model.

Feldkamp and Quirk report loss of distinctness and smearing for deeper phantom features, with noise and sampling imbalance affecting the result. Those limitations are part of the demonstration. Their reported tracking precision is a property of their apparatus, not a coil-position requirement or achieved accuracy for this project.

The SERDP summary reports interference between the EMI system and some orientation hardware, and an unsuccessful low-metal IMU development. Adding an IMU is therefore not, by itself, evidence that the required pose accuracy will be achieved in operation.

## What is not a physical demonstration of this design

- Liao & Carin's cited adaptive-EMI examples and Tantum et al.'s position-uncertainty results are numerical studies. They remain relevant methodological precedents.
- The earlier local tensor-prior and localization calculations are synthetic and limited; they do not validate a complete learned reconstructor or the proposed apparatus.
- Focused Bayesian design and sequential design with nuisance variables/costs are established methods. Their existence settles broad algorithmic novelty claims, not hardware feasibility for this layout.
- Patent descriptions show disclosed approaches and claim boundaries. Their descriptions alone do not establish measured performance.
- Instrument manuals and project pages are useful comparators, but numerical specifications belong to the named instrument under its stated conditions.

Benavides & Everett remains a relevant geometry-aware inversion reference in [Prior art](../PRIOR_ART.md). The retained local full-text retrieval failed, so this update does not use it as the sole primary evidence for a new detailed experimental assertion. No broad re-search is needed to support the narrower conclusions above.

## Design decisions supported by these experiments

1. Preserve raw measurements and calibrated TX/RX geometry together. Calibrate tracking offsets and clocks, rather than assigning a nominal platform pose to all coils.
2. Make the scene estimate depend on all unique accepted measurements. A new scan can change the inferred number or location of targets; a visually narrower map is not sufficient evidence of improvement.
3. Separate apparatus calibration, preparation of reference examples and inference on unknown scenes. Freeze learned parameters during a survey and test independent objects.
4. Define failure and out-of-model outputs. Track model mismatch, noise, poor sampling and pose errors as possible causes of an incorrect map.
5. Retain several acquisition configurations for comparison. Prior multiaxis success does not establish that RX-only rotation supplies the excitation diversity of multiple TX directions.

These are evidence-supported engineering choices, not claims to a newly invented sensing principle. The resulting [design specification](DESIGN_SPEC.md) can be completed as a concept now. Project-specific superiority, calibrated uncertainty and instrument specifications remain subjects for later measurement.
