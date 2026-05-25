---
tags:
  - project/WMT
  - AMOC
  - AMOC/weakening
  - "#supervisor/robb_wills"
Last Eddited: 2025-12-01
---

# [[2026-05-25]] Benchmark
- Redo the density flux calculation for each grid, time and for every models
	- the new version is the same with the previous one in terms of the `Dh, Dw, Dtotal`, but haven’t been normalised by grid area `areacello`
	- the new output: `alpha_over_cw` and `Qh` (identical to the original data)
		- the idea is to decompose the response of density flux (last 50-year 4xCO2 means minus the first 500-year piControl mean (i.e., climatology))
		- 


# [[2026-05-19]] Discussion: Storyline
#presenter/Jingzhi_Zhang 
- ==**Significant difference in climatological sea ice coverage over the Labrador Sea between the LAB and ENA cluster**==
	- the ENA cluster (weak climatological AMOC_z, MOC_sfc and WMT_sfc) has substantially greater sea ice coverage over the Labrador Sea
	- This difference has been founded and discussed in Lin et al., 2023. 
		- They found negative sensitivity of climatological sea ice cover against AMOC_z climatology.
		- “Models with the abundant-ice-covered Labrador Sea in the mean state (i.e., weak AMOC climatology), the magnitudes of sea ice decline reach 10-20% in the first 5 years of 4xCO2. Projected sea ice loss between S10 (i.e.,strong AMOC climatology) and W10 (i.e., weak AMOC climatology) already has contrasting magnittudes in the first month”
		- “For model with strong mean state AMOC, with less sea ice and **stronger upward turbulent heat fluxes climatologically,** the weak sea ice decline leads to little change in net shortwave radiation and stronger suppression of the upward turbulent heat fluxes. Also, the surface warming penetrates to the subsurface due to the stronger climatological mixing (~={blue}i.e., in my case, stronger WMT=~) in the upper ocean. The subsurface warming and the corresponding density decrease drive the AMOC weakening at a leading time scale of 1-5 years”
- Then, ~={red}**what’s new in my research???**=~
	- metric: surface-forced WMT; surface-forced MOC
	- I also discussed the ENA regional WMT, even though the cluster is classified based on WMT over Labrador Sea
	- I found, models with higher dense climatological surface-forced WMT (SWMT) in the North Atlantic tends to experience greater dense SWMT reduction under 4xCO2 forcing, correspondingly larger decline in surface-forced MOC and AMOC in depth coordinate.
	- Now, can I explain why? in the first place, that higher climatological SWMT would leads to higher sensitivity of SWMT under CO2 forcing? Or should I?
	- the WMT equation: $$\begin{align} F(\sigma)&=\frac{1}{\Delta \sigma}\int\int_{\sigma}^{\sigma+\Delta \sigma}D(x,y,t)\;dA \\ D(x,y,t) &=\frac{\alpha(x,y,t)}{c_w}Q_H(x,y,t)-\beta(x,y,t)S(x,y,t)Q_F(x,y,t)\end{align}$$
		- where:
			- $\alpha$ is the thermal expansion coefficient calculated at each grid point for every month of output data, $c_w$ is the specific heat capacity of sea water (~constant)
			- $Q_H$ is the surface heat flux into the ocean
			- $\beta$ is the haline contraction coefficient also calculated at each grid point for each time
			- $S$ is the surface absolute salinity;
			- $Q_F$ is the freshwater flux
		- We have proved that **the heat-flux-driven density flux and corresponding WMT dominates its climatological magnitude and variability under 4xCO2 forcing**. Then, maybe check which variable’s variation dominates this heat-flux-driven density flux variability: $$\begin{align}D(x,y,t)&=D_T+D_W \\ D_T&=\frac{\alpha(x,y,t)}{c_w}Q_H(x,y,t)=\alpha'(x,y,t)Q_H(x,y,t) \\ \Delta D_T &\approx \Delta \alpha' Q_H+\Delta Q_H \alpha' + \Delta \alpha'\Delta Q_H \end{align}$$
			- I might need to show, for those models with higher climatological $Q_H$, the $\Delta D_T$ is also higher, with the expectation of higher $\Delta Q_H \alpha'$, in both Labrador Sea and Eastern North Atlantic region




# [[2025-11-13]] Discussion: Paper figures

## Suggestions for the figure modification

### **Figure 1**:
- zoom in to focus on 24-29 kg m-3
- try:
    - separately plot the cluster mean surface-forced AMOC climatology/mean response; Explicitly labeled the $\sigma_{AMOC_{max}}$ in the figure in AMOC panel and WMT distribution panel.
    - Or plot it with the WMT distributions, for which we can have a direct label of $\sigma_{AMOC_{max}}$ in the WMT distribution as well
- Remember the supplementary for all 14 WMT distributions in piControl, Response, (and maybe the 4xCO2)

### **Figure 2**:
- No need to use $\sigma_{max}$. Since it’s only used in the WMT to find the densest portion. Now:
- Simply find the maximum $AMOC_{surf}$ across the entire sigma space
- the WMT should focus on the $\sigma >\sigma_{AMOC_{max}}-0.5$
    - For the choice of $\sigma >\sigma_{AMOC_{max}}-0.5$, search for papers (e.g., Dylan’s paper) for support

### **Figure 3**:
- for the ‘grey’ part, clarifying the LAB and ENA remaining WMT (dark blue and dark orange)
- think of ‘percentage’
    - maybe normalise the WMT by region size? (the motivation: the ENA region is much bigger than the LAB region) → but accounts for the actual contributions to the total WMT, the size also matters, so maybe not

### Figure 4:
- make each scatter plots the same size, organising into a ‘triangular’ shape to highlight the hierarchy of WMT_response vs WMT_climatology
	- WMT_total
		- WMT_lab
			- WMT_lab,T
			- WMT_lab,S
		- WMT_ena
			- WMT_ena,T
			- WMT_ena,S

### Figure 5:
Mechanism plot:
- trace back to the surface density flux formula (see Oldenburg et al. (2021)).
- the surface-forced WMT at each density is calculated by integrating this surface density flux over all surface area in each density bin.

![[63A32032-D6D7-4E6D-A2B1-82988106A76B_1_105_c 2.jpeg]]