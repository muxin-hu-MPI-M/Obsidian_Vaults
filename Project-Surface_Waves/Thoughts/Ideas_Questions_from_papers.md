---
Type:
tags:
  - important_paper
  - wave/surface_wave
  - project/surfwaves
Last Eddited: 2026-08-05
---
# (Rühs et al., 2025) Non-negligible impact of Stokes
**Title**: Non-negligible impact of Stokes drift and wave-driven Eulerian currents on simulated surface particle dispersal in the Mediterranean Sea

## Wave-driven Eulerian currents
![[Screenshot 2026-08-05 at 14.31.50.png|center|666]]

> [!Quote] **Question**
> - Figure 3(a) shows wave-driven eulerian currents tend to decrease Lagrangian surface speed, related to the effect of anti-Stokes forces and a reduction in atmospheric momentum transfer through increasing surface roughness
> 	- **Q1: Which effect is more important/significant in terms of wave-driven Eulerian currents? Anti-Stokes effect or the wave-mediated momentum transfer**
> - The increase in Lagrangian velocity is as expected from the general seasonality of winds and surface wave activity, weakest in summer and strongest in winter. However, the strongest decrease in Lagrangian velocity is surprisingly weakest in fall and on average, slightly stronger in summer than in winter. Which means, in summer, surface drift speed decreases with wave coupling, dominated by wave-driven Eulerian currents
> 	- **Q: Why the decrease in Lagrangian velocity, due to anti-Stokes + wave-mediated momentum transfer, is stronger when the wave activity is weakest (e.g., summer)?**


# (Tamura et al., 2012) Stokes-induced mass flux
**Title**: The Stokes drift and wave induced-mass flux in the North Pacific
## Stokes induced mass transport
**Stokes-induced mass transport = Stokes transport**
“In linear wave theory, the wave induced-mass transport in Eulerian coordinate (equation (5)) is equal to the Stokes transport (depth integral of the Stokes drift velocity, equation (2))”: $$\mathbf{M}^w = \rho \int_{-\infty}^{\overline \zeta} \mathbf{\overline u}^s(z)\;dz$$
where
- $\overline \zeta$ and $\mathbf{\overline u}^s$ are wave-averaged components of surface elevation and velocity vector
- **Which means:** **One can directly estimate the wave induced-mass transport by the Stokes transport**
## Wave characteristics in Northern Pacific
- **Wave spectrum**
	- “Large mechanical wind energy is transferred to surface waves in the midlatitudes from 30 to 60 N [e.g., Wang and Huang, 2004], corresponding to strong surface wind fields associated with storm tracks.”
	- **The variations of wave spectrum are characterised by strong downshifting of the peak frequency due to non-linear wave-wave interactions** ($S_{nl}$).
		- Corresponding to the high windsea energy (strong high frequency) with lower spectral peak (also strong low frequency), the surface Stokes drift velocity and the e-folding depth becomes large in NP (high frequency contribute to surface Stokes; low frequency contribute to Stokes transport)
	 - **Therefore the wave-spectra are characterised by bi-modal shape**; stationary higher frequency peak with slowly upshifting spectral peaks at low frequency.
		 - Physically, this means waves in the NP is a mix of both high frequent wind waves and low frequent swells. Local differences in the NP:
			 - **At midlatitudes**: Ocean waves generated in the midlatitudes propagate far from storms and radiate eastward in the tropical and subtropical Pacific as swells. 
			 - **At lower latitudes**: especially in ITCZ, trade winds blowing constantly generate local windsea with background swells, sporadically coming from higher latitudes. Because spectral energy due to short waves is now dominant, surface Stokes become large but it drops rapidly with depth. Thus e-folding depth become small in the ITCZ regions.
- **Scales**
	- “spatial distributions of mean wave height and mass transport roughly correspond to the synoptic scale (~1000 km); 
	- divergence of wave induced mass flux corresponds to meso-scale features (100~500 km) (Tamura et al., 2012)
- **Wave-induced divergence/convergence”
	- “the corresponding wave-induced divergence and convergences produce vertical velocities of $O(5-10\;cm/day)$. Although these are weaker than values of $O(10-100\;m/day)$ often found in strong frontal zones, they are comparable to the annual and seasonal mean Ekman pumping/suction in the open ocean over the Gulf Stream and the Kuroshio” (Tamura et al., 2012)
	- “Wave-induced mass divergences and convergences cna rearrange the Ekman transport so as to cancel the wave effects”

## limitations of estimated surface Stokes drift
> [!Quote] Limitations of estimation from bulk wave parameter
> - in the paper, they discussed the estimated surface Stokes drift velocity and the corresponding e-folding depth from bulk-wave parameters
> 	- the result show the estimated surface Stokes drift velocity is significantly lowered than the real ones; while the estimated e-folding depth is significantly greater, and has a zonal variation, while the real should show meridional variations;
> 	- The discrepancy is explained by the predominance of swell propagation from higher latitude in case of bulk parameterisation, which only has contribution from the lower frequency component of the wave spectrum with no contributions from the higher frequencies
> 	- ~={green}Therefore, appropriate treatments of the wave spectrum are required for evaluating the surface Stokes drift and e-folding depth (Tamura et al., 2012).=~
> - **Q: Does my estimation of Stokes transport also has similar issues, with the lack of consideration of high-frequency component?**
> 	- **A**: According to (Breivik et al., 2014), our estimation of Stokes transport includes the contribution from the diagnostic high frequency spectral tail; however, it overestimates the transport on average by 17%

