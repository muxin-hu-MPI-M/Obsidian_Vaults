---
tags:
  - project/WMT
  - AMOC
  - AMOC/weakening
  - "#supervisor/robb_wills"
Last Eddited: 2025-12-01
---


# [[2026-05-28]] Clarification of notation

| variable                      | Description                                                                                                                                                                                                                                                           | Unit       |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| $AMOC_z$                      | Values of overturning stream-function in Atlantic sector at 35 N, 1000 m depth. Direct model output                                                                                                                                                                   | Sv         |
| $WMT$                         | Surface-forced Water Mass Transformation distributed over density (i.e., sigma) space.                                                                                                                                                                                | Sv         |
| $MOC_{\sigma}$                | Surface-forced overturning stream-function, a function of $\sigma$ and latitude                                                                                                                                                                                       | Sv         |
| $\sigma_{MOC_{\sigma}^{max}}$ | The $\sigma$ level of the maximum surface-forced AMOC founded north of 50 N in the Atlantic basin.                                                                                                                                                                    | kg m-3     |
| $IWMT$                        | Integrated $WMT$ over a certain $\sigma$ range: $(\sigma > \sigma_{MOC_{\sigma}^{max}}-0.5)$; It is used to quantify how much surface buoyancy forcing contributes to the formation of densest water masses that sink to form the deepest limb of surface forced AMOC | kg s-1     |
| $D$                           | Density flux. Has two component: (1) heat-flux driven density flux $D_h=-\alpha Q_h /c_w$ and (2) freshwater-flux driven density flux $D_w=-\beta S Q_w$                                                                                                              | kg m-2 s-1 |
- every annotation will have two values:
	- $\overline{X}^{\text{pi}}$: climatology, defined as 500-year time mean of piControl experiment. For simplification, use $X$ directly
	- $\overline{X}^{\text{4x}}$: last 50-year time-mean of abrupt-4xCO2 experiment
	- The “response” will be defined as: $\Delta X = \overline{X}^{\text{4x}} - X$ 
		- the response in heat-flux driven density flux $\Delta D_h$ will be decomposed into:
			- $\Delta D_h^Q = -\overline{\alpha'}^{\,pi}\Delta Q_h$
			- $\Delta D_h^\alpha =-\Delta\alpha'\,\overline{Q_h}^{\,pi}$
			- $\Delta D_h^{nonlinear}= -\Delta\alpha'\Delta Q_h$
			- $\Delta D_h^{cov}=\Delta D_h - \left( \Delta D_h^Q + \Delta D_h^\alpha + \Delta D_h^{nonlinear} \right)$ → residual terms, measuring the change in the temporal covariance between $\alpha'$ and $Q_h$, including seasonal and interannual co-variability


# [[2026-05-27]] Discussion with Hongdou
#presenter/Hongdou_fan 
- Calculate the corresponding response in WMT for each decomposed density flux terms (delta_dh_Q, delta_dh_alpha), to see if each term could influence the denser sigma space
- plot the regions with active WMT in the densest portion (that can reach to the deepest depth)
	- or, plot the spatial pattern of WMT at certain density bin (the colour would be the Sv value for each grid) → select denser sigma
- Figure 3, maybe reduced with no climatological AMOC_z compares with climatological MOC

# [[2026-05-25]] Benchmark
- Redo the density flux calculation for each grid, time and for every models
	- the new version is the same with the previous one in terms of the `Dh, Dw, Dtotal`, but haven’t been normalised by grid area `areacello`
	- the new output: `alpha_over_cw` and `Qh` (identical to the original data)
		- the idea is to decompose the response of density flux (last 50-year 4xCO2 means minus the first 500-year piControl mean (i.e., climatology))
- The mathematical expressions for four components should be: $$D_h(x,y,t) = -\alpha'(x,y,t)Q_h(x,y,t),$$
   - where: $$\alpha'(x,y,t)=\frac{\alpha(x,y,t)}{c_w}.$$
   - define the climatological mean over each experiment:
     - $\overline{D_h}^{\,pi}=-\overline{\alpha' Q_h}^{\,pi},$ 
     - $\overline{D_h}^{\,4x}=-\overline{\alpha' Q_h}^{\,4x},$
   - the actual response: $$\Delta D_h=\overline{D_h}^{\,4x}-\overline{D_h}^{\,pi}=-\overline{\alpha' Q_h}^{\,4x}+\overline{\alpha' Q_h}^{\,pi}.$$
   - Now decompose using climatological mean fields: $\overline{\alpha'}^{\,pi},\quad \overline{Q_h}^{\,pi},\quad \overline{\alpha'}^{\,4x},\quad \overline{Q_h}^{\,4x}.$
   - Define:
     - $\Delta \alpha'=\overline{\alpha'}^{\,4x} -\overline{\alpha'}^{\,pi},$
     - $\Delta Q_h'=\overline{Q_h'}^{\,4x} -\overline{Q_h'}^{\,pi},$
   - The three mean-field decomposition terms are: $$\begin{align} \Delta D_h^Q &= -\overline{\alpha'}^{\,pi}\Delta Q_h, \\ \Delta D_h^\alpha&=-\Delta\alpha'\,\overline{Q_h}^{\,pi}, \\ \Delta D_h^{nonlinear}&= -\Delta\alpha'\Delta Q_h.\end{align}$$
   - Because the actual density flux uses the time mean of the product: $$\overline{\alpha'Q_h} \neq \overline{\alpha'}\,\overline{Q_h},$$, ==**there is an additional covariance, which measures the change in the temporal covariance between $\alpha'$ and $Q_h$, including seasonal and interannual co-variability==**: $$\begin{align}\Delta D_h^{cov}&= \Delta D_h - \left( \Delta D_h^Q + \Delta D_h^\alpha + \Delta D_h^{nonlinear} \right) \\&=-\left[\overline{\alpha'Q_h}^{\,4x}-\overline{\alpha'}^{\,4x}\overline{Q_h}^{\,4x}\right]+\left[\overline{\alpha'Q_h}^{\,pi}-\overline{\alpha'}^{\,pi}\overline{Q_h}^{\,pi}\right].\end{align}$$

- The decomposition results show:
	- If focusing on the annual mean climatology (directly taking the time mean for both piControl and 4xCO2), the temporal covariance term is very large, can take up to 40% of the total variability
	- If focusing on the winter (either defined as JFM, or DJF), the temporal covariance term reduced

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