---
tags:
  - PhDs
  - Seminars
Last Eddited: 2025-11-19
---
# [[2025-11-28]] New Group Leader Intros
## Multiscale Cloud Physics
presenter: #presenter/Franziska_Glassmeier
- mesoscale clouds; connect to the mesoscale dynamics
	- e.g., stratocumulus; trade cumulus
- when clouds start to rain, it is influenced by the large scale patterns and processes (from land, SST)
- One opportunity: patterns simplified to either bright block (clouds) or dark block (rained) -> **bi-stability** 
	- mesoscale cloud scale in the middle of smaller scale to large scale, and can be considered as bi-stability state
	- multiscale stochastic
- group goal: multi-scale evolution
## Theoretical Ocean Dynamics 
presenter: #presenter/Noel_Gutierrez-Brizuela
- the large scale ocean, past and present
	- simple dynamics (without good computation), prescribed density, smooth atmosphere
	- virtually intractable 
- recipe:
	- learn about a climate theory
	- is the ocean's role oversimplified
- Assumptions:
	- 1. upwelling creates the equatorial cold tongues
	- 2. Laminar flow in the subtropical ocean interior
		- storm-generated waves 9NIWs) power turbulence in the ocean interior
	- 3. Ekman currents mandate the Southern Ocean response to winds changes
		- eddy rich GCMs have shown this is not true
		- but it's so tempting to invoke Ekman every time we see the currents strengthen
## Large-scale Coupled Dynamics
presenter: #presenter/Moritz_Grunter
- investigate how the atmosphere's interaction with ocean and the land shapes the general circulation
- The Pacific SST pattern determines important aspects of the general circulation, precipitation, radiative feedback -> inherently **coupling problem**
	- Will need to consider the importance of land
- ==Land is a *blind* spot in pattern effect literature==
	- Land-sea thermal contrast determines the Walker-circulation trend
	- 4xCO2 only over land --> cooling in eastern Pacific qualitatively similar to observation trend in 1979-2020
		- northward move of ITCZ, strengthened subtropical high in Southern Pacific
## Scale Interaction Modelling group -SIM
presenter: #presenter/Hans_Segura_Cajachagua

## Climate Extremes
presenter: #presenter/Chao_Li

# [[2025-11-19]] ENSO Optimisation in the ICON XPP Earth System Model
presenter: #presnter/Dakuan_Yu
- strong El Nino in 2026 is predicted
- ENSO pattern: regression between NINo3.4 with SST anomaly
- MPI-ESM1-2-HR (old) get the pattern to far to the west compared to the observation → general for all climate model
- ENSO metric: Yann et al. (2015) → ENSO evaluation in climate models
    - CMIP6 model (mean) perform bad in SST negative feedback (weaker than obs)
    - also weak positive feedback
    - wide spread ensemble, but enhanced compared to CMIP5
- How ICON-XPP performs compared to obs
    - Strong precip bias in southern side of central-east Pacific
    - cold bias in SST
    - much Weak ENSO amplitude
        - peak ENSO in summer time → far from obs, where peak is in winter
    - **summary**: ICON-XPP still has common climatological bias, too weak ENSO feedback and ENSO amplitude
- Ensemble tunning experiments: tunning parameterisation parameters, without adding new parameterisation
    - entrainment parameter valid for dx=20km → cloud physics scheme
    - Prantdl number -> turbulence scheme
    - find the cost function from the 60 experiments with the combination of all single cost function —> find the smallest cost function —> the optimisation of parameters combination
    - the optimised one: improve the SST bias and SSH bias, but still perform bad in ENSO metric, compared to CMIP6
        - climatology: slightly close CMIP6 mean
        - metric: closer to the ENSO metrics
        - feedback: still in the range of CMIP6 ensemble mean
    - Compare to observation:
        - climatology: still away from the obs
        - metric: slightly better, but still away
        - feedback: slightly better, but still away
- The efficiency of parameter tunning is the same as doubling the resolution. at least the tunning is helpful.
- the major bias compare to the CMIP6 models in low resolution ICON-XPP comes from the bias in seasonality, instead of global mean.
- for lower (CMIP6 resolution), the MPI-ESM performs much better

