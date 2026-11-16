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
three related diagnostics:
1. **Reference-state departure**
    $$ F_S(t)-\operatorname{clim}(F_N). $$
    This shows how the withStokes state evolves relative to the noStokes climatology at each time step
2. **Mean climatological response**
    $$ \overline F_S-\overline F_N. $$
    This quantifies the finite-period mean difference.
3. **Departure relative to control variability**
    Show the noStokes percentile envelope or standardized departure $Z_S$. $$Z_S(t,\phi) = \frac{ F_S(t,\phi)-\overline{F_N}(\phi) }{ \sigma_{N,\mathrm{residual}}(\phi) }.$$
    This expresses the withStokes departure in units of noStokes variability. Because the control is drifting, $\sigma_{N,\mathrm{residual}}$ should preferably be calculated after removing its seasonal cycle and trend. This is an effect-size diagnostic, not automatically a significance test.

> [!Important] research question is now:
> How does the finite-period mean OHT climatology differ between withStokes and noStokes?

Use the same matched 30 post-branching years for both simulations.
- For the annual-mean response: $$\Delta F_{\mathrm{annual}}(\phi) = \overline{F_{\mathrm{withStokes}}}(\phi) - \overline{F_{\mathrm{noStokes}}}(\phi).$$
	- This produces one latitude profile.
- For the climatological monthly response: $$\Delta F_m(\phi) = \overline{F_{\mathrm{withStokes}}\mid m}(\phi) - \overline{F_{\mathrm{noStokes}}\mid m}(\phi),$$
	- where $m$ is January, February, etc. This produces 12 latitude profiles and describes how the mean seasonal cycle differs.
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

