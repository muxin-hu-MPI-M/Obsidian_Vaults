---
tags:
  - wave/surface_wave
  - proposal/surfwaves
  - stokes_drift
  - ICON/experiment
Last Eddited: 2026-05-21
---
# Assessing Two Stokes Profile Reconstruction Strategies

## Purpose
This note compares two possible Stokes profile forcing strategies for a climate-scale ICON study. The first is a globally complete total-sea reconstruction, here named `STOKES_TOTAL`. The second is a partition-aware reconstruction, here named `STOKES_PARTITIONED`, which reconstructs wind-sea and total-swell components where the decomposition is physically admissible and otherwise requires a fallback. The comparison is framed from both a reviewer perspective and an analysis-design perspective.

The central question is not which method is more physically detailed in an ideal wave-dynamical sense. The central question is which method provides a defensible, interpretable, and globally applicable forcing for a climate-scale experiment.

## `STOKES_TOTAL`: Total-Sea Reconstruction
`STOKES_TOTAL` reconstructs one Stokes drift profile at each grid point using the ERA5 total surface Stokes drift vector and a total-sea Stokes transport estimated from bulk wave parameters. The Breivik-type Phillips-spectrum profile is then used to assign the vertical structure. The main experiment is interpreted through the comparison
$$
STOKES\_TOTAL - CTRL .
$$
### Strengths
- It uses a single closure everywhere in the global ocean. This makes the experiment easy to explain and easier to defend in a climate-scale study.
- It directly matches the main research question: whether adding a plausible reconstructed Stokes profile affects ICON at climate-relevant scales.
- It avoids mixing physically different reconstruction regimes within one operational forcing product. There is no need to combine high-confidence decomposed profiles with lower-confidence fallback profiles.
- It is consistent with the practical motivation of Breivik-type Stokes profile approximations: obtaining a broadband Stokes drift profile from limited bulk information when full spectral reconstruction is not used.
- It is more physically informed than a monochromatic profile because the Phillips-spectrum approximation accounts for a broadband spectral shape while retaining a small number of required inputs.
- It reduces the burden of interpretation for the reader. A climate audience can focus on the ocean response to Stokes forcing rather than on the geometry of wind-sea and swell partitioning.
- It avoids a global attribution claim that the available data cannot fully support. The experiment does not claim to separate wind-sea and swell contributions to the climate response.

### Weaknesses
- It assumes that the whole reconstructed profile follows the direction of the ERA5 surface Stokes drift.
- It cannot represent vertical directional rotation of the Stokes drift profile, which may occur when wind sea and swell propagate in different directions.
- It does not distinguish whether the Stokes forcing arises mainly from wind sea or swell.
- It may underrepresent directional complexity in regions where long swell contributes strongly to the Stokes transport but the surface Stokes drift is dominated by local wind waves.
- Reviewers may still ask whether the bulk transport direction and the surface Stokes drift direction are always compatible. This should be addressed by explicitly stating the closure assumption.

### Reviewer Risk
The main reviewer risk is that the method is approximate. However, this risk is manageable because the approximation is simple, explicit, and globally consistent. A reviewer may disagree with the assumption that the profile follows the surface Stokes direction, but the assumption is easy to state and does not create hidden regime changes across the domain.

### Useful Complementary Tests
- Map the climatological distribution of surface Stokes drift magnitude and estimated total-sea Stokes transport to show that the forcing is physically plausible.
- Compare the reconstructed profile depth scale with simpler monochromatic or exponential-integral profiles at representative locations.
- Test sensitivity to the transport estimate, for example by perturbing the estimated Stokes transport magnitude within a plausible uncertainty range.
- If available for selected regions or periods, compare the bulk reconstructed profile against profiles obtained from direct spectral integration. This is not required for the main climate experiment, but it would strengthen the method.

## `STOKES_PARTITIONED`: Wind-Sea And Swell-Aware Reconstruction

`STOKES_PARTITIONED` attempts to reconstruct separate wind-sea and total-swell profiles. The partition-specific Stokes transports are estimated from wind-sea and total-swell bulk wave parameters. The corresponding surface Stokes magnitudes are obtained by solving a two-vector decomposition of the ERA5 total surface Stokes drift:
$$
u_{s0}\hat{\boldsymbol{\theta}}
=
u_{s,ww}\hat{\boldsymbol{\theta}}_{ww}
+
u_{s,ts}\hat{\boldsymbol{\theta}}_{ts}.
$$
The solution is physically admissible only if

$$
u_{s,ww}\ge 0,\qquad u_{s,ts}\ge 0 .
$$
If this condition is satisfied and the wind-sea and swell directions are not collinear, the two reconstructed profiles can be summed. If the decomposition is inadmissible or ill-conditioned, the method requires a fallback.

