---
tags:
  - project/MesoEddy_Upwelling
  - "#presenter/Noel_Gutierrez-Brizuela"
  - "#presenter/Xinyue_Li"
Last Eddited: 2026-01-18
---
# Updates

## [[2026-06-05]] Change Figure 4 and caption
Modification of the Figure 4 as: 
- Figure with 2x2 subplots
	- 1st row: Time-mean Temperature in historical run and time-mean difference, with (u,w) and its difference, the difference figure has a contour showing the maximum Temperature trend
	- 2nd row: Time-mean Stratification pattern in historical run and time-mean difference
- Write the corresponding Figure caption and description
	- Logic:
		1. upwelling remains active and intensified slightly, matching with Figure 1, 2 with increased alongshore winds.
		2. Strengthened upwelling keep supply cooler subsurface water to the surface
		3. despite this dynamical intensification, the near coastal waters become warmer, and the warming trend is more pronounced towards the coast
		4. Warming is accompanied by a vertically differentiated response in mean stratification. Specifically, stratification weakens within a shallow near-surface layer ($\sim$20 m), while it strengthens immediately below. The anomalies reach to ~10% of the climatological value
		5. This vertical modulate reflects an enhanced near-surface vertical mixing coexisting with a reduced vertical mixing belows



## [[2026-04-28]] My job: Update Figure 3 & description
Three panels:
1. **Historical** mean section-averaged Temperature + Velocity vector field (u, w)
2. **Future** mean section-averaged Temperature + Velocity vector field (u, w)
3. **Change (Future - Historical)** in Temperature and velocity vector field

The zonal velocity $u$ in the model output is defined in the eastward direction. However, the analysis sections are oriented approximately perpendicular to the Peruvian coastline, which is rotated by about 30 degrees relative to the meridional axis. To ensure a consistent representation, $u$ is projected onto the cross-shore direction to derive the corresponding cross-shore velocity component $u’$:$$u'=u\cdot\cos{(30\degree)}$$
## [[2026-03-04]] 1st Draft
![[Upwelling_region_outline-1.pdf]]

## Discussion on 1st Draft
### Plan for the figures
![[Figures_plan.jpg]]

### Plan for the figures: design
![[Figures_plan_2.jpg]]





# Project Description
![[Peru upwelling project.pdf]]


## Basic Info
- Grid
	- atmosphere: `r2b8_atm_r0033`
	- ocean: `r2b9_oce_r0004`, `lev=72`
## Questions
- “we will assess whether eddy compensation strengthens under continued warming and how this changes change the “residual” upwelling” **Why so-called “residual upwelling”**
- What’s the background of the hypothesis 2? Why mesoscale eddies increasingly compensates the wind-driven overturning over time? Is it because the amount/strength of mesoscale eddies are projected to increase?

## Note: Thomsen et al. (2021)
(Thomsen et al., 2021)
- **Background info**:
	- Coastal upwelling rates are classically determined by the intensity of the upper-ocean offshore Ekman transport. 
	- A potential important effect: quasi-balanced mesoscale and submesoscale turbulence is responsible for rectified eddy transport which also tends to counteract the Ekman upwelling cell. ~={red}In general, (sub)mesoscale turbulence modulates offshore transport, hence the net upwelling rate.=~
	- Eddy effects generally oppose the Ekman circulation, resulting in so-called “eddy cancellation”
		- The eddy-induced circulation attempts to flatten isopycnal surfaces, reduce available potential energy
	- The traditional consideration: Wind-driven Upwelling (derivations from **Marshall and Radko (2003)** **in the context of an upwelling system**.
		- the fluid transported by the Ekman circulation in the mixed layer. The structure being steady, this fluid crosses isopycnals as it moves offshore, and a **net buoyancy gain** is needed that must exactly satisfy the relation
		  $$
		     v_{\text{ek}}\;\partial_{y}\langle \bar{b} \rangle _{\text{ml}}(y)=\bar B_{\text{ml}}(y) \tag{1}
		     $$
		- where:
			- $v_{\text{ml}}$: wind-driven Ekman velocity vertically averaged over the mixed layer;
			- $\langle \bar{b} \rangle _{\text{ml}}$: mean buoyancy in the mixed layer, where $b=-g\rho_0/\rho$ with $g$ being the gravitational acceleration, $\rho$ the potential density.
			- $\bar B_{\text{ml}}$: net buoyancy supply to the mixed layer, supposed to mainly result from air-sea exchanges sector where coastal upwelling is taking place
		- This relationship may seem to exert a strong constraint on the mean upper-ocean thermohaline structure of upwelling systems, because given the wind and buoyancy forcings, the mean buoyancy gradient over the mixed layer would need to adjust so that the relation can be balanced
		- this term neglect the role of the eddy-induced circulation, unrealistic!!
		    
- **Method**: idealised upwelling model with constant winds but ==varying heat fluxes== with and without submesoscale-rich turbulence;
	- Eddy cancellation is evaluated with different methods all account for the **quasi-isopycnal nature （准等密度性质）** of the ocean circulation away from the surface
- **Result**:
	- ==zero heat fluxes==: nearly full cancellation of the Ekman cross-shore circulation by eddy effect near the shore
	- ==with increasing heat fluxes==, the cancellation is reduced and the transverse flow progressively approaches the classical Ekman circulation
	- sensitivity of eddy circulation to synoptic changes in air-sea heat fluxes is felt down to 125 m depth; mesoscale dynamics dominate the cancellation effect 

## Discussion Meeting
- can be finished in a short time
- cooling of the eastern central pacific, while the land is heated 
  → pressure gradient increasing 
  → the wind goes north is strengthening
- **ERRIRE** captures the cooling trend with high accuracy compared to CMIP, high resolution