---
tags:
  - Meeting
  - project/surfwaves
  - "#presenter/Noel_Gutierrez-Brizuela"
  - "#presenter/Nils_Brüggemann"
Last Eddited: 2026-01-13
---

# [[2026-05-22]]
## Regular meeting with Nils
#presenter/Nils_Brüggemann 
From last meeting:
- Stokes reconstruction, Non-collinear case:
	- find to have **unique solution** if non-collinear. This is because the vector equation is just a 2×2 linear system in a non-collinear basis
		- a **physically admissible positive solution** only if both magnitudes are positive (constraint)
		- That positive solution exists **if and only if surface Stokes velocity vector $\mathbf u_{st}$ lies inside the convex cone spanned by two direction​ unit vector.** If it lies outside that cone, then the unique signed solution necessarily has one negative coefficient, and the positive-only decomposition is impossible.
- ==**Therefore, each grid point and time step is assigned to one of 3 cases:**==
	- **Collinear**: fallback to non-decomposition
	- **Non-collinear, admissible decomposition**: if surface Stokes velocity vector $\mathbf u_{st}$ lies inside the convex cone spanned by two direction​ unit vector.
	- **Non-collinear, inadmissible decomposition**: if surface Stokes velocity vector $\mathbf u_{st}$ lies outside the convex cone.
		- This does not imply that one wave component is physically absent. Rather, it indicates that ~={red}the bulk ERA5 partition directions and the total ERA5 surface Stokes drift vector are mutually inconsistent under a two-direction, non-negative closure=~.
			- Such inconsistency can arise because total swell may combine several swell systems, because partition-mean directions are bulk energy-weighted summaries, or because the surface Stokes drift is computed from the full spectrum rather than from only two idealised directional components.
- For the collinear and inadmissible cases, one alternative is to apply the total-sea fallback reconstruction using the ERA5 total surface Stokes drift and the bulk total-sea transport estimate.
	- This fallback is introduced to avoid spatially intermittent missing forcing in ICON, not to imply that the total-sea profile is physically equivalent to a successful wind-sea/swell decomposition
	- The fallback therefore provides a practical lower-information estimate in regions where the available bulk wave diagnostics do not support a physically admissible partitioned reconstruction.
- A 1-month test on 2024 finds relative percentage of 3 cases: 
	- if the collinear threshold set to 10 degrees
		- decomposition fail: ~35%
			- collinear: ~13%
			- ~={red}non-collinear but negative magnitude: ~22%=~
		- decomposition success: ~65%
	- if the collinear threshold set to 5 degrees
		- decomposition fail: ~32%
			- collinear: ~6%
			- ~={red}non-collinear but negative magnitude: ~26%=~
		- decomposition success: ~68%
	- Tested over January-Feb and June-July, no clear difference
	- ![[Screenshot 2026-05-18 at 14.39.06.png]]
- The decomposition is attractive from a wave-physics perspective, but risky in a climate-scale research:
	- The method is not globally applicable. It mixes 2 different physical closures in on global forcing product. One key question emerges: whether a climate signal comes from Stokes forcing itself, from the decomposition, from fallback regions, or from discontinuities between reconstruction regions
	- The later analysis cannot cleanly attribute global responses to wind sea versus swell.
	- To sum up, it adds a second, partly unresolved methodological question on top of the main research question: the potential climate relevance of Stokes forcing.
- I propose: maybe just use the **global fallback** total-sea reconstruction, assuming the Stokes transport in the same direction of surface Stokes velocity.



# [[2026-05-20]]
## Regular meeting with Noel
#presenter/Noel_Gutierrez-Brizuela 
- introducing the decomposition with higher complexity, but cannot apply it to the global ocean, will make the analysis more complex. because the fallback total-sea profile is **NOT** physically equivalent to the successfully decomposed profile
	- The fallback total-sea profile is most defensible when wind sea and swell propagate in similar directions. It is less informative for opposing or strongly mixed systems where **bulk ERA5 partition directions and the total ERA5 surface Stokes drift vector are mutually inconsistent under a two-direction, non-negative closure**
- Maybe use the total-sea profile globally (median complexity), which assumes the Stokes transport estimated from bulk wave parameters is in the same direction of surface Stokes velocity
- This method is already enough when the focus of the 

# [[2026-05-13]]
## Regular meeting with Nils
#presenter/Nils_Brüggemann 
Paper reading:
- ~={blue}(Fujiwara et al., 2026)=~ ***“Wave-driven ocean currents: how the conservative effects of Stokes transport induce large-scale currents”***
	- **When introducing the Stokes drift into the model, need to consider the diagnosed vertical component of Stokes drift** (called pseudo-velocity, see Eq. (2)) $w^{s}(z)=-\nabla \cdot \int_{-H}^{z} \mathbf u^{s}\;dz’$,  where $z=-H(x,y)$ is the bottom profile. 
		- The Stokes drift velocity obtained from the traditional definition is not necessarily incompressible.
		- Here the incompressibility of the Stokes velocity is implicitly assumed: $\nabla \cdot \mathbf u^{s} + \partial_z w^{s}=0$
	- **Need to consider some boundary conditions.** see details in Eq. (5)-(8)
	- Their study implement the monochromatic Stokes drift approximation, as the consideration of wave-induced Ekman pumping suggests that the vertically integrated Stokes transport characterises the resulting current response. No need to have sophisticated approximation
	- **Idealised simulation with localised Stokes drift is imposed on a ~={red}Linearised case**=~ (High Rossby number, neglect the Vortex Force term → neglect the Stokes advection of momentum)
		- **Flat bottom:** Lagrangian field exhibits a dipole circulation pattern on either side of the Stokes forcing. Transient water displacement happens, transient “Stokes-Ekman pumping” happens, but near zero long term (>= 72 h) changes in geostrophic current after zero Stokes
		- **Slope bottom**: has net long term irreversible changes in geostrophic current after zero Stokes through the generation of topographic Rossby waves
During meeting:
- Stokes reconstruction: find the cases that is non-collinear, but cannot yield physically constrained decomposition (i.e., enforcing positive magnitudes)
	- Check the percentage and spatial pattern of this non-collinear but non-admissible cases

# [[2026-05-12]]
## Regular Meeting with Noel
#presenter/Noel_Gutierrez-Brizuela 
- Nobu’s paper suggests a quasi-hydrostatic, or “wavy hydrostatic””

# [[2026-05-07]]
## Discussion with Nobu
- When introduce the Stokes profile, need to consider diagnosing vertical component of Stokes drift (called pseudo-velocity), which implicitly satisfy: $\nabla \cdot \mathbf u^{s} + \partial_z w^{s}=0$
	- The equation: $$w^{s}(z)=-\nabla \cdot \int_{-H}^{z} \mathbf u^{s}\;dz’$$
	- And apply boundary condition:
		- at the upper boundary $z=\eta$: $\partial_t \eta = -\nabla \cdot \int_{-H}^{z} (\mathbf u+\mathbf u^{s})\;dz’$, and pressure $p=0$.
		- at the bottom boundary at $z=-H$, a no-normal-flow boundary condition is applied for the Eulerian velocity: $w+\mathbf u \cdot \nabla H=0$, as well as the Stokes velocity $w^s+\mathbf u^s \cdot \nabla H=0$
- **neglect the Stokes shear force term will introduce artificial source/sink of potential vorticity**
- preserve all three terms can make the system consistent
	- Lagrangian advection
	- Lagrangian Coriolis
	- Stokes shear terms
- It’s rather easier to implement the full three terms. Using Eq. (1) in ~={blue}(Suzuki & Fox-Kemper, 2016)=~, which is also the expression that the ICON used
	- ~={red}Need to check if **hydrostatic formulation** ensures the consistency in potential vorticity=~
		- Update: yes, consistent when considering quasi-hydrostatic
		- the term $u_j^L\cdot \nabla u_j$ will be cancelled out by summation between the RHS and LHS of the formula Eq. (1), which leaves the Suzuki’s formulation with the (1) Lagrangian advection, (2) Lagrangian Coriolis and (3) Stokes shear force term
		- The Suzuki’s alternative formula is consistent in potential vorticity
# [[2026-05-05]]
## ERA5 forcing on ICON: talk with Helmuth
#presenter/Helmuth_Haak 
Four stages of forcing ICON with ERA5 atmosphere + waves:
1. Download the ERA5 wave data to the Levante using the script from Helmuth
2. Modify the python reader script (recipe), calculate the Stokes transport (parameter for reconstruction) in the python reader. Provide transport and surface velocity to Yak coupler.
3. Modify the Yak coupler. The ERA5 wave fields have coarser resolution than normal ERA5 atmospheric fields. contact Moritz Hnnke from DKRZ.
4. Branch off from ICON-main, modifying the ICON code

Steps:
- Download the ERA5 wave data:
	- copy the python virtual environment requirement to local directory: 
	  `cp /work/mh0033/m211054/projects/ecmwf_download/data1/requirements.txt.`
	- create the virtual environment:
	  `python3 -m venv ${DDIR}/.venv`
		`source ${DDIR}/.venv/bin/activate` 
		`python -m pip install -r requirements.txt` 
		`source ${DDIR}/.venv/bin/activate`
	- create `.cdsapirc` in the HOME directory, copy the url and key information from the ECMWF website account (see details in the website too)
	- copy the script for downloading, and adjust for desired variables, save directory:
	  `cp /work/mh0033/m211054/projects/ecmwf_download/data1/00_download_era5_1971.sh.`
	- use “screen” to run it, remember the which Levante(0-6) to use the screen
	  `screen -r`
		`ctrl-a d`
- Modify python reader recipe:
	- ICON reads the ERA5 atmospheric forcing through **(1) Python reader**, the Python reader will push the ERA5 fields to the **(2) Yak Coupler** to do interpolation, and finally feeds to the **(3) ICON execute model**
	- Calculate the Stokes transport in the python reader
	- Feed the Stokes transport and direct output of surface Stokes velocity to the Coupler to do interpolation (which interpolation method?)
	- Reconstruct the profile in the ICON code (where the vertical information is provided)

# [[2026-04-30]]
## Eddy-wave meeting summary
### Surface wave-induced mass transport and ocean current responses
#presenter/Yasushi_Fujiwara
- wave-current interaction
- remotely generated swells can influence the local wave field
- Interest: how the wave-induced mass transport modify the ocean current?
	- short-time scale current response