# [[2025-11-12]] Chaos, Coupling and Climate: The Oceanic Pathway to Atmospheric Blocking
Presenter: #presenter/Jamie_Mathews (IMPERIAL)
- Observational analysis (ERA5)
    
    - conversation of atmospheric block and Gulf stream, see their connections
        
    - block; wave breaking in the jet stream results in extreme and persisitant heat waves (colocated) and cold spells (downstream)
        
    - Case study:
        
        - Induction: warm conveyor belt outflow aligns with start of blocking contour
            
        - low-level SH fluxes generated → creating negative PV anomalies in the PBL → convey negative PV to higher altitude by warm conveyor belt
            
        - low-level LH fluxes → moisture flux
            ![[Screenshot 2025-11-12 at 11.30.43.png]]
    - 4 months ahead of the blocking, warm oceanic mixed layer; cold oceanic layer after block, block removes heat from the ocean through heat fluxes
        
- test ECMWF IFS experiments
    
    - setup: block the surface turbulence (S+L) heat fluxes from the Gulf stream to see the downstream effect
    - results:
        - 30% reduction in blocking frequency across the whole NH, strongest over the storm track
        - reduction of duration, size and intensity, but increase in number of blocks detected → less PV anomalies merging? influence become stronger when increasing resolution
            - why increase? don’t sustain the PV anomalies → more PV negatives will be detected.
    - effect on the ocean:
        - increase the SH flux, reduction in LH flux; less eddy kinetic energy
        - reduction in the frequency of warm conveyor belt
        - increase in SH heat flux creates more pv outflow values?
- Chaos, coupling and Climate
  ![[Screenshot 2025-11-12 at 11.23.09.png]]
    - build a 3d model
        - find: warm water before blocking, cold water after, negative PV before the block in the boundary layer → transport to higher altitude
        - increased convection → stronger blocking → convection parameterisation need to be carefully considered


# [[2025-10-27]] How much AMOC weakening would cool Europe under climate change?
Note taking from the Seminar presentation given by #presenter/Eduardo_Asenjo and #presenter/Felix_Schaumann

- **Questions**
	- To what extent?
	- Underlying evidence of AMOC weakening?
	- spatial patterns? Other locations in EU general?
	- how does AMOC-related cooling interact with global warming??
- **Existing evidence**
	- from highly idealised experiments (i.e, freshwater hosing)
	- many do the forcing under PI background, not under global warming scenarios?
	    - Maybe combine freshwater hosing + global warming (increasing CO2)?
- **Focus of the study**
	- Concrete impacts of a given amount of AMOC weakening;
	- **transient** effects until 2100
	- Coherent modelling framework: across T, AMOC strength
	- **How much AMOC will cause the EU to cooling?**
- **Modelling framework**
	- MPI-ESM1.2-LR, ensemble members
	- Greenland hosing with diff. magnitude and ways (linear increasing hosing, or one-time hosing)
	- also do different GW scenario (SSPxx)``
- **Result**
	- wide range of AMOC strength and SST anomaly over Europe for different configurations
	- the correlation between the magnitude in EU mean cooling and AMOC weakening:
	    - Approximately linear
	    - The linear relationship is scenario independent!! (SSP xx)
	        - but ofc, higher CO2 emission, the mean T will still get to higher in EU, but the linear relationship persist
	- Beyond EU mean T change?
	    - net cooling: cooling per 1 Sv AMOC weakening subtracting the surface warming under SSP1.26 scenario
	    - e.g., in Hamburg: to deviate the T increase under SSP1.26 in Hamburg at 2100, we need the AMOC to weak 3.1 Sv
	- Net cooling this century is possible in large parts of Europe (if AMOC weakens xx%); Especially under low-emission scenarios
	- **However, still highly unrealistic** (as the hosing experiments are not coupled to the GCMs)
	    - But the idea is to generate an range for effects of AMOC to the EU mean T
- **Other paper’s perspective**
	- The results shows how GW will deviate the cooling effect from a weakening AMOC towards the EU (compare to Jackson et al., 2024)
	- Other papers usually do the hosing experiment under PI condition; And they aiming at few centuries afterwards → more equilibrium effects involved