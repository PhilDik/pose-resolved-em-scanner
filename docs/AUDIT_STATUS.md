# Audit status

Documentation version: **2026.09.25**. Author: **Philipp Dik**.

The bounded technical and prior-art review for this documentation version is complete. It began with the existing repository and retained local research outputs; targeted verification and corrections followed. This is a record of the reviewed evidence and unresolved limits, not an exhaustive patent search or a determination of patentability or freedom to operate.

## Published scope

The existing candidate architecture is preserved: one large TX loop and three separately read RX coils on a common rotating carrier, with provisional 50/30/8 cm packaging dimensions. The [schematic](../figures/three-rx-ring.svg) is a conceptual layout, not a photograph, manufactured prototype or validated mechanical drawing. Its original concept date is retained.

The research hypothesis concerns the whole chain: measured TX/RX pose, calibration, repeated measurements, joint reconstruction and optional reference-based learning. Additional measurements update the scene map and its uncertainty; any pretrained model remains fixed during scanning. Wheel-integrated coils remain an optional mechanical idea without an established advantage.

## Conclusions and evidence limits

- Relevant physical experiments support parts of this workflow. Their apparatus, conditions and limitations are identified in the [experimental basis](EXPERIMENTAL_BASIS.md). Their measured performance does not transfer to this proposed head.
- The reviewed scientific and patent disclosures substantially overlap the broad sensing, pose registration, inversion and adaptive-measurement ideas. Overall novelty has not been established; absence of an exact match would not establish it.
- The repository does not contain a completed instrument, a validated reconstruction implementation or evidence of a measured complete-system advantage. The [validation record](VALIDATION.md) separates structural arguments and local synthetic checks from physical measurements.
- The [future comparison protocol](BENCHMARK.md) defines work needed for new performance claims. Those experiments remain future work; they are not represented as completed by this publication.

## Retained source gaps

The [prior-art review](../PRIOR_ART.md) preserves source-level qualifications. In particular:

- US20260227541A1 was reviewed through a saved Justia reproduction. Its official publication has not been independently verified in this audit; detailed claim conclusions remain provisional.
- The retained Benavides-Everett full-text retrieval failed. It is not used as the sole primary evidence for new detailed experimental claims.
- ALLTEM's full publisher text was reviewed, including the experimental and evaluation sections. A failed separate HTML download does not negate that reading or revert its status to abstract-only.

These gaps limit the corresponding assertions. They do not require repeating the whole search to publish the narrower conclusions above. A later claim that depends on an unresolved source should trigger a focused verification of that source.

## Version and citation

This version packages the existing concept and its reviewed limitations. The substantive evidence update is recorded in commit [`489087be00ca06d03b853a5e56f8d07cad41cab0`](https://github.com/PhilDik/pose-resolved-em-scanner/commit/489087be00ca06d03b853a5e56f8d07cad41cab0). See the [change record](../CHANGELOG.md) and [citation metadata](../CITATION.cff).

A new project-specific experiment is required before claiming instrument performance or superiority. The present documentation can stand as a research concept with those limitations explicit.