- Particle trajectory at second order: Stokes drift
	- the first order is the orbital movement (no net transport)
	- Lagrangian mean velocity = Eulerian-mean + Stokes 
- Stokes transport / Ekman transport can reach more than 30% outside tropics → "❓transient”
- Stokes drify, by definition, not 100% incompressible, however, people assume it but introducing the diagnosed vertical Stokes drift
- ==**Short-timescale current responses → group-induced return flow**==
	- inhomogeneous wave field, waves patterns generally follow the wind field
	- waves are “groupy”
	- Eulerian return flow response (van den Bremer and Taylor 2015) for the wave group
		- convergence.divergence of Stokes transport at group edges raises lowers te water surface
		- Group-bound forced response: potential flow from group from one edge to another
	- the idealised simulation
		- the return flow is almost vertically uniform → barotropic
		- Barotropic Eulerian velocity immediately cancel the Stokes mass transport
	- wave-resolving simulation vs wave-averaged simulation, show same solution
- ==**long-timescale (subinertial) current responses → rotation induced return flow**==
	- CSF, rotation-induced anti-Stokes flow (Eulerian velocity) → make the mean Lagrangian velocity is zero
	- Coriolis force on Stokes drift, vertical convergence of Reynolds stress
	- time-dependent inhomogeneous forcing?
		- inhomogeneous Stokes forcing on a rest ocean induced a dipole-like pattern Lagrangian velocity response → geostrophic circulation
		- The Eulerian velocity almost cancel the Stokes, but some residuals that shows in the Lagrangian velocity
	- ~={red}**Net circulation selectively occurs for small-scale wave inhomogeneity**=~
	- Realistic topography: wave driven currents remains even afer cyclone passes, fluctuations propagates southward as topographic Rossby waves
	- Wave-forcing effects (e.g., mass transport) modifies the wind-driven effects by 1-12%

### In situ observation of wind-wave interactions
#presenter/Marc_Buckley
- longwave travel faster, and even faster than the very surface wind (since wind slows down below critical layer)
	- **critical layer:** where wind speed matches with the phase speed of surface wave
- traditional parameterisation: drag coefficient from observation
- observation of the “**sheltering layer**”; below by the high viscosity detach the wave from the boundary layer
	- air goes down and accelerating on the wind-facing side (crest), air goes up and decelerating on the other side (backside, trough)
	- the sheltering mechanisms dominates the growth of short young waves
	- Phase shift w.r.t height on the either side of the wave crest

### Direct numerical simulation of surface wave-induced forces
#presenter/Carsten_Eden
- understanding of wave effects in LES model mostly rely on Craik-Lebovitch forcing
- but no observation evidence for Coriolis-Stokes or vortex force






# [[2026-04-29]]
## Talk with Yasushi Fujiwara
- In his idealised simulation with localised Stokes drift (linearised case)
	- Focus on the high Rossby number, which cancels the Vortex force term in the below WAB equation $$\partial_t \mathbf u + (\mathbf u \cdot \nabla) \mathbf u + f \times \mathbf u^{L} = b + D^U - \nabla (p+\frac{1}{2}|\mathbf u^{L}|^2 - \frac{1}{2}|\mathbf u|^2) - (\nabla \times \mathbf u)\times \mathbf u^{s}$$
		- because the VF term incorporates quadratic terms, which is neglected in the linear case
	- The VF term can be decomposed into two other terms: $$-(\nabla \times \mathbf u)\times \mathbf u_s = u^s_j \nabla u_j - (\mathbf u^s \cdot \nabla)\mathbf u$$
	- Which is the “Stokes-Eulerian gradient term” and the “Stokes momentum advection” term
	- In ~={blue}(Fujiwara et al., 2026)=~, the entire VF term is neglected, ~={red}hence the Stokes momentum advection is also neglected=~
- In my intended experiment, which I will ONLY neglect the Stokes shear term in the below alternative from of WAB equation: $$\partial_t \mathbf u + (\mathbf u^{L} \cdot \nabla) \mathbf u = -f\times \mathbf u^{L} + b + D^{u} - \nabla p - u_j^{L}\nabla u_j^{s}$$
	- The Stokes shear force term $- u_j^{L}\nabla u_j^{s}$ is the combination of Stokes-Eulerian-gradient term $u^s_j \nabla u_j$ with the gradient of pressure correction term $- \nabla (\frac{1}{2}|\mathbf u^{L}|^2 - \frac{1}{2}|\mathbf u|^2)$
	- In my experiment, ~={red}the Stoke advection of momentum will be preserved=~, which in principal has additional Stokes advection of momentum compared to the linearised case from ~={blue}(Fujiwara et al., 2026)=~, therefore generating different result
- ==**POTENTIAL PROBLEM**==:
	- Check whether removing Stokes shear force is still conserving the relative vorticity


# [[2026-04-24]]
## Meeting: Jean, Nils, Mikhail, Carsten, Chris
- comparing tests:
	- force the ecWAM and ICON-wave with ERA5 atmosogere (i.e. winds), no ocean.
	- force the ecWAM and ICON-wave with ERA5 atmosphere and OR6 oceans (which are consistent since OR6 is forced by ERA5)
- the ICON-wave calculate the actual Stokes drift profile use the **Kenyon integration**: $$ \begin{equation}\mathbf{v}_s(z) = g \int_{-\infty}^{\infty} F(\mathbf{k}) \frac{\mathbf{k}}{\omega} \left[ \frac{2k \cosh 2k(z + h)}{\sinh 2kh} \right] \mathrm{d} \mathbf{k} \tag{1}\end{equation} $$ see details in ~={blue}Kenyon, 1969 and Breivik et al., 2014=~
	- This ensures **no deep-water limit** to enable representations of waves in intermediate and shallow water depth, which is important for operational forecast
- **Wave-mediated stress changes** (i.e., modifications in momentum flux) are expected to exert a stronger influence in the tropics.
	- Surface waves modulate how atmospheric momentum is transferred into the ocean, thereby influencing upper-ocean mixing and the resulting temperature variability.
	- As the tropics are regions of persistent large-scale convection and ascent, these impacts may be further communicated to the overlying atmosphere.
- When add the Stokes drift velocity into the model, one need to consider the divergence of Stokes drift
	- the usual treatment is to have a “negative” vertical Stokes drift velocity to insure the divergence-free

# [[2026-04-23]]
## Meeting with Jean Bidlot
#presenter/Jean_Bidlot
- For my “Wave-modified Lagrangian transport” project, strong motivation
	- ==**The analysis should prioritise regional variability and shorter time scales**==, rather than global mean behaviour. Long-term averages (e.g., 30-year climatologies) tend to smooth out a large fraction of swell-related signals, since swell events are intermittent and exhibit strong seasonality. As a result, the remaining wave effects in such averages are dominated by wind-wave contributions, which are more directly tied to large-scale, persistent wind patterns. This motivates a shift toward:
		- regional diagnostics
		- event-based or seasonal analysis  
		to better capture the dynamical impact of swells.
	- ==**The reconstruction of Stokes profile needs careful consideration**==
		- clarify the assumptions: see ~={blue}(Breivik & Christensen, 2020)=~
		- The reconstruction must use the ~={red}*mean wave period derived from the first spectral moment*=~ to ensure consistency with theory.
		- It’s better to use the profile based on Phillip’s spectrum, introduced in ~={blue}(Breivik et al., 2016)=~. This profile specifically better captures the high frequency contribution at the surface
		- Limitations in representing swell contributions
			- The separation between wind waves and total swell is statistically convenient, but introduces important limitations:
				- “Total swell” often consists of **multiple swell systems**, each with distinct directional–frequency (2D) spectral characteristics and narrow spectral peaks.
				- Representing these systems using **bulk (mean) wave parameters** inevitably loses spectral detail.
				- The separation method proposed by **Breivik & Christensen** may effectively introduce _artificial wave components_, which could be a point of criticism.
			- A possible justification is that:
				- from a **statistical perspective**, mean wave parameters can be interpreted as representing the **aggregate effect** of multiple swell systems within a grid cell
				- however, this assumption and its implications should be clearly acknowledged and discussed
	- ==**Forcing the ICON model:**==
		- ~={red}ERA5 Wave+ atmosphere → force the ICON-o=~:
			- Jean’s comment: It ensures internal consistency within the forcing fields, but not consistency with the simulated ocean (or atmosphere, if coupled).
			- if uses the ERA5 Wave → force the ICON-a/o, waves are not consistent with ICON winds, strong wave-wind mismatch
	- ==‼️**Potential collaboration**==
		- contact: jean.bidlot@ecmwf.int
		- **topic**: *Comparing reconstructed Stokes drift profile with “true” profile integrated from full 2D spectrum*
			- Reconstruct the Stokes profile using wave parameters through a hierarchy of approximations
				- Monochromatic profile (see ~={blue}Breivik et al., 2014=~)
				- Exponential integral profile (see ~={blue}Breivik et al., 2014=~)
				- Profile based on Phillips spectrum (see ~={blue}Breivik et al., 2016=~)
			- compare these reconstructions to the reference value calculated by integration of wave spectrum field (the “true” value by definition)
		- Need to determine the depth $z$ space (the deepest depth, and the resolution), and provide to Jean
			- Maybe have a look over the e-folding depth scale? or inverse depth scale
			- determine a strategy to decide the depth space
		- Jean will provide the simulation with the (1) true value; and (2) wave parameters for reconstruction
		- ~={red}*The comparison could be served to a new paper (if large difference), or a support background information (in Appendix) that the reconstruction is okay in the wave-modified Lagrangian transport project*=~


