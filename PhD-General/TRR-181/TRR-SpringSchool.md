---
Last Eddited:
tags:
  - project/PhD_general
  - TRR_181
---

# Science communication:
- general structure for summarising your PhD to someone:
	- **Broad topic**: one sentence to summarise what’s your work
	- **fact**: we know x, we know y
	- **gap**: But we don’t know z, z is important for ...
	- **Therefore**: I’m interested in z ...

# Turbulence course
## Characteristics
While no precise, universally agreed upon mathematical definition of a turbulence state exist.
- chaos
- irregularity
- broad energy spectrum
- non-linearity
- 3D (not universally accepcted)
## various fluid models

# Atmospheric turbulence
- is Richardson number a good indicator for atmospheric turbulence? use the model to test
- turbulence is highly intermittent, appears in small patch
- large area of error forecast using the Richardson number: when Ri is large → stable, but still has TKE generation (more than 40% of the area falls to capture the TKE by simply using Richardson number)
- which parts of the TKE equation generate the TKE?
	- positive horizontal shear: largest shear (source), large negative vertical shear (sink)
	- When Ri >= 1, the flow is stable, TKE is growing because horizontal shear production always exceeds the dissipation
- is TKE-parameterisation good enough for capturing the reality


# Stochastic coupling
- atmosphere has coaser resolution than the ocean
- ==**stochastic approach**== (SA): ~={red}flux calculation by random sample instead of area average=~
- **induce underlying variability into the atmosphere (spatial to temporal)**
	- most model use the **area averaging method**: the SST to air-T gradient is calculating using the area mean SST and area mean air-T → problematic since do not capture the spatial variability over time (e.g., when a eddy passing by)
	- SA: randomly sample (in time within one time step?) but still the mean is maintained
- test on low-resolution: improvement of tropical precipitation
- large scale effects at mid-latitudes: 
	- warmer Atlantic and Southern Ocean in the mean SST❓
	- decrease the variance of the SST, also less variance in the Sensible heat flux
- BUT stochastic approach should increase the variability
	- SA increase the sensible HF variance for **mesoscale**
	- strong effects along SST gradients
	- How does it interact with large scale (which leads to smaller variance there?)
		- Hypothesis:
			- small scale variability might deteriorate the propagation of synoptic scale features and stabilise large scale system








