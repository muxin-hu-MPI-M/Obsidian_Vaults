---
tags:
  - Meeting
  - project/surfwaves
  - "#presenter/Noel_Gutierrez-Brizuela"
  - "#presenter/Nils_Brüggemann"
Last Eddited: 2026-01-13
---


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


# [[2025-12-16]]
## Regular meeting with Noel
#presenter/Noel_Gutierrez-Brizuela 


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

# [[2025-11-24]] 
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

![Screenshot 2025-10-07 at 10.12.42.png](attachment:efb3d116-6d0a-45fe-b184-50641a9f6a47:Screenshot_2025-10-07_at_10.12.42.png)

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

![Screenshot 2025-10-07 at 11.05.05.png](attachment:64d93962-09ed-4b48-a1a9-4c949f66ceb5:Screenshot_2025-10-07_at_11.05.05.png)

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