# [[2026-04-22]]
## Regular meeting with Nils
- Reconstruction of Stokes drift for wind waves and swells separately: possible
- Literature reading:
	- What has been done?
		- “~={blue}**McWilliams & Restrepo (1999)**=~ used a wind climatology to assess the impact on the general circulation from adding the Coriolis-Stokes and vortex forces, as well as Stokes drift to the tracer advection equation. They found significant wave effects amounting to up to 40% of the wind-driven Ekman transport in extra-tropics ”
			- Relations for the Ekman and Sverdrup transports as functionals of the surface wind stress apply to the total Lagrangian-mean transport, not the Eulerian-mean, where the Stokes transport is a component. This implies that these Eulerian-mean transports have a Stokes-canceling component, albeit with a different depth profile.
			- Estimates based on assumption that surface stress and gravity wave spectrum are in equilibrium with the surface wind (→ which isolates wind waves!!!) suggest wave effects are more significant at higher latitude. The Stokes transport is a significant fraction of the Ekman transport
		- “The past decades have shown that waves play a significant role in the upper ocean layer through enhanced mixing and significant alteration of the momentum budget of the ocean.” ~={blue}(Bremer and Breivik, 2019)=~”
		- “As wave models become incorporated into these coupled model systems, it seems likely that the Stokes drift will be found to play a significant role in the distribution and climatology of sea ice as well as being an essential mixing process in the the wider climate system ~={blue}(Bremer and Breivik, 2018)=~”
	- What’s region of interest? 
		- “The Coriolis-Stokes force is of significance for ocean modelling outside the tropics where the wave field is mostly dominated by swell (McWilliams & Restrepo (1999))” - ~={blue}(Bremer and Breivik, 2018)=~
		- “The CL vortex force ($u^s\times(\nabla \times u^E)$ in Suzuki’s paper, Eq. (2)) is of greatest importance when modelling near shore, shallow-water conditions because of the strong shear in Eulerian currents. See Uchiyama et al. (2016) and Warner et al. (2010)” - ~={blue}(Bremer and Breivik, 2018)=~
			- The role of the CL vortex force in Langmuir turbulence is however important throughout the world’s oceans