### Strengths
- It targets a real physical limitation of the total-sea method. Surface Stokes drift is often more sensitive to short wind waves, whereas Stokes transport and deeper Stokes drift can be more influenced by longer swell.
- It can represent some directional separation between wind-sea and swell contributions where the bulk partition directions are geometrically consistent with the ERA5 total surface Stokes drift.
- It provides useful diagnostic information about where a two-component closure is possible and where the ERA5 bulk directions are mutually inconsistent.
- It is attractive for regional studies or process studies where mixed sea states, swell dominance, or directional wave effects are central to the research question.
- It can serve as a sensitivity test for the total-sea closure by asking whether a more directional reconstruction changes the modeled response.

### Weaknesses
- It is not globally admissible. The decomposition works only where the total surface Stokes vector lies inside the positive cone spanned by the wind-sea and total-swell directions.
- It requires special handling of collinear or nearly collinear directions, where the two-vector system becomes ill-conditioned.
- It requires fallback profiles in rejected or ill-conditioned cases, but those fallback profiles are not physically equivalent to successful decomposed profiles.
- It creates a mixed-closure forcing product: some regions use partitioned profiles, while others use a total-sea fallback. This complicates interpretation of the global model response.
- It does not guarantee true wind-sea and swell attribution because total swell can contain several swell systems, and a bulk total-swell direction may not represent the directional structure of the swell spectrum.
- It increases methodological complexity without necessarily improving the climate-scale interpretation. If the paper does not aim to attribute the response to wind sea versus swell, the added complexity may distract from the main result.

### The Closure Issue
The closure issue is the central weakness of `STOKES_PARTITIONED`. The method tries to infer two unknown surface Stokes magnitudes from one total surface Stokes vector and two prescribed bulk directions. This is only valid when the total vector can be represented as a non-negative linear combination of the two partition directions. If one solved magnitude is negative, the decomposition is not physically admissible: it would require one component to point opposite to its assigned wave direction.

This failure does not mean that wind sea or swell is absent. It means that the available bulk diagnostics are insufficient or mutually inconsistent under the assumed two-component closure. The inconsistency may arise because the total swell direction averages multiple swell systems, because partition-mean directions are energy-weighted rather than Stokes-drift-weighted summaries, or because the ERA5 surface Stokes drift is derived from the full spectrum rather than from only two idealized components.

For climate-scale analysis, this closure issue matters because the model response would be driven by a forcing product that changes its physical meaning from place to place. Where the decomposition succeeds, the profile is partition-aware. Where it fails, the fallback returns to a total-sea assumption. A reviewer could reasonably ask whether the global response reflects Stokes forcing itself, the partitioning method, or the spatial distribution of fallback cases.

### Reviewer Risk
The main reviewer risk is interpretability. A reviewer may accept the physical motivation but still question whether a partially admissible decomposition should be the operational forcing for a global climate experiment. The method could look more advanced but less clean. If the paper's main contribution is climate-scale Stokes forcing rather than Stokes profile reconstruction, the decomposition may make the manuscript harder to evaluate.

### Necessary Complementary Tests
- Map the fraction of admissible, collinear, and rejected grid points over the full simulation period.
- Show where fallback profiles are used and whether those regions overlap with dynamically important climate-response regions.
- Run a high-confidence-only test in which inadmissible or fallback regions are excluded or damped, then compare it with the mixed partitioned run.
- Compare `STOKES_PARTITIONED` against `STOKES_TOTAL` to quantify whether the added directional complexity materially changes the climate-scale response.
- If the difference is small, report the partitioned method as a robustness check. If the difference is large, treat reconstruction choice as a major uncertainty rather than as a clean improvement.

## Synthesis For Choosing The Focused Experiment
For a climate-scale ICON paper, `STOKES_TOTAL` should be the focused experiment. It gives a globally consistent forcing, aligns directly with the climate-impact question, and avoids asking the reader to accept a mixed reconstruction product whose fallback cases are not physically equivalent to successful decompositions. Its limitations are real, but they are transparent and can be stated as part of the bulk-profile closure.

`STOKES_PARTITIONED` should be retained as a diagnostic or sensitivity framework rather than as the default forcing. It is useful for exploring where the total-sea closure may be weakest and for testing whether directional partitioning changes the modeled response. However, because it cannot be applied uniformly over the global ocean, it is less suitable as the operational experiment for a climate-scale paper.

The strongest manuscript strategy is therefore to present `STOKES_TOTAL` as the main experiment and discuss `STOKES_PARTITIONED` as an attempted or potential extension. This keeps the main result interpretable while still acknowledging the known physical limitation of total-sea Stokes profile reconstruction.
