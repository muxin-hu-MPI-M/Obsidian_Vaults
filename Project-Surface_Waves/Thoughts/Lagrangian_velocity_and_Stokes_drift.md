---
tags:
  - wave/surface_wave
  - project/surfwaves
  - concept
  - Theory
Last Eddited: 2026-08-24
---
# Wave-averaged Boussinesq (WAB) Equations
- These equations are found by a multiscale asymptotic analysis, where small, fast motions are separated from larger, slower motions. If wave steepness is taken to be a small parameter, and the fast, small-scale solutions are taken to be surface gravity waves, then the ==**WAB equations result upon filtering out the waves but retaining the leading order wave couplings with other scales** [Craik and Leibovich, 1976; McWilliams et al., 2004]. (Suzuki & Fox-Kemper, 2016)==
- Equations (1), (2), (3), and (4) are identical through vector identities but differ significantly in interpretation. 
	- In (1), the forces are organized in a way that is often used in the regular (i.e., without rapidly oscillating surface waves) Eulerian dynamics. Then, the wave-current nonlinear interaction is regarded as a modification of the regular vortex force, ðr3uÞ3u, to a wave-influenced one, ðr3uÞ3uL, accompanied by a modification of the pressure. This form also allows a straightforward combination of the Coriolis force and the vortex force as ðr3u1fÞ3uL. 
	- Equation (2) shows the dynamics of the slow currents, following the filtered Eulerian velocity ð@t1u rÞu. For these reasons, (1) and (2) are popular [e.g., McWilliams et al., 1997; McWilliams and Fox-Kemper, 2013]. 
	- ==**The form of (3) is closest to the exact wave-current interaction theory known as the Generalized Lagrangian Mean theory [Andrews and McIntyre, 1978; Ardhuin et al., 2008] and is useful in showing that the WAB equations are an asymptotic approximation to the exact theory** [Leibovich, 1980; Holm, 1996]. ==
	- Finally, (4) reveals the similarity in wave-current interaction to the Lorentz force in plasma physics [Holm, 1996]. Other closely related forms are discussed in Garrett [1976]; Smith [2006]; and Ardhuin et al. [2008].

# Wave-filtered Eulerian/Lagrangian velocity
But in a wave field, ocean velocity contains rapid wave orbital motion plus slower current. If we average over wave periods, the fixed-point Eulerian wave velocity can average to nearly zero, while a parcel still has a net drift due to not perfectly symmetric wave orbital motion for a moving parcel. That net wave-induced drift is the **Stokes drift**.
- Wave-filtered Eulerian velocity (i.e, Eulerian velocity of the slow currents)
- Wave-filtered Lagrangian velocity: $$\mathbf{u}^L = \mathbf{u} + \mathbf{u}^s$$
- The Stokes drift and Lagrangian velocity are taken to be the average velocity of a trajectory to leading order in small wave slope