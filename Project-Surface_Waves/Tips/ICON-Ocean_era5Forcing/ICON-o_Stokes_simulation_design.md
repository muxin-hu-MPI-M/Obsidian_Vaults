---
tags:
  - project/surfwaves
  - wave/surface_wave
  - ICON-O
  - ICON/experiment
Last Eddited: 2026-08-31
---

# Fixed Design
## Summary Table

| EXP ID | Forcing | Period | Description |
| ------ | ------- | ------ | ----------- |
|        |         |        |             |

## Description
- **Parent run** 
	- ICON-O, r2b7, L72
	- ERA5 Stewart single-year-repeat forcing (1990/1991)
	- 15 years of spin up; model year 1990-01 to 2015-01
- **Branches**: restart from the Parent simulation
	- noStokes: continued simulation without Stokes forcings, but with detailed output
	- withStokes: 
		- enable Stokes forcing and Apply velocity diffusion on Eulerian velocity
		- in the first month, enable the `l_restart_from_eulerian = .TRUE.`, then continue with normal Stokes restart
## Output
- Parent run: minimum output with only `_P1M_qps` and `_P1M_mon`
- Branches:
	- **Common**:
		- monthly means of 3D scalar: `to, so`
		- monthly means of 3D velocity: `u,v,w,uu,vv`
			- `uu,vv` used for EKE calculation: $EKE = \frac{(<u^2>-<u>^2+<v^2>-<v>^2)}{2}$
		- monthly means of surface 2D fields: `tos, sos, zos, zos_square, mlotst, mld, HeatFlux_Total, HeatFlux_Shortwave, HeatFlux_LongWave, HeatFlux_LongWave, HeatFlux_Sensible, HeatFlux_Latent, atmos_fluxes_SWnetw, xxx` → check with Helmuth
		- monthly means of vertical mixing: `tke` and all tendency terms, length scale
		- monthly means of freshwater/heat transport per basin: `global_hfbasin, atlantic_hfbasin, pacific_hfbasin, global_sltbasin, atlantic_sltbasin, pacific_sltbasin`
		- monthly means of tracer flux: `uT, vT, wT, uS, vS, wS`
		- monthly eddy density fluxes: `rho, uR, vR, wR` ✅
		- KE budget??
		- near-inertial diagnostic → don’t have in my code
	- withStokes additional
		- monthly means of velocity + `stokes_u, stokes_v, eulerian_u, eulerian_v`
		- monthly means of tracer flux + `eulerian_uT, eulerian_vT, stokes_uT, stokes_vT`, same as salt flux

# Thoughts: [[2026-08-31]]
For the simulations, I would think in three layers:
1. **Immediate / transient adjustment**  
    After Stokes forcing is introduced, the ocean will first adjust through fast processes: velocity changes, pressure-gradient response, vertical motion, and near-surface compensation. This is worth analysing because my forcing is hourly and seasonally varying, so the transient response is part of the physical problem, not just noise.
2. **Seasonal adjusted response**  
    This is probably my main target. After removing the initial spin-up, compare monthly/seasonal means between CTRL and STOKES. The question becomes: does the Stokes-forced experiment show a coherent residual response organised by wave regimes, swell seasonality, latitude bands, or basins?
3. **Longer-term mean imprint**  
    I would avoid calling this “quasi-equilibrium” unless the simulation is very long. The ocean, especially below the mixed layer, may not equilibrate on my integration length. A safer phrase is **“adjusted climatological response”** or **“multi-year mean residual imprint.”**
    
For “non-negligible,” I would define it later using three criteria rather than one number:
- **Detectable**: STOKES minus CTRL exceeds internal variability or remains robust across months/seasons/years.
- **Dynamically organised**: the response is spatially coherent and linked to Stokes transport convergence/divergence, swell regimes, rotation, stratification, or basin geometry.
- **Tracer-relevant**: the circulation response changes heat/salt/tracer transport, especially where compensation occurs at depths with different tracer properties.