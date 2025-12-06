---
tags:
  - ICON
  - project/surfwaves
Last Eddited: 2025-11-27
---
# [[2025-11-05]]
## Short Overview of ICON-waves
![[Screenshot 2025-11-05 at 11.39.28.png]]

![[Screenshot 2025-11-05 at 11.40.03.png]]

![[Screenshot 2025-11-05 at 11.40.44.png]]

![[Screenshot 2025-11-05 at 11.41.06.png]]

![[Screenshot 2025-11-05 at 11.41.29.png]]

![[Screenshot 2025-11-05 at 11.42.43.png]]

## Wave-ocean coupling
![[Screenshot 2025-11-05 at 11.44.59.png]]
- replace the surface wind stress by the wave stress
	- since the full coupling will be like wind stress -> wave stress -> ocean
- waves as a mediator for the energy transfer
![[Screenshot 2025-11-05 at 11.48.08.png]]

![[Screenshot 2025-11-05 at 11.50.19.png]]
- two options for the Stoke's drift:
	- the Brevik's profile: doesn't have veer with depth
	- full spectrum can capture the veer with depth
- both profiles can be compute efficiently, Brevik's profile is 3-5 times faster
![[Screenshot 2025-11-05 at 11.51.49.png]]

![[Screenshot 2025-11-05 at 11.52.45.png]]
- introducing the wave-to-ocean parameters (stress?)
- coarser resolution makes the Langmuir turbulence calculation unresolvable (the vortex force should be parameterized)
- wave can affect the ocean currents via incompressibiliyt (the momentum)
- two focuses at the moment to start:
	- wave stress
	- wave -> Stokes drift (in momentum equation) -> Langmuir turbulence -> additional term in TKE