- Comment from Nils:
	- Keep literature reading, discuss the idea with Jean Bidlot, ask for feedback
	- will determine the topic next week!!!!
	- Reconstruct the **hourly** global Stokes profile **by my own**, as the ERA5 forcing to ICON is in hourly time step by default
	- For using **pyicon** in DWD icon grid, change the “cartesian” related variables to the below block.
		- Can change the `pyic.convert_tgrid` to a new version which reads the `cell_cart_vec, vert_cart_vec, edge_cart_vec, edge_prim_norm` like below (need modification)
		- then, can save the converted ds_IcD to a new file for later usage.
		- Can easily calculate the grad and div.
		- related to the previous difficulties recorded in [[ICON_Data-process_Tips#Atmospheric grid (r2b5 from DWD) → impossible!!]]. This time, it is possible!!!
	```python
	  # --- coordinates
       #GB: at DWD no cartesian info in grid files --> calculate
       clon = f.variables['clon'][:]
       clat = f.variables['clat'][:]
       vlon = f.variables['vlon'][:]
       vlat = f.variables['vlat'][:]
       elon = f.variables['elon'][:]
       elat = f.variables['elat'][:]
       elon_pn = f.variables['zonal_normal_primal_edge'][:]
       elat_pn = f.variables['meridional_normal_primal_edge'][:]
       self.cell_cart_vec = np.ma.zeros((self.clon.size,3), dtype=self.dtype)
       self.cell_cart_vec[:,0] = np.cos(clat[:])*np.cos(clon[:])
       self.cell_cart_vec[:,1] = np.cos(clat[:])*np.sin(clon[:])
       self.cell_cart_vec[:,2] = np.sin(clat[:])
       self.vert_cart_vec = np.ma.zeros((self.vlon.size,3), dtype=self.dtype)
       self.vert_cart_vec[:,0] = np.cos(vlat[:])*np.cos(vlon[:])
       self.vert_cart_vec[:,1] = np.cos(vlat[:])*np.sin(vlon[:])
       self.vert_cart_vec[:,2] = np.sin(vlat[:])
       self.edge_cart_vec = np.ma.zeros((self.elon.size,3), dtype=self.dtype)
       self.edge_cart_vec[:,0] = np.cos(elat[:])*np.cos(elon[:])
       self.edge_cart_vec[:,1] = np.cos(elat[:])*np.sin(elon[:])
       self.edge_cart_vec[:,2] = np.sin(elat[:])
       self.dual_edge_cart_vec = np.ma.zeros((self.elon.size,3), dtype=self.dtype)
       #GB: this is not used anywhere so far, so we leave it as zero
       #self.dual_edge_cart_vec[:,0] = f.variables['edge_dual_middle_cartesian_x'][:]
       #self.dual_edge_cart_vec[:,1] = f.variables['edge_dual_middle_cartesian_y'][:]
       #self.dual_edge_cart_vec[:,2] = f.variables['edge_dual_middle_cartesian_z'][:]
       self.edge_prim_norm = np.ma.zeros((self.elon.size,3), dtype=self.dtype)
       self.edge_prim_norm[:,0] = - elon_pn[:]*np.sin(elon[:]) - elat_pn[:]*np.sin(elat[:])*np.cos(elon[:])
       self.edge_prim_norm[:,1] =   elon_pn[:]*np.cos(elon[:]) - elat_pn[:]*np.sin(elat[:])*np.sin(elon[:])
       self.edge_prim_norm[:,2] =   elat_pn[:]*np.cos(elat[:])
	```

# [[2026-04-15]]
## Meeting with Nils & Nobu: Stokes-induced Lagrangian transport
#presenter/Nils_Brüggemann #presenter/Nobushiro_Suzuki 
- run quick test on ***Oceananigans*** using Julia?
- **The research question is too general**:
	- Need to specify more
	- literature reading for background information
		- what has been done? been discussed?
		- What’s might be the focused region?
# [[2026-04-07]]
## Discussion with Nobu:
#presenter/Nobushiro_Suzuki 
also see the previous discussion: [[Regular-Meeting_Note#Comment from Nobu]]
- **Stokes advection and the Stokes-Coriolis cannot be separated**
	- This is because once considered the Stokes advection term, it describes dynamics in the Lagrangian framework
	- Hence, Coriolis effect must acts on Lagrangian velocity as well, otherwise the parcel is advected by Stokes drift but Earth’s rotation does not feel that motion, which is physically inconsistency
- **However, one can ignore the Stokes shear force**
	- As it represents interactions between the Eulerian mean flow and gradient of Stokes drift (mainly its vertical gradient), which transfers energy between waves and turbulence
	- Stokes shear mostly affect:
		- Langmuir turbulence
		- upper-ocean small scale mixing 
- **When considering the Stokes advection + Stokes-Coriolis, but ignoring the Stokes-shear term is acceptable** 
	- this idealised setting still keeps the minimal consistent Lagrangian dynamics
## Discussion with Noel:
- The idea ([[Proposal-Large_scale_dynamical_impact_of_Stokes_drift#Isolating the Large-Scale dynamical impact of Stokes drift]]) is cool
- **How to implement the Stokes drift into the model?**
	- **add the depth-mean Stokes drift value to the top layer** → good approximation, easiest
		- At each time step, if the return flow happens also at the same layer, or the layer with identical tracer value (e.g., potential temperature). The net effect would be almost entirely cancelled
	- **retrieve the full velocity profile in the depth space, add the velocity accordingly** → more consistent, requires more work on interpolating the depth coordinate
- **For the analysis afterwards, we can get:**
	- the model output for calculating the OHT, wind stress
	- Then, also compute the Stokes transport of heat from the prescribed Stokes drift, and the Stokes-mediated wind stress component
	- The “residuals” as the system’s response

# [[2026-04-01]]
## ICON-wave focus group: meeting
- the wave-current interaction part is finished
	- simple test: constant current field (Gaussian-like)
	- physical implementation is ready, but rather the test part
	- wave-current: how important it is? comparing wave to wave-current
		- wave height adjustment (i.e., subtraction): ~20 cm in the coarser resolution (r2b4 ~1/2 degree resolution). 
		- Strong current in Southern ocean, you don’t see many difference? Need to have spin-up to set up current system in Southern ocean (ocean starts from rest)
		- **current refraction**: components? need to look over test book!!!!
			- Chris: wave-action spectrum spread
	- next step: run r2b7 with spinned up ocean
## Comment for modifying Stokes advection:
- if only add the Stokes into tracer equation, **the PV is not consistent since the PV incorporates the momentum equation**
- The add of Stokes drift into the ICON model is feasible and possible.
### Isolating the Lagrangian transport effect of surface wave
Surface gravity waves generate a Stokes drift that contributes to the Lagrangian motion of water parcels in the upper ocean. $$\mathbf u_{L}=\mathbf u_E + \mathbf u_s $$
Where $\mathbf u_E$ is the Eulerian velocity and $\mathbf u_s$ is the Stokes drift induced by surface waves.

However, most ocean and climate models neglect the Stokes transport component and assume that large-scale tracer transport is governed solely by the Eulerian circulation. As a result, the potential contribution of wave-induced Lagrangian transport to upper ocean tracer redistribution is not explicitly represented.

To investigate the possible climate relevance of this missing transport pathway, this study will incorporate the diagnosed Stokes drift fields derived from wave reanalysis products into the advection operators of the ocean model: $$\frac{D \phi}{D t}+(\mathbf u_E + \mathbf u_s)\cdot \nabla \phi = ...  $$
where $\phi$ represents variables such as mass, momentum, and tracers that are transported by the effective Lagrangian velocity. In this configuration, Stokes drift contributes directly to the transport of ocean properties while the model continues to solve the full momentum and mass conservation equations.

This experiment design isolates the transport pathway associated with surface waves, while allowing the coupled ocean-atmosphere system to dynamically adjust through pressure gradients, sea-surface height changes, and Eulerian return flows. Any perturbation introduced by the imposed Stokes drift can therefore induce circulation adjustments within the resolved ocean dynamics.

The experiment does not attempt to represent the full wave-current interaction dynamics described by the wave-averaged momentum equations. In particular, wave-induced momentum forcing terms such as Coriolis-Stokes forcing are not explicitly included. Instead, the objective is to isolate the role of wave-induced Lagrangian transport as an additional transport pathway.

A key physical motivation for this design is that compensation of transport perturbations in the ocean does not occur instantaneously. Adjustment of the ocean typically involved processes operate across various spatial and temporal scales and exhibit regional variability. Consequently, the introduction of wave-driven Lagrangian transport may alter surface tracer pathways and upper-ocean heat redistribution before the ocean circulation fully compensates the imposed perturbation, and may lead to a residual tracer transport that persists after large-scale Eulerian compensation has developed, particularly in regions where dynamical adjustment is slow.

The present configuration therefore provides a controlled framework to test how the ocean circulation dynamically adjusts to wave-driven Lagrangian transport. The imposed Stokes drift introduces an additional transport pathway, to which the ocean circulation dynamically adjusts through pressure gradients, sea-surface height changes, and Eulerian return flows. Because the full wave momentum dynamics are not represented, the resulting circulation changes should be interpreted as the system’s response to the imposed transport perturbation rather than the true dynamical pathways of wave-current interaction. Nevertheless, this framework allows a direct assessment of **how much of the imposed Stokes transport is effectively compensated by the circulation adjustments and how much contributes to residual tracer redistribution.** Quantifying the resulting tracer transport therefore provides ~={red}**a measure of the potential climate relevance of wave-induced Lagrangian transport in an idealised setting**=~. In this way, the experiment evaluates whether this additional transport has the potential to influence the coupled climate system, even when allowing the ocean circulations to dynamically adjust to the imposed perturbation. The key question addressed in this study is whether wave-driven Lagrangian transport can generate a persistent residual tracer redistribution after the large-scale ocean circulation dynamically compensates the imposed transport perturbation.

#### Summary
- **Experimental design:**  
    The idealised experiment is designed to isolate the transport pathway associated with Stokes drift. Specifically, the diagnosed Stokes drift is added to the Eulerian velocity to form an effective Lagrangian velocity that advects mass, momentum, and tracers with water parcels.
- **Dynamical adjustment allowed:**  
    The coupled ocean–atmosphere system is allowed to dynamically adjust to the imposed transport perturbation through pressure gradients, sea-surface height changes, and Eulerian circulation responses, ensuring that the overall mass balance of the system is maintained.
- **Idealised representation of wave effects:**  
    The experiment does not attempt to represent the full wave–current interaction dynamics described by the wave-averaged momentum equations. Consequently, several wave-induced dynamical processes are not included, such as Coriolis–Stokes forcing, Langmuir turbulence, and wave-induced pressure corrections. In addition, the imposed transport may introduce inconsistencies in the full dynamical framework (e.g., potential vorticity balance), since the momentum pathways associated with the wave field are not explicitly represented.
- **Interpretation of the response:**  
    As a result, the diagnosed circulation changes should be interpreted as the ocean’s dynamical response to an imposed transport perturbation rather than the true physical pathways through which waves interact with the ocean.
- **Scientific value of the experiment:**  
    Despite these idealisations, the configuration provides a controlled framework to assess the potential climate relevance of wave-driven Lagrangian transport. By quantifying how much of the imposed Stokes transport is dynamically compensated and how much contributes to residual tracer redistribution, the experiment evaluates whether this additional transport pathway can produce a measurable impact on the coupled climate system.

# [[2026-03-26]]
## Discussion on “Stokes advection of heat” with Lars and Nobu
### Comment from Lars
Inviscid simulations, which is purely driven by waves. $u$ is Eulerian, $u^S$ is Stoke drift, $u^L$ is Lagrangian velocity, ~={red}**The latter advects momentum, tracer and potential vorticity, and oscillates around zero**=~.
![[Screenshot 2026-03-31 at 11.37.29.png|center]]
- In a steady state (when the Coriolis-Stokes forcing is fully developed), the induced **mean Eulerian compensation** (~={red}oscillating Eulerian return flow=~) is exactly the same magnitude of the Stokes drift (100% cancel)
- However, in an **unsteady state** (when the Coriolis-Stokes forcing is still developing, the first 5 hours in the above plot ), there is **net influence from the Stokes drift** 
	- In real climate, the Stokes drift is not steady, it varies in its magnitude and direction
	- In some regions, the development of Eulerian return flow is slow (longer unsteady balance). In these regions, the Stokes transport may induce net impacts already before the full Eulerian compensation is done
### Comment from Nobu
“... Also, instead of treating the stokes advection as a surface flux, **it might be easier to just add the Stokes advection and the Stokes-Coriolis in the ocean interior --either at the top layer or, better, over some depth assuming some simplified vertical profile of the Stokes drift.** In this way, all you need to do is to get a Stokes-drift data from ERA5, and you don't have to do anything artificial to u,v,T,S. In this way, you can also consider the Stokes advection and Stokes-Coriolis together, which is what you should do outside of the equatorial regions. Aside from dealing with ICON's unstructured grid, this should be relatively straightforward with Helmuth's help.”


# [[2026-03-20]]
## Discussion on project Stokes transport & 1st paper idea
#presenter/Nils_Brüggemann #presenter/Noel_Gutierrez-Brizuela #presenter/Christopher_Higgins 
Three ideas so far:
### Coupled ICON atm-oce with different c_k
- Apply three (?) different mixing parameterisations (e.g. Gaspar with ck=0.1,0.5 and k-eps or kpp or even something else)
- run simulations for current climate (control simulations)
- investigate heat budget of upwelling systems and effect of mixing on upwelling dynamics
- repeat analysis with simulation with enhanced CO2 forcing (e.g. 4xCO2 or 1% CO2 increase)
- Pros:
	- minor developments required (GOTM-library needs to be updated in recent master)
	- ==coupled atmosphere-ocean simulation==
	- control simulation is ready
- Cons:
	- computationally heavy
	- no surface waves
- ~={red}Comment from Noel=~: 
	- Changing the mixing efficiency parameter $c_k$​ globally will inevitably alter water mass properties throughout the ocean. Consequently, the characteristics of the water that upwells in coastal upwelling regions will also be modified, which can complicate the interpretation of the local heat budget.
		- The key issue is ==non-local influence (or remote influence)==. When $c_k$​ is changed globally, the resulting differences in coastal heat budgets cannot be attributed solely to processes occurring in the coastal upwelling region.
		- the upwelled water originates remotely. If the global mixing changes these water masses, then the temperature and heat content of the source water changes and the background stratification changes
		- Therefore, it becomes unclear whether the change is caused by the **local effect of altered mixing**, or **remote changes in water mass properties that are advected into the region**
	- Thus, think about doing a “regional tuning” (e.g., from a simple latitude-longitude box)
- ~={red}Comment from Christ=~: 
	- Good idea of analysing the atmosphere-ocean feedback mechanism and its effect on coastal upwelling dynamics
	- Can use a ==metric of wave activity== (have a look over the paper: *wind sea swell climatology*) to be the reference of tuning the mixing parameter $c_k$
		- strong wave activity → increase the $c_k$, but again, this will introduce non-local influence which complicates the interpretation
		- any method to separate local & non-local influence?
### ICON ocean with ERA5 Atmosphere and wave forcing
- run historic period of ICON ocean with ERA5 forcing
- add Stokes drift from ERA5 to TKE equation and repeat simulation
- analyse heat budget of coastal upwelling systems and compare the different simulation
- Pros:
	- computationally less expensive
	- surface wave effects can be analysed, limit wave effects to Langmuir turbulence
- Cons:
	- reading of Stokes drift by python reader needs be developed
	- incorporation of Stokes drift into TKE is necessary
	- To implement Langmuir turbulence, the reconstruction of Stokes profile is needed. The idea is to use Breivik’s parameterisation (see (Breivik et al., 2014)) which approximate Stokes drift profile via (1) Surface stokes drift (directly from ERA5) and (2) Stokes transport (need estimation)
### Coupled ICON atm-oce with ERA5 Stokes-induced heat/mass divergence
- Run historic period of atmosphere-ocean coupled ICON with “forcing” that is introduced by the heat/mass divergence from Stokes transports
- This idealised experiment **intends to perturb the coupled atmosphere-ocean ICON model with Stokes-induced heat and mass divergence**
	- In this setup, the Stokes effects act only as a perturbation that “kicks” the coupled system
	- The ==tracer transport associated with the Stokes drift is therefore represented through the imposed divergence, while the Stokes-induced Eulerian adjustment emerges dynamically within the model after the mass divergence is applied==.
- The primary focus of this idealised experiment is thus the Eulerian adjustment in the coupled system, whereas the waves themselves simply provide the initial perturbations at every timestep
- **This approach is fundamentally different from (Wu et al., 2019).** In their study, they attempted to introduce the full wave-averaged Boussinesq framework, but the implementation in their isolating experiments was inconsistent. 
	- In particular, they considered the Stokes-induced return flow but did not include the Stokes advection of tracers in the first place. 
	- In addition, when manipulating the wave-averaged Boussinesq equations, they neglected the wave-induced advection of momentum while isolating the Coriolis-Stokes forcing. 
	- These omissions lead to an internally inconsistent representation of the wave effects.
- The new paper (Li et al., 2026) 

# [[2026-03-18]]
## Regular meeting with Nils
- Discussion on designing future “ICON-Wave sensitivity test”
	- we need to be careful of “playing” around the Stokes drift related effects
		- some papers, like (Wu et al., 2019), separated the Stokes drift related effects to (1) CSF and (2) Stokes advection of travers. 
		- The idea is great, but how they implement the experiment is wrong. 
		- For example, in one of their experiment, they **only considering the CSF in the momentum equation but ignore the Stokes advection**. The significant “response” of models in terms of surface tracer distribution are simply due to the presence of “Eulerian return flow”.
		- Also, they keep the CSF forcing but neglect the “pressure correction” term and the “vortex forcing” term. **This implicitly ignores the Stokes advection of momentum**, which is in the first-order of importance in the momentum equation
	- ~={red}**==Thus, in our experiment, we can classify the wave effects==**=~ (after discussed with #presenter/Nobu_Suzuki, also see [[Research-Gaps#2026-02-09 Update Refine research questions]]
		- **Surface wave mediated momentum flux** (i.e., stress)
		- **Stokes-drift related processes**
			- CSF
			- Stokes advection of tracers and momentum
			- Stokes shear (its effect on large-scale flow)
		- **TKE changes induced by surface waves**
			- Langmuir turbulence (also related to Stokes shear, but its effect on small scales)
			- wave-breaking

# [[2026-03-12]]
## Noel’s group meeting: Update from Muxin
- Presentation: ==**“Proposal: Stokes transport of Heat”**==
	- Received feedback:
		- perform a sensitivity experiment by ==**forcing the ICON model with the Stokes-induced heat convergence**== ($-\nabla \cdot Q_{st}$)”, even though its magnitude is much smaller than the net surface heat flux $Q_{net}$ into the ocean
		- However, Stokes drift also produces Lagrangian mass convergence, denoted as $-\nabla \cdot M_{st}$. Since the ocean must conserve mass, this convergence cannot remain unbalanced and must be compensated by adjustments in the Eulerian flow and sea surface height.
		- Therefore, ~={red}**a physically consistent forcing should account for both heat and mass conservation**=~. Considering the vertically integrated ocean heat conservation (OHC):
		  $$OHC=\rho c_p(SST\times SSH) \tag{1}$$
		  Perturbations in heat content due to Stokes-induced heat convergence can be expressed as:
		  $$OHC+\delta Q = \rho c_p(SST+\delta T)(SSH+\delta h) \tag{2}$$
		  Subtracting $OHC$ from equation (2) will leaves:
		  $$ \delta Q = \rho c_p [\delta T(SSH+\delta h)+\delta h(SST+\delta T)] \tag{3}$$
		  **This relationship links Stokes-induced heat convergence to perturbations in SST ($\delta T$) and SSH ($\delta h$) while implicitly accounting for mass convergence.**
		- Therefore, the Stokes-induced forcing could be represented in the model as perturbations to SST and SSH, allowing the ocean model to dynamically adjust while conserving both heat and mass
	- How to get the $\delta T$ and $\delta h$?
		- we have the heat and mass convergence ($-\Delta Q_{st}$ and the $-\Delta M_{st}$) at each time step
		- ==Mass conservation determines $\delta h$:==
		  Integrate from bottom $z=-H$ to $z=h$ (free surface), the continuity equation gives:
		  $$\begin{align}
		  \int_{-H}^{h}(\nabla \cdot \mathbf{u}+\frac{\partial w}{\partial z})\;dz&=0 \\
		  \int_{-H}^{h}\nabla \cdot \mathbf{u}\;dz + w(h)-w(-H) &=0
		  \end{align}$$
		  Since $w(-H)=0$, and the surface vertical velocity: $w(h)=\frac{\partial h}{\partial t}$, then:
		  $$\frac{\partial h}{\partial t}=-\int_{-H}^{h}\nabla \cdot \mathbf{u}\;dz$$
		  The mass divergence (i.e., mass flux divergence) is expressed as:
		  $$\begin{align}\nabla \cdot M&=\nabla \cdot (\rho\int_{-H}^{h}\mathbf{u}\;dz)\\&=\rho\int_{-H}^{h}\nabla \cdot \mathbf{u}\;dz\end{align}$$
		  Then the SSH change is therefore linked to the mass divergence:
		  $$\frac{\partial h}{\partial t}=-\frac{1}{\rho}\nabla \cdot M$$
		  Define $\Delta M_{st} = \nabla \cdot M_{st}$, for a discrete time step $\Delta t$:
		  $$\delta h = -\frac{\Delta M_{st}}{\rho}\Delta t$$
		  ~={red}**This directly gives the SSH perturbation forced by Stokes mass convergence**=~ $\Delta M_{st}$
		- ==**Heat content perturbation:**==
		  Rewrite the equation (3):
		  $$\Delta Q_{st}\Delta t=\rho c_p[\delta T(SSH-\frac{\Delta M_{st}}{\rho}\Delta t)+(-\frac{\Delta M_{st}}{\rho}\Delta t)(SST+\delta T)]$$
		  get the $\delta T$ by replacing the $\delta h$.
 

# [[2026-03-11]]
## Regular Meeting with Nils
#presenter/Nils_Brüggemann 
- Issue: **Significantly stronger surface Stokes drift in ICON-Wav compared to ECWAM over tropical and subtropical regions** 
	- The RMSE values reach ~0.2 m/s over subtropical regions, which is substantially large comparing to the range of absolute magnitude (0-0.4 m/s)
	- The significant wave height (SWH) shows relatively good agreement between the two models for both total waves and wind waves. Slightly higher RMSE values appear in the comparison of swell SWH, but their magnitude (~0.01) remains small relative to the absolute SWH values.
	- This suggests that the overall wave energy levels are broadly consistent between the two models. Moreover, the spatial patterns of SWH difference do not resemble those observed in the surface Stokes drift velocity
	- Therefore, the discrepancies in surface Stokes drift are unlikely to originate solely from differences in bulk wave energy. A plausible explanation is differences in the simulated wave energy spectrum, as Stokes drift depends strongly on the higher-order frequency part of the spectrum and scales roughly to the 3rd moment of frequency spectrum (see details in [[Note_Stokes-Drift-Profile#Stoke Drift Profile]])


# [[2026-03-04]]
## ICON-wave focus group: Meeting 5
- ICON-wave status:
	- **Development of wave-current refraction is finished**, simulation is running, updates will be provided in the GitLab page: [[https://gitlab.dkrz.de/icon-waves/projects/icon-waves-working-group/-/boards]]
	- Development of Wave-modified fluxes encounters problem (i.g., momentum flux: $\tau_{oc}=\tau_a - \tau_{w} - \tau_{ds}$, and energy flux: $\phi_{ds}$) with the “type variables” in the ICON code
	- Initialised the TKE-scheme update with incorporating the Stokes drift. 
		- problem with coupling, since the stokes drift is a 3D variable
		- TKE scheme update also requires the implementation of energy flux from the wave (i.e., wave-dissipated energy from wave-breaking)
	- We would like to have the ERA forced simulations with:
		- ERA5 atmospheric forcing + ICON-o
		- ERA4 atmospheric forcing + ICON-Wave + ICON-O
- **ICON-wave standalone simulation compare to the ERA5 wave products:** 
	- compare the surface stokes drift velocity with the **geological location**, instead of remapping

# [[2026-03-03]]
## Helmuth
- ask Nils about joining the project from TRR that I can use the node hour
	- 1239: difficult one with Gulia, need to ask her maybe
	- 1102: from Nils → [[2026-03-05]] Update: Already received the memembership!


# [[2026-02-19]]
## Meeting: project eddy and upwelling
#presenter/Noel_Gutierrez-Brizuela 
- **scientific problem**: observed cooling /constant SST in the historical trend, but the simulation with GHG increasing shows the warming trend from past to the future
	- accelerated warming in the simulation
	- but the observation is cooling
	- ICON simulation shows the historical cooling!! and then transition to warming in the 2020-2050 period
- Takeaway message:
	- SST experience a regime shift after 2020 
		- downward Qnet weakens
	- Nearshore warming is accompanies by stronger stratification and enhanced offshore transport
		- thus, not obvious reason for the SST warming
	- the warming enters through the vertical pathway
		- through vertical mean transport
		- with enhanced offshore transport redistributing this nearshore heat
	- air sea damping strengthens in the future period, limiting nearshore eddy amplification


# [[2026-02-12]]
## Brainstorming Session
#presenter/Nils_Brüggemann #presenter/Noel_Gutierrez-Brizuela #presenter/Christopher_Higgins 
See detailed notes in [Updated ideas](Research-Gaps#2026-02-12 Update Ideas)

## Regular Meeting with Nils
- regarding the updated research questions listed in [Refined research questions](Research-Gaps#2026-02-09 Update Refine research questions)
	- the $\Delta TKE$ could be very interesting, as it change the stratification, the extent of the mixing layer, change the sea state, some 2nd and 3rd feedback mechanisms may ultimately change the upwelling intensity and structure
		- Nils: “The Langmuir turbulence may change a lot to the TKE”
	- one potential idea (listed in [[Research-Gaps]]): **relate the Stokes transport (largely dominated by swells) to the dispersion of pollutants in Eastern-boundary coastal system**. 
		- Usually, these pollutants will be carried away by the Ekman transport offshore
		- Since the swell propagates onshore (e.g., in Peruvian), and the Stokes transport is largely determined by the swells, this may counteract to the ekman transport
		- But need to identify ~={red}extreme cases for swells=~, which can be filtered by applying mask.
			- mask: use significant height, to pass certain threshold
			- first one would need to get a time series of the regional mean wave height (from difference wave component (wind waves, total swells))
- We could also try to force the ICON-o with the ERA5 atmospheric and wave forcing
	- to study and estimate the Stokes transport

# [[2026-02-04]]
## ICON-Wave focus group: Meeting 4
- update on **ERA5** simulation: the simulation is basically done.
	- r2b7: same atm/oce, one year simulation
	- can check the [output namelist](https://gitlab.dkrz.de/icon-waves/projects/icon-waves-working-group/-/issues/1)
	- also some output for year 2024. 
	- Issues:
		- **Wave-current interactions**: 
			- can find the information in [MR!1664](https://gitlab.dkrz.de/icon/icon-nwp/-/merge_requests/1664)
			- the current branch (MR!1961) coded the refraction of the ocean currents due to the wave (i.e., wave-current interactions)
		- **Readers for forcing**:  
			- Test if the python reader from Helmuth #presenter/Helmuth_Haak can read the ERA5 forcing for one year test in r2b7 (the same branch **MR !1961**)
			- check with Helmuth and Nils next week


# [[2026-02-03]]
## Regular Meeting with Nils
#presenter/Nils_Brüggemann 
Discussion on the output namelist and an update on the detailed research questions. The note can be found here [[ICON_Output_Namelist#Potential output namelist]]


# [[2026-01-29]]
## Regular Meeting with Nils
#presenter/Nils_Brüggemann 
- **output namelist**: 
  think of getting the direct model output for the ~={red}**heat, salinity tendency budget**=~ for the tracer (T, S, then calculate the density (buoyancy)) budget analysis.
	- the core to analysis is the ==heat budget in the mixed layer change off shore== and how this is related to the wind-driven upwelling and wind-induced surface waves
		- the extent of mixed layer is influenced by the mixing process, and this is related to the state of ocean (temperature, salinity), and the turbulence mixing in air-sea interface and mixed layer bottom
- in r2b7 configuration:
	- the eddy are not resolved, the output of $\bar{u\rho}$ is given as the mean covariance, there’s no eddy velocity or eddy heat flux.
	- hence the eddy transport of tracer is represented by parameterisation, namely “**Gent McWilliams parameterisation (GM90)**” (see details in )
		- short intro: The **Gent-McWilliams (GM) parameterisation** (often referred to as Get-McWilliams or GM90) is a fundamental mathematical technique used in ocean general circulation models (OGCMs) and climate models to represent the effects of unresolved, small-scale ocean eddies (mesoscale eddies) on large-scale ocean currents and tracer transport. 
		- Its primary potential meaning and function is to **act as a sink for available potential energy by "flattening" isopycnals (surfaces of constant density)** and reducing horizontal density gradients, simulating how eddies mix the ocean without relying on computationally expensive, high-resolution simulations.
- One can also think of the representation of coupling mechanisms between waves-ocean-atmosphere in the atmosphere.

# [[2026-01-19]]
## Regular Meeting with Nils
#presenter/Nils_Brüggemann 
- does the ocean has the output for surface fluxes? (sensible heat, latent heat, momentum?). I only find the `HeatFlux_Total` → ==**check namelist for the ocean**==
- needed variables that is missing on standard ICON-XPP output (see [[ICON_Output_Namelist#ICON-XPP standard output]]): 
	- **atmosphere**: wind stress components (`tauu, tauv`);
	- **ocean**: 
		- wind stress components (`stress_xw, stress_yw`)
		- tke (different components → difference balance term in the tke equation (see details in [[ICON_Parameterisation]])
- ==**For the case of c-k=0.3:**==
	- compare to the extreme case (c_k=1.0), it is more realistic; No major change in the background climate
		- Which is good for us to attribute the change in our target (i.e., wind, wave, upwelling) to local change and local processes. No need to have a thorough analysis of the global climate
- To do:
	- Ask the `Wind_speed_10m` output in ocean grid and if the atmospheric grid also has wind stress components; → the wind_speed_10m in ocean output is only on when turning on, but in general I don’t need this.

# [[2026-01-12]]
## Regular Meeting with Nils
#presenter/Nils_Brüggemann 
- **ICON-waves-ocean coupled test?**
```
Dear all,

you can find step-by-step instructions on how to prepare and run an ICON‑waves‑ocean coupled test simulation here:

[https://gitlab.dkrz.de/icon-waves/projects/icon-waves-working-group#how-to-run-icon-waves-on-levante](https://gitlab.dkrz.de/icon-waves/projects/icon-waves-working-group#how-to-run-icon-waves-on-levante "https://gitlab.dkrz.de/icon-waves/projects/icon-waves-working-group#how-to-run-icon-waves-on-levante")

Please try it out and let me know if you have any issues or comments.

Best regards

Mikhail
```
- **Update on the ICON-XPP test run** (default c_k versus changes in c_k)
	- the new output (TKE-related variables) → problem, cannot produce new output files! Already contacted Helmuth. He said this might be a bug for the ICON code, need to contact **Kalle**
	- Will continue some basic comparisons on variables we discussed and decided during our meeting last week (e.g., wind profile). This time, **applying the new defined masks**
- Nils’ suggestion on calculating the mean values when considering the region mask. Recorded in [[ICON_Data-process_Tips#Calculate Spatial Average]]
  ```python
  # consider the different grid area. this is important when lower resolution
  # to normalise the effect of different grid area
  to_ave = (to * ds_tg.cell_area * mask).sum(dims='ncell')/ (ds_tg.cell_area * mask).sum(dims='ncell')
  ```

## Regular Meeting with Noel
#presenter/Noel_Gutierrez-Brizuela 
Discussions on the figures that compare the SST between different simulations: 
1. default (c_k=0.1)
2. Exp_1 (c_k=0.3)
3. Exp_2 (c_k=1.0)
and find:
- the zonal averaged potential temperature profile shows a signature spatial pattern showed above. The upper ocean near the Equator is slightly cooler than the subtropics; The SST is similar to the subsurface water over subtropical region: ![[Screenshot 2026-01-13 at 16.09.57.png|center]]
- **Increasing the viscosity parameter $c_k$ enhances vertical mixing in the upper ocean, transporting heat absorbed from the atmosphere into the ocean interior.** In extreme cases (e.g., $c_k=1.0$), this strong mixing can cool the surface layer despite increased heat uptake.
- **A competing effect arises because a cooler surface increases the ocean’s capacity to absorb atmospheric heat**, particularly in the tropics where net radiative input is large.
- **As a result, excess atmospheric energy is absorbed and mixed downward over the tropical and subtropical oceans**, while the very near-surface layer remains cooler due to efficient vertical redistribution.
- **Dynamically, this subsurface heat is transported poleward by ocean currents.**  
    When the warmer subsurface waters reach higher latitudes, they are mixed upward, warming the surface ocean there.![[Screenshot 2026-01-13 at 16.03.47.png|center]]
    

# [[2026-01-08]]
## Ice Melting: From the lab to ocean
#presenter/Detlef_Lohse
- melt rate of icebergs and glaciers
	- current models are off by an order of magnitude
	- ice melting as complex, multi-scale, multi-physics phenomenon
	- because the density of water depends on T and S, the ambient water around the ice will change position, and influence ice melting
	- relevance of Stefan problem on large scales: buoyancy driven flow
	- over year 2023,24,25, the melting of Antarctic is significantly deviates from other previous years
	- melting on smaller scale: melt ponds → essentials for radiative heat balance of earth (lowers albedo)
	- convection in melt ponds → all due to the density of the water which moves warm and heavy water down from the surface
	- the melting from under water would ‘eat’ the ice above → through convection in the water layer
	- ice melting regime:
		- temperature-driven: depends a lot on ambient water temperature
		- salinity-driven
		- competing between two driven mechanisms
			- the scallops (i.e., the “tips” of ice attached to the ice stick) show up: because the warm ambient water is entrained towards the ice from below, which also makes the scallops move downward with time
			- and it was the impact from salinity in the ambient water!!! Which change the ambient water density effectively!!

# [[2026-01-07]]
## ICON-Wave focus group: Meeting 3
- ERA5-forced ICON-ocean standalone run: haven’t done
	- difference between the master branch versus Helmuth’s branch: interpolation of the ERA5 data.
		- ERA5 forcing and `omip` forcing is flagged different; The ICON read the forcing by coupler (i.e., python reading)
- ERA5-forced ICON-waves standalone run
- fluxes in the ICON ocean: update from Chris
	- energy flux from wave to ocean
		- we have he correct swell dissipation term in ICON-waves (advanced dissipation source term)
		- the impact from the swell to the atmosphere is reflected in the wind input term, as if the negative wind input, which means the swell travel faster than the atmosphere, there’s negative $S_{in}$ and negative momentum flux. While the dissipation term due to white capping and wave breaking remains the same.


# [[2026-01-05]]
## Regular meeting with Nils
#presenter/Nils_Brüggemann 
- For regional masks: can use the `ckdtree` to define;
	- Since we are more interested on the air-sea interface (as surface waves are oceanic waves), one should put a **ocean mask on top of the regional masks** to masked out the land components!!!
- Some atmospheric variables worth to check:
	- atmospheric conditions:
		- large-scale; global: 
			- SLP, is there a major shift in ITCZ? ENSO?
		- local: 
			- wind profiles; 10m-wind speed → might be informative for boundary layer conditions
			- surface fluxes (sensible heat, latent heat, wind stress (momentum flux))
## Generic Academic Skills
- when encounter the cases when the model crashed, keep track the time the the values changed 



# [[2025-12-17]]
## ICON-Waves focus group: Meeting 2
- the documentation for the ICON-Wave discussion group:[link]([[https://gitlab.dkrz.de/icon-waves/projects/icon-waves-working-group/-/boards]]), comment and track on this website
- Eva Boza:
	- how the wave and rain will affect the air-sea CO2 exchanges
	- modify the parameterisation of the flux, which is currently only link to the wind input (U_10), no wave contribution
		- also for the sensitivity test of different combination of wind, rain, wave and have a look over the CO2 exchange fluxes
		- will focus on the global and then move to the regional
- Update on the wave stress on the ICON-ocean:
	- wave to ocean stress looks alike to the atmospheric stress, despite a bit different magnitude
	- next step; dynamic coupling between waves and oceans (including logical switch, allow to switch which mode we want to use)
		- Nils will handle the additional filed ‘forcing_stress’ in ICON-O which can be set to wind stress or the wave stress depending on the simulation mode
		- The current ICON-Wave won’t update the advanced dissipation source term in the first Phase (since the ICON-Wave is basically the WAM)
			- I can modify to play
	- The ERA5-forced ICON-Waves standalone run → have a look over the output!!
		- but this is not physically coupling! the ERA5 only change the boundary conditions
			- ERA5 provides the significant wave height
		- check the output namelist
		- the stokes drift from ICON-Waves use the same vertical coordinates as ICON-O
		- EAR5-forced ICON-O standalone run for full 2024? link? Nils will provide in the website
	- We should provide a proposal to DKRZ for running ICON-Waves in Levante

# [[2025-12-15]]
## Regular meeting with Nils
#presenter/Nils_Brüggemann 
- Show the derivation of wind-induced stress (or “wind stress”, “wind-to-wave stress”). The [Link]([[Note_Wave-induced-stress_and_Young-waves#Momentum and Energy Flux at Air-sea Interface]])
- Report issue with ICON-XPP simulation with default TKE settings
	- Has set disturbance TWICE!!! One in 1325-01-01 and one in 1345-01-01
	- Should I worry about the email from DKRZ?
```
______________________________________________________________________
		[33] CVR-DIR (Max-Planck-Gesellschaft)
		Quota             :    1925 TiB
		Actually used     :    1856 TiB
		Percentage used   :      96%
______________________________________________________________________
```
		
- **Discussion**: Wave-induced processes and their effects
	- maybe we first choose a terminology to describe this long set? (e.g., wave-effects)
	- **Governing equations** for 4 main wave-modified processes:
		- momentum flux
		- Stokes drift related effects 
			- Langmuir Turbulence
			- Stokes-Coriolis force
			- advection by Stokes drift
		- surface roughness
- For later analysis:
	- start from simple comparison for the sensitivity test for simulations with different `c_k`
	- define the sections: coastal upwelling region, and a broader region of interests of SE Pacific
		- focus on domain-averaged value on: potential T, density, other output in TKE. And most important, focusing on the profile of these values
		- suggestions from Nils:
			- can have a comparisons of certain values in a depth * time space, can have a look over the time dependency (trend, seasonality)
- ask Stephan for the weird simulations of `c_k=0.01`, is this often that the simulation crashes?


# [[2025-12-10]] 
## Noel’s group meeting
#presenter/Noel_Gutierrez-Brizuela 
- W.R. Young and Basile Gallet
- <span style="background:#fff88f">swell is predominantly linear </span>→ predictable (kind of) (surline.com → surf conditions of swell)
	- period: 6-30 s
	- group velocity: 5-20 m/s
	- wavelength: 50-1000 m
	- sea-surface slope: ~0.05-0.2
	- still consider shallow compared to the ocean depth
- <span style="background:#fff88f">energy flux</span> = group velocity * Energy density $=\frac{\rho g^2 T \alpha^2}{8 \pi}$
	- T is the period
	- a is the amplitude
	- turns out to be large energy flux carried by swell (40 KW/m when T=10sec, a=1m) → good sites for wave power production
		- 25 km coastlines = 1 Gigawatt ~ 1 nuclear per plant
- gravity has acceleration rate $=\sigma^2\alpha$, sigma is $\sqrt{gk}$ → deep water wave dispersion relationship
	- swell has rotary acceleration
- after a storm, dispersion sorts out the waves, long and faster waves outrun the storm, this is the swell
	- measure the frequency of waves, one events generated waves at different frequency
	- frequency change with time: $\omega(t)=(\frac{g}{4\pi R})\times t$ 
		- R is the range from the source of the wave to where you observe
		- group velocity is only dependent on the k is deep water dispersion: $\omega^2=gk$
		- the R (distance from the source region)can be calculated by measuring the frequency relationship with time.
	- the direction of the source?
		- Swell incident on a triangular array of bottom pressure sensors (measure the phase lag from three arrays to get the direction)
		- **but swell seems not to follow great circles backtracking** → errors?
		- why don’t wave follow great circles?
			- not because of the Stokes-Coriolis force!!
			- Young’s hypothesis: refraction by surface currents between the source and the receiver
				- wave can be influenced by the wind the ocean currents
				- when the wave group encounters with strong currents (ACC, and equater longitudinal currents)
				- however, the current velocity doesn’t matter!!!!
					- if a uniform current, do not deflect the wave direction
					- but the <span style="background:#fff88f">vorticity of the currents refracts waves </span>(many vorticities in ACC!)

# [[2025-12-08]]
## Regular Meeting with Nils
#presenter/Nils_Brüggemann 
- there’s no k-epsilon scheme in the XPP configuration?
  In the `./types/XPP/piControl-R2B5_R2B7.config` → `EXP_TYPE` files for piControl for b5b7
  ```shell
  [[[ocean_vertical_diffusion_nml]]]
      vert_mix_type = 2           # 1: PP; 2: TKE
      alpha_tke = 30.0
      c_eps = 0.7
      c_k = 0.1                   # V2024-07: 0.05
      cd = 3.75
      #use_kappa_min = true        # b5b7: def=false
      #kappah_min = 2.0e-5         # b5b7: def=1.0e-5
      kappam_max = 100.0
      kappam_min = 0.0
      mxl_min = 1.d-8
      only_tke = true
      ppscheme_type = 0
      tke_min = 1.d-6             # V2024-07: 1.d-5
      tke_mxl_choice = 2
      tke_surf_min = 1.d-4
      use_lbound_dirichlet = false
      use_ubound_dirichlet = false
      # The following settings were deleted as a precaution measure:
      bottom_drag_coeff = 
      convection_instabilitythreshold = 
      lambda_wind = 
      richardsondiffusion_threshold = 
      salinity_verticaldiffusion_background = 
      temperature_verticaldiffusion_background = 
      tracer_convection_mixingcoefficient = 
      tracer_richardsoncoeff = 
      use_wind_mixing = 
      velocity_richardsoncoeff = 
      velocity_verticaldiffusion_background = 
      clc = 0.15
      l_lc = true
  ```
- when run the icon-xpp b5b7, with parent simulation from Stephan (piControl spin-up 1300 years) with default setting in ocean vertical diffusion parameter (see below), the simulation can only continues with 25 years, and will end up in error message `Surface height too large!`
	- but when set `c_k=1.0`, the simulation continues further >= 25 years
		```Fortran
		/
		&ocean_vertical_diffusion_nml
		    vert_mix_type = 2 ! 1: PP; 2: TKE
		    alpha_tke = 30.0
		    c_eps = 0.7
		    c_k = 0.1 ! V2024-07: 0.05
		    cd = 3.75
		    ! use_kappa_min = .true. ! b5b7: def=false
		    ! kappah_min = 2.0e-5 ! b5b7: def=1.0e-5
		    kappam_max = 100.0
		    kappam_min = 0.0
		    mxl_min = 1.d-8
		    only_tke = .true.
		    ppscheme_type = 0
		    tke_min = 1.d-6 ! V2024-07: 1.d-5
		    tke_mxl_choice = 2
		    tke_surf_min = 1.d-4
		    use_lbound_dirichlet = .false.
		    use_ubound_dirichlet = .false.
		    clc = 0.15
		    l_lc = .true.
		/
		```
	- might due to the atmosphere crashes. need to change the ‘trajectory’ of the simulation (refers to Chaos theory)

## Academic writing course: Graphic and Colors
- can use `seaborn` package to define the context of the figure about the figure size to match the desired width for publishing (https://seaborn.pydata.org/generated/seaborn.set_context.html)
- go to wiki and check the **style guide** for the unit
	- consistency is important
	- LaTex’s *siunit* package
- for the d/dt, the ‘d’ should be Roman (not italic) since it is not a variable. For variable, use italic style
- colormap
	- pre built colour maps that are perceptually linear and well suited for colour blind people
		- Crameri Color Maps
		- Colormaps for Oceanographie
		- Color brewer
	- build you own colour map
		- Check if your colour map is not good for color-blinder
			- HCL wizard
			- color blind test


# [[2025-12-03]] 
## Noel’s group meeting
### Solve a Fluid dynamic equation
- when considering two rigid-body plate on the <span style="background:#fff88f">top and bottom</span> of the fluid respectively, and moves in two velocities $u_t$ and $u_b$. 
  If we assume: 
  (1) Purely horizontal (in $x$ direction), incompressible flow → constant density; 
  (2) The rigid-body plates have friction
  What is the fluid velocity?
- in this case, the governing equation would be only the zonal momentum equation:
  $$
    \begin{align}
	\frac{\text{D} u}{\text{D} t} &= \frac{\partial u}{\partial t} + u\frac{\partial u}{\partial x}+v\frac{\partial v}{\partial y} + w\frac{\partial w}{\partial z} \\
	&=-\frac{1}{\rho_0}\frac{\partial p}{\partial z} + \frac{\partial}{\partial z}(\nu\frac{\partial u}{\partial z})
	\end{align}
    $$
    
    since the flow is purely horizontal and only has zonal component, the meridional and vertical velocity will be cancelled; And due to incompressibility, there’s no pressure gradient term. 
    → the whole equation will end up in:
    $$
    \frac{\partial}{\partial z}(\nu\frac{\partial u}{\partial z})=0
    $$
    
    with this, we can get the analytical solution for the zonal flow velocity $u=u(z)$, by the boundary conditions

### Fourier Transform
The video link: [https://youtu.be/gTOzmE7_-mU?si=hh7dkCD_TSs3r_HI](https://youtu.be/gTOzmE7_-mU?si=hh7dkCD_TSs3r_HI)

- ==Fourier transform==:
	- ~={orange}**It takes a signal in time (or space) and expresses is as a sum of sinusoidal waves with different frequencies, amplitudes and phases**=~
	- It decomposes any signal into its building-block waves
	- If $X(f)$ is the Fourier Transform of $x(t)$, then the Power Spectral Density (PSD) is:
	  $$
	    PSD(f)=\frac{|X(f)|^2}{(\text{normalisation})}
		$$
		Hence, PSD represents the distribution of power (i.e., variance) per unit frequency.
	- If the FT is applied to a *time series*, then the PSD tells you:
		- ~={orange}**how the variance (energy) of a signal is distributed across frequencies or wavenumbers**=~.
		- Which time scales dominate variability. It answers what time scales control the **variability** of the scaler quantity
		- Variability is dominated by rapid (high frequency) or slow (low frequency) processes

# [[2025-11-25]] 
## Regular Meeting with Noel
Progress
- Theoretical part:
    - **(Finished)** Few formula for **estimating Stokes drift profile** (i.e., wave variance spectrum), Stokes transport, and how does it related to the additional source term in TKE (i.e., Stokes drift shear → Langmuir Turbulence generation)
- Technical part:
    - **(In progress)Tried with conducting the ICON-XPP b5b7 experiments with simple forcing (OMIP)**, but some technical difficulties remained in setting the `mkExp` version of ICON-XPP (working on that, and will asking technical guys during retreat with Nils)
    - **(Finished) Played around with the ocean standalone model; changing the TKE scheme parameters** (the c_k, which influence the turbulent viscosity $A_v$ (i.e., the ability of eddy to mix momentum),and therefore the shear production term $A_v(\frac{\partial \overline{\mathbf u_h}}{\partial z})^2$)
    - **(In progress) Read the technical details of ECMWF wave model** (i.e., ECWMF version of WAM) → to have a sense of how other wave models are performing
- Administration part:
    - **Have already decided my panels**: Nils, Noel, Jin-son (Professor, so called “main supervisor” but no), Christopher Higgins (PostDoc from DWD)
        - Mainly co-supervised by Nils and Noel
    - Arranging the first panel meeting: aiming at the second week of December (8th to 12th of Dec)
Questions
- Ask for this ‘scientific pitch’
    - what do people expect from me??
    - discuss the scientific way

# [[2025-11-24]]
## Regular meeting with Nils.
- XPP simulation: omip; 100 years
- why in 30 years ocean only experiments, the difference
- ask johanna for TRR 181 workshop
- wait for the mkexp
- ask panels for the time of panel meeting, 2nd week of Dec
    - Nils
    - Jin-son
    - Christ
    - Noel
- register enroll as PhD before the panel meeting
## TRR181: Workshop; Ferrel Cell
- outline: the summary part from the material
- accommodation in Bremen (TRR181)

# [[2025-11-18]] 
## ICON-XPP: T&F meeting
see details in [here]([https://gitlab.dkrz.de/icon/icon-nwp/-/wikis/mkexp-20251118#make-experiments-mkexp-for-xpp---introduction-purpose-and-advantages](https://gitlab.dkrz.de/icon/icon-nwp/-/wikis/mkexp-20251118#make-experiments-mkexp-for-xpp---introduction-purpose-and-advantages))
### *mkexp* versus *make_runscripts*:
- no copies of long run-scripts (by `make_runscripts`) with different histories
    - since complete runscripts (by `make_runscripts`) are complex and error-prone,
- basic configuration (by `mkexp`) of XPP experiments is stored and maintained elsewhere (e.g. `piControl-R2B5_R2B7.config`),
- experiment configuration file contains relevant changes only (copy of e.g. `xpp-b5b7-HDext.config`, this file is much more convenient to share and circulate,
- for publication: branch and configuration files have to be delivered, **only**,
- modularised structure: options for standard input and output or other types of experiments (historical run) are available,
- other extensions, like new options or experiment types (1% CO_2 increase) are easy to add
- error-prone initialisation mechanism (`isRestart.sem` by `make_runscripts`) replaced by initialisation options (`mkexp`),
- mkexp command accepts input options - helpful for ensemble experiments.

### Options available
- `initialize.config`: start experiment from initialisation files, IFS for atmosphere, T and S from EN4-1950 for ocean (default: start from specific restarts for atmosphere, ocean, HD)
- `initialize_atmosphere.config`: start from ocean restart, initialise atmosphere from IFS
- `XPP/output/default_atm`, `default_ocean.config`: output necessary for pyicon quickplot package
- `CMIP7_output_atm.config` and `CMIP7_output_oce.config`: output for CMIP7 (under development)
- `XPP/input/historical` for historical experiment (start with simulation year 1850)

# [[2025-11-17]]
## Regular Meeting with Nils
**Questions**:
- ICON-XPP: detailed experiment settings:
    - simulation time: 100 years
    - forcing? → do we need to compare with the observations? → start with control, to see what is happening, enough to compare → go for historical? everything is changing
    - TKE-scheme: choose the default vs parameter changed
- **on wave model:**
    - The Wave model form China: 3rd generation **WASNUM:**
        - consider 5 representative ocean surface wave related processes:
            1. wave modulation of air-sea momentum flux → wave stress (part of the total wind stress) still not 100% sure about the physics behind
            2. sea spray induced air-sea enthalpy flux
            3. sea surface current the Stokes drift on air-sea flux
                1. it only consider the influence of **surface stokes drift velocity** → the mean wind relative to ocean surface: $U=U_a-U_{oc}-U_s$ → affect the calculation of friction velocity
            4. non-breaking wave-induced vertical mixing on upper-ocean
            5. rainfall induced sea surface cooling
    - For the Stokes drift profile: since we need to have the $V_s$ (Stokes transport) and the surface stokes drift velocity $v_0$ to approximate the profile, so how can we get both?
        - discussion over the next meeting
        - assemble questions and ask Lars

**Panel meeting**
- to IMPRS and arrange the studentship in UHH, filling out some forms from the TRR RTG. as soon as Chirs agrees
- first panel meeting: determine the meeting date → 5 pages of report, ask more experienced ppl (Nils)
    - 1 to 2 figures
    - intro, current status, what’s next
    - evaluate our plan
    - prepare for the questions in **MPI panel meeting** (no need to prepare presentation for the first time)
        - discuss do we need presentation in the next meeting (for discussion)

**Next steps (aside from reading)**
- running the omip with changed parameters, check the upwelling regions,or Peru, West Calinfornia, make comparisons in MLD, SST, SSS, profile of density TKE
- observation data: [https://www.cen.uni-hamburg.de/en/icdc/service/tutorials.html](https://www.cen.uni-hamburg.de/en/icdc/service/tutorials.html)
    - pick one year to compare
- read more papers about of the TKE mixing in upwelling regions → check some important stuffs to look at.

# [[2025-11-10]]
## Regular Meeting with Nils

- email to Stephan for the ICON-XPP, to do a reference simulation
    - and ask for a quick plots example scripts
- AMOC and other regions (costal upwelling regions)
- run coarser configuration coupled simulation, changing small details in mixing scheme, generate general plots to find more details/interesting to check
    - check the mixing and effects in the ocean/atmosphere and find the different between different mixing scheme
- have a look over $k-\epsilon$ scheme

# [[2025-11-10]] 
## Meeting with Noel
- why running uncoupled model? since coupled and uncoupled is such different
- how waves behave in the costal upwelling region by running simulations → give a view of what’s will this looks like
- WAVEWATCH II and III
- ERA5: download the wave related variables to have a sense of the wave
    - download the data and test by myself
    - or looking into some papers
- reading the papers about the waves. You will find more in the NWP field
    - reason: because it’s rather easy to find the research questions regarding the prediction of weather instead of the climate
        - some suggestions:
            - Qing Li et al., 2016 Langmuir mixing effects on global climate, how they design the experiment
            - Effects of Surface Turbulence flux parameterization on the MJO: the role of ocean surface waves
            - Turbulence in the Upper-Ocean Mixed layer: Eric
            - Dynamics of winds and currents coupled to surface waves

# [[2025-11-06]] 
## Additional meeting with Nils
- ‘main’ supervisor: need a professor
    - University side? or Bjorn
- Panel:
    - Noel (Group Leader in MPI-M)
    - Christopher Higgins (PostDoc at DWD)
    - Nils (‘co’-supervisor)
    - Jochem/Jin-son/Bjorn/
- 26 N 30 W → location for checking the TKE profile

## Meeting with Helmuth Haak
- OMIP is a climatological forcing , so the date doesnt mean anything. 2010 is the date of the initial state (oras5 from 2010-01), so therefor it makes sense to set your startdate also to 2010, but this is a matter of taste. If you use a transient forcing like ERA5 (with a real date) it becomes important. For OMIP it doesn’t matter.

# [[2025-11-05]] 
## ICON-Waves & Noel’s GM
### ICON-Waves Focus; 1st meeting
- meet regularly, and communicate with each other to avoid overlap.
- **Short overview of ICON-waves** (See details in [Short Overview of ICON-waves & Current status](https://www.notion.so/Short-Overview-of-ICON-waves-Current-status-2a269691c52b80c98923ff6269d6dfca?pvs=21))
- **ICON-waves: Wave-ocean coupling** (led by Christopher Higgins, see details in [Short Overview of ICON-waves & Current status](https://www.notion.so/Short-Overview-of-ICON-waves-Current-status-2a269691c52b80c98923ff6269d6dfca?pvs=21) )

### Noel’s regular group meeting
- Paper figure discussion, suggested paper to read:
    - paper: Heat Transport through Diurnal Warm Layer
    - paper: Global ocean heat transport dominated by heat export from the tropical Pacific

# [[2025-11-03]]
## Regular Meeting with Nils
- run shorter time (25-30 years) range for coupled configuration
- run uncoupled, 100 years
- try both: **r2b7-uncoupled (~30 yrs, OMIP-forcing (same year over and over again, useful to test something)**; r2b7-coupled
    - where is most affected btw coupled and uncoupled simulation
    - Then apply the **website**: automatically generate figures
- ICON-XPP: coupled, coupled with atmos model from DWD with ICOn-o, with resolution r2b7 (ocean), r2b5 (atmos)
- get fluent workflow (running, plotting), compare plots in the website
    - A useful way to manage diff. ICON version is to have a main directory containing the NAME for all diff ICON version (check ICON version using the below code:
        ```
        # inside the icon-model file
        # run the below code in bash
        module load git
        git log
        ```
        In the icon-model directory, you will find the running scripts in:
        ```
        /icon-model/run/exp_{run_name}.nc
        ```
        The {run_name} is better to have a similar naming structure as nibxxxx; muhxxxx  
- learn GIT, with the materials from Nils
- start register the PhD project at UHH (check the email for the materials)
- official supervisor:
    - Panel:
    - Check out RTG tings

# [[2025-10-27]]
## Regular Meeting with Nils
### General plan for the coming few weeks (27/10/2025 - 10/11/2025)
- **First stage: run ICON-o (check [ICON documentation](https://icon-o.gitlab-pages.dkrz.de/icon-o-documentation/))**
    - With two mixing schemes to evaluate the difference
        - TKE-scheme (default):
            
            → Check the slides from Nilsq
            
            → With the mixing length $L_m$ diagnostic
            
        - TKE-scheme with prognostic mixing length
            
    - Analysis based on surface forced WMT and AMOC (strength?)
- **Second stage: run ICON-TKK: a/o coupled, coarser resolution with same exp setting**

### Other topics
- Book recommendation: ocean Turbulence by Thrope
- referring back to textbook is important and useful
- TRR spring school

# [[2025-10-15]]
## Noel’s group meeting
- molecular viscosity and dynamic viscosity are different

# [[2025-10-07]]
## Discussion on ICON-O-Wave Experiments
‼️The reference experiment can be runned
- Full: combined all of them
- resolution: R2B7 20km, 72 layers, need to decide:forcing; simulation period; model output (e.g., MLD, TKE profile, Hs, wave direction
- ICON-Wave R2B7 grid is difference from the ICON-O
- Lots thing need to do: the Physics + codin
- **Maybe combine experiment (change some of the exps)**
    - exp 2: Stokes drift + langragian current
    - **Lars mention his choice of experiments:**
        - Possible first experiments: wave stress instead of wind stress
        - Langmuir turbulence need to be parameterized by adding a term in the TKE function ( the 3rd term at right hand side):
            ![[Attachments/Screenshot 2025-10-28 at 10.48.16.png|center]]
            
        - $\tau_{a}$: atmospheric stress term (frequent velocity), can we derive from the velocity shear btw atmosphere and ocean? We can test separately: by running two models (ICON-O and ICON-Wave, without coupling) to see if there’s dramatic changes
    - Wave stress ($\tau_{oc}$?) can be already calculated
        

## **Suggested version of choices of experiments and configurations**
- Difference between the ICONwave and the IFS-WAM?
    - The $\tau_{a}$ term in ECMWF is influenced by the mean state term (no wind, still has $\tau_{a}$)
    - tau in ICON-Wave is more influence by the wind term

## Meeting with Nils
- Read books (download online → check the chat with Nils)
    - Chapter about turbulent models —> chapter 11.2
- Read papers Nils suggested in the email
- Two aspects:
    - theories: basic theory —> physical part
    - Technical skills:
- Get familiar with VScode and **Git** —> nice backup system

# [[2025-10-06]]
## Regular Meeting with Nils
- Meeting after the wave meeting tomorrw 07/10/2025 11:00
    - Check the email from Nils
        - request some documents for the setup of the ICON-Wave during the meeting
- Discuss the plan for the coming weeks
- air sea coupling need to be highlighted —> WMT
- series of how wave model evolve, from not sophisticated to novelly
- research questions:
    - how air-sea coupling would change after the implementary of ICON-Wave
    - What’s the implication for the MOC, southern Ocean upwelling/mixing? atmosphere


