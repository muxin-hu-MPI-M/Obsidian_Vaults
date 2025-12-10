---
tags:
  - Meeting
  - project/surfwaves
  - pro
Last Eddited: 2025-11-27T09:57:00
---


# [[2025-12-10]] Noel’s group meeting
- W.R. Young and Basile Gallet
- swell is predominantly linear → predictable (kind of) (surline.com → surf conditions of swell)
	- period: 6-30 s
	- group velocity: 5-20 m/s
	- wavelength: 50-1000 m
	- sea-surface slope: ~0.05-0.2
	- still consider shallow compared to the ocean depth
- energy flux = group velocity * Energy density $=\frac{\rho g^2 T \alpha^2}{8 \pi}$
	- T is the period
	- a is the amplitude
	- turns out to be large energy flux carried by swell (40 KW/m when T=10sec, a=1m) → good sites for wave power production
		- 25 km coastlines = 1 Gigawatt ~ 1 nuclear pwer plant
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
		- but swell seems not to follow great circles backtracking → errors?
		- why don’t wave follow great circles?
			- not because of the Stokes-Coriolis force!!
			- Young’s hypothesis: refraction by surface currents between the source and the receiver
				- wave can be influenced by the wind the ocean currents

# [[2025-12-08]] 
## Regular Meeting with Nils
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


# [[2025-12-03]] Noel’s group meeting
## Solve a Fluid dynamic equation
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

## Fourier Transform
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

# [[2025-11-25]] Regular Meeting with Noel
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

# [[2025-11-24]] Regular meeting with Nils.
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

# [[2025-11-24]] TRR181: Workshop; Ferrel Cell
- outline: the summary part from the material
- accommodation in Bremen (TRR181)

# [[2025-11-18]] ICON-XPP: T&F meeting
see details in [https://gitlab.dkrz.de/icon/icon-nwp/-/wikis/mkexp-20251118#make-experiments-mkexp-for-xpp---introduction-purpose-and-advantages](https://gitlab.dkrz.de/icon/icon-nwp/-/wikis/mkexp-20251118#make-experiments-mkexp-for-xpp---introduction-purpose-and-advantages)

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

# [[2025-11-17]] Regular Meeting with Nils
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

# [[2025-11-10]] Regular Meeting with Nils

- email to Stephan for the ICON-XPP, to do a reference simulation
    - and ask for a quick plots example scripts
- AMOC and other regions (costal upwelling regions)
- run coarser configuration coupled simulation, changing small details in mixing scheme, generate general plots to find more details/interesting to check
    - check the mixing and effects in the ocean/atmosphere and find the different between different mixing scheme
- have a look over $k-\epsilon$ scheme

# [[2025-11-10]] Meeting with Noel

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

# [[2025-11-06]] Additional meeting with Nils

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

# [[2025-11-05]] ICON-Waves & Noel’s GM

## ICON-Waves Focus; 1st meeting

- meet regularly, and communicate with each other to avoid overlap.
- **Short overview of ICON-waves** (See details in [Short Overview of ICON-waves & Current status](https://www.notion.so/Short-Overview-of-ICON-waves-Current-status-2a269691c52b80c98923ff6269d6dfca?pvs=21))
- **ICON-waves: Wave-ocean coupling** (led by Christopher Higgins, see details in [Short Overview of ICON-waves & Current status](https://www.notion.so/Short-Overview-of-ICON-waves-Current-status-2a269691c52b80c98923ff6269d6dfca?pvs=21) )

## Noel’s regular group meeting

- Paper figure discussion, suggested paper to read:
    - paper: Heat Transport through Diurnal Warm Layer
    - paper: Global ocean heat transport dominated by heat export from the tropical Pacific

# [[2025-11-03]] Regular Meeting with Nils

- run shorter time (25-30 years) range for coupled configuration
    
- run uncoupled, 100 years
    
- try both: **r2b7-uncoupled (~30 yrs, OMIP-forcing (same year over and over again, useful to test something)**; r2b7-coupled
    
    - where is most affected btw coupled and uncoupled simulation
    - Then apply the **website**: automatically generate figures
- ICON-XPP: coupled, coupled with atmos model from DWD with ICOn-o, with resolution r2b7 (ocean), r2b5 (atmos)
    
- get fluent workflow (running, plotting), compare plots in the website
    
    - A useful way to manage diff. ICON version is to have a main directory containing the NAME for all diff ICON version (check ICON version using the below code:
        
        ```bash
        # inside the icon-model file
        # run the below code in bash
        module load git
        git log
        ```
        
        In the icon-model directory, you will find the running scripts in:
        
        ```bash
        /icon-model/run/exp_{run_name}.nc
        ```
        
        The {run_name} is better to have a similar naming structure as nibxxxx; muhxxxx and find the
        
- learn GIT, with the materials from Nils
    
- start register the PhD project at UHH (check the email for the materials)
    
- official supervisor:
    
    - Panel:
    - Check out RTG tings

# [[2025-10-27]] Regular Meeting with Nils

## General plan for the coming few weeks (27/10/2025 - 10/11/2025)

- **First stage: run ICON-o (check [ICON documentation](https://icon-o.gitlab-pages.dkrz.de/icon-o-documentation/))**
    - With two mixing schemes to evaluate the difference
        - TKE-scheme (default):
            
            → Check the slides from Nilsq
            
            → With the mixing length $L_m$ diagnostic
            
        - TKE-scheme with prognostic mixing length
            
    - Analysis based on surface forced WMT and AMOC (strength?)
- **Second stage: run ICON-TKK: a/o coupled, coarser resolution with same exp setting**

## Other topics

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
            
            ![Screenshot 2025-10-07 at 10.22.43.png](attachment:5855362f-5356-41d2-a773-b5d31d743586:Screenshot_2025-10-07_at_10.22.43.png)
            
        - $\tau_{a}$: atmospheric stress term (frequent velocity), can we derive from the velocity shear btw atmosphere and ocean? We can test separately: by running two models (ICON-O and ICON-Wave, without coupling) to see if there’s dramatic changes
            
        
        Wave stress ($\tau_{oc}$?) can be already calculated
        
        ```
          ![Screenshot 2025-10-07 at 10.27.29.png](attachment:a7529848-7653-484d-bf16-ebb7500ce9f2:Screenshot_2025-10-07_at_10.27.29.png)
        ```
        

### **Suggested version of choices of experiments and configurations**

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

# [[2025-10-06]] Regular Meeting with Nils

- Meeting after the wave meeting tomorrw 07/10/2025 11:00
    - Check the email from Nils
        - request some documents for the setup of the ICON-Wave during the meeting
- Discuss the plan for the coming weeks
- air sea coupling need to be highlighted —> WMT
- series of how wave model evolve, from not sophisticated to novelly
- research questions:
    - how air-sea coupling would change after the implementary of ICON-Wave
    - What’s the implication for the MOC, southern Ocean upwelling/mixing? atmosphere