## [[2026-11-15]] `global_hfbasin` vs `global_hfl` 
The source explicitly describes the final step as calculating heat transport from the residual of heat-content tendency and integrated surface heat flux. See the [ICON-O diagnostic source](https://github.com/wafarm/icon-model/blob/main/src/ocean/diagnostics/mo_ocean_diagnostics.f90) and [ICON-O output documentation](https://icon-o.gitlab-pages.dkrz.de/icon-o-documentation/0306_output.html).

### Definition
Consider the ocean north of latitude $\phi$. Define:
- $H_N(\phi)$: heat content north of $\phi$;
- $Q_N(\phi)$: integrated surface heat flux north of $\phi$, positive into the ocean;
- $F(\phi)$: northward OHT across $\phi$, positive into the northern region.

Its heat budget is
$$ \frac{\partial H_N}{\partial t} = Q_N+F. $$
Therefore,
$$ F(\phi) = \frac{\partial H_N}{\partial t} - Q_N. $$
In the ICON source:
$$ \texttt{global\_hfl}=-Q_N, $$
while the final diagnosed transport is
$$ \boxed{ \texttt{global\_hfbasin} = \texttt{global\_hfl} + \frac{\partial H_N}{\partial t} } $$
with the appropriate ICON sign convention. Internally, ICON stores the tendency contribution with the opposite sign, explaining the source line

```
global_hfbasin = global_hfl - global_hfbasin
```

The storage calculation includes temperature tendency and, at the surface level, ice and snow tendencies. The implementation is visible in the [ICON-O diagnostic source](https://github.com/wafarm/icon-model/blob/main/src/ocean/diagnostics/mo_ocean_diagnostics.f90).

Conceptually,
$$\mathrm{global\_hfbasin} = \mathrm{implied\ transport\ from\ surface\ flux} - \mathrm{storage\ contribution},$$

using ICON’s internal sign convention.

### Summary
> [!Important] 
> - `global_hfl` answers what northward OHT would be required to balance the surface heat-flux pattern if regional ocean heat storage were zero → implied, at equilibrium $\frac{\partial H_N}{\partial t}\approx 0 \rightarrow F\approx \texttt{global\_hfl}$
> - `global_hfbasin` is a budget-consistent transport diagnostic, but it is not a pure direct advective transport diagnostic.

But my simulations are losing heat, so this equilibrium assumption is not satisfied. Surface-flux-implied OHT does not account for the spatial pattern of ocean heat storage, an important limitation in transient climates.

### At global boundary
At the global boundary, At the southernmost latitude, the northern “cap” is the entire ocean. Physical OHT across the pole must be zero:

$$ F(-90^\circ)=0. $$
But raw implied transport gives
$$ \texttt{global\_hfl}(-90^\circ) = -Q_{\mathrm{global}}, $$

which is nonzero whenever the ocean gains or loses heat globally.
For example,
$$ \Delta Q_{\mathrm{global}} \approx 0.0582\ \mathrm{W\,m^{-2}} \approx20.9\ \mathrm{TW}, $$
so the withStokes-minus-noStokes implied transport at the southern endpoint should be approximately
$$ \Delta\texttt{global\_hfl}(-90^\circ) \approx-20.9\ \mathrm{TW}. $$
The differential heat-content tendency is approximately
$$ \Delta\frac{dH}{dt} \approx+20.0\ \mathrm{TW}. $$
Therefore,
$$ \Delta\texttt{global\_hfbasin}(-90^\circ) \approx 20.0-20.9 = -0.9\ \mathrm{TW}. $$
That is precisely your differential budget residual. So `global_hfbasin` already uses the storage tendency to cancel almost all of the global surface-flux imbalance.
Using `global_hfl` alone would not cancel the `HeatFlux_Total_global` difference—it would explicitly retain it.

> [!Question] Which one to use?
> - If your question is: **How does actual heat redistribution differ between the two transient simulations**?
> 	- then use $$\Delta\overline{\texttt{global\_hfbasin}}.$$
> 	- It represents the budget-inferred OHT after accounting for regional storage.
> - If your question is: **What redistribution would be required to balance the surface-flux anomaly if storage were negligible?**
> 	- then use $$\Delta\overline{\texttt{global\_hfl}}.$$
>
### Three-part decomposition
```python

d_hf = (
    qps_ww["global_hfbasin"].mean("time")
    - qps_nw["global_hfbasin"].mean("time")
)

d_hfl = (
    qps_ww["global_hfl"].mean("time")
    - qps_nw["global_hfl"].mean("time")
)

# Physical cumulative heat-storage contribution north of each latitude
d_storage = d_hf - d_hfl
```
Then calculate the convergence from each:
```python
Q_total = ioht_convergence_from_area_band(
    d_hf.squeeze(drop=True), area_band
)

Q_implied = ioht_convergence_from_area_band(
    d_hfl.squeeze(drop=True), area_band
)

Q_storage = Q_total - Q_implied

```
The physical meanings:
- $Q_{\text{total}} = \Delta C_{\text{OHT}}$
- $Q_{\text{implied}} = -\Delta Q_{\text{surf}}$
- $Q_{\text{storage}} = \Delta \frac{\partial H}{\partial t}$
and: $$\Delta C_{\text{OHT}}=\Delta \frac{\partial H}{\partial t} - \Delta Q_{\text{surf}}$$
This decomposition can tell you whether a convergence anomaly primarily:
- changes regional heat storage;
- balances a surface-flux anomaly;
- or involves both.

### Direct heat transport
Conceptually,
$$ F_{\mathrm{direct}}(\phi) = \rho_0c_p \int_{\text{section at }\phi} v_n\,\theta\,dA $$
represents heat actually crossing the latitude section. Meanwhile,
$$ F_{\mathrm{hfbasin}} = \frac{\partial H_N}{\partial t}-Q_N $$
is the same transport inferred from the heat budget of the ocean north of that latitude. If every budget term is represented consistently,
$$ F_{\mathrm{direct,total}} \approx F_{\mathrm{hfbasin}}. $$
but this usually represents resolved advective heat transport. The budget-derived `global_hfbasin` can include the effective contribution of every process affecting the heat budget:
$$F_{\mathrm{hfbasin}} \approx F_{\mathrm{resolved\ advection}} + F_{\mathrm{parameterized}} + F_{\mathrm{diffusive}} + F_{\mathrm{ice/snow}} + F_{\mathrm{numerical\ residual}}.$$
The useful diagnostic comparison would be
$$F_{\mathrm{hfbasin}}-F_{\mathrm{direct,advective}}.$$
This residual measures the combined contribution from parameterized mixing, diffusion, ice-related terms, sampling limitations and numerical budget errors. So yes: direct transport is the appropriate independent comparison for `global_hfbasin`; `global_hfl` is instead a complementary surface-budget diagnostic.

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

