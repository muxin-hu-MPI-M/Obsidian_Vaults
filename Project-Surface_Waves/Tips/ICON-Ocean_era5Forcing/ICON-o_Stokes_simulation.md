---
tags:
  - project/surfwaves
  - wave/surface_wave
  - ICON-O
  - ICON/experiment
Last Eddited: 2026-08-31
---

# Note
## [[2026-11-16]] How to use `noStokes` simulation?
## [[2026-11-15]] Not thermodynamic equilibrium
![[Pasted image 20261116135006.png]]
When convert the global ocean heat content loss in $J$ to equivalent heat flux $Wm^{-2}$:
```
noStokes
  trend: -6.460e+21 J/year
  power: -204.707 TW
  equivalent flux: -0.5693 W/m²

withStokes
  trend: -5.829e+21 J/year
  power: -184.701 TW
  equivalent flux: -0.5137 W/m²

withStokes − noStokes
  trend: 6.314e+20 J/year
  power: 20.009 TW
  equivalent flux: 0.0556 W/m²
```
- Why reducing? The ocean in general did not reach to a quasi-equilibrium state, with a net energy loss to the ==“atmosphere”, which served as an infinite heat reservoir in our ICON-O simulation==
- The anomaly between `withStokes` to `noStokes` reaches to only $0.05 Wm^{-2}$, which is around 10% compared to the control flux 
This anomaly is well explained by the difference in the total surface heat flux anomaly between two simulations:
![[Pasted image 20261116135233.png]]
By taking the mean over the simulation year:
```
Mean surface flux, noStokes:   -0.2260 W/m²
Mean surface flux, withStokes: -0.1679 W/m²
Surface-flux difference:       0.0582 W/m²

Heat-content tendency difference: 0.0556 W/m²
Differential budget residual:    -0.0026 W/m²
```

- So the difference in the mean surface flux could explain ~95% of the difference in the reduction of the global ocean heat content. 
- the `withStokes` simulation reduces slower in the global ocean heat content, and is reflected mostly in the less negative heat flux (less ocean heat → atmosphere), and is also reflected in smaller global mean SST:
![[Pasted image 20261116135843.png]]
- might be a side story but may not be the main focus of my study, as suggested by Noel #presenter/Noel_Gutierrez-Brizuela , the main focus should be the energy/heat redistribution by the ocean

## [[2026-11-15]] How ICON calculates `global_hfbasin`?
## [[2026-08-31]] Some thoughts
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
# Detailed output
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
		- monthly means of surface 2D fields: `tos, sos, zos, zos_square, mlotst, mld, HeatFlux_Total, HeatFlux_Shortwave, HeatFlux_LongWave, HeatFlux_LongWave, HeatFlux_Sensible, HeatFlux_Latent`
		- monthly means of vertical mixing: `tke, tke_Lmix, tke_Ttot, tke_Tbck, tke_Tiwf, tke_Twin, tke_Tdis, tke_Tdif, tke_Tspr, tke_Tbpr, tke_Pr, tke_plc` 
		- monthly means of freshwater/heat transport per basin: `global_hfbasin, atlantic_hfbasin, pacific_hfbasin, global_sltbasin, atlantic_sltbasin, pacific_sltbasin`
		- monthly means of tracer flux: `uT, vT, wT, uS, vS, wS`
		- monthly eddy density fluxes: `rho, uR, vR, wR` 
		- monthly mean stresses:
			- ERA5 stress received by ICON: `era5_ustress/vstress`.
			- Intermediate open-water atmospheric stress: `atmos_fluxes_stress_xw/yw`.
			- Final stress actually applied to the ocean: `TopBC_SurfStress_u/v`.
			- `tauxu` and `tauyv` are then stress–velocity products derived from the final applied stress.
	- withStokes additional
		- monthly means of velocity + `stokes_u, stokes_v, eulerian_u, eulerian_v`
		- monthly means of tracer flux + `eulerian_uT, eulerian_vT, stokes_uT, stokes_vT`, same as salt flux

