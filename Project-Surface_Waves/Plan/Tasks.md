---
tags:
  - project/surfwaves
  - PhDs
  - task
---
# Status Tracking

- [x] **Literature Reading I**: Narrowing scientific gap in the field of surface waves #project/surfwaves 
	- What are surface waves?
	- What are the remaining gaps? → Wave-induced processes on air-sea interactions in the upwelling system (see details in [1st Panel Report](Panel-Meeting_Note)
	- How do we model surface waves?
- [x] Familiarise myself with ICON-XPP
- [x] 1st Panel Meeting
- [/] Literature Reading II
- [x] Update: narrowing scientific questions and PhD outline
- [ ] Experiment: Evaluate impacts of different turbulence mixing efficiency parameter `c_k` to the Peru-Chile Upwelling System (PCUS)
	- [x] Experiment design: (1) CTRL (default `c_k=0.1`); (2) CK03 (`c_k=0.3`)
	- [x] Simulation
		- [x] spin-up for 30-years; Less output
		- [x] focus period: later 20-years with detailed output
	- [ ] Analysis


# Weekly Plan 



## [[2026-06-22]]
- [ ] Panel Meeting: #project/PhD_general 
	- [ ] Update the progress report
		- [x] what’s to include? discuss with Nils during meeting
		- [x] A new sypnosis
		- [x] check with Nils/Noel
		- [ ] Thesis synopsis: declare that “1st part of the thesis”
		- [ ] Status report: replace Fig 1&2 with volume transport, but mention the tracer transport
		- [ ] Plane until next AP
		- [ ] Gnatt plot
	- [ ] Slides for 10 mins short presentation

## [[2026-06-15]]
- [x] Pipeline: era5 wave forcing to ICON-o; Check information [[Regular-Meeting_Note#Meeting with Nils-Arne (DKRZ)]] #project/surfwaves 
	- [x] modify ICON code to ‘receive’ the new forcing variable
		- [x] modification
		- [x] compile to see if okay
	- [x] add the wave provider script in the run script, test if it works
		- [x] compile the icon with levante.intel
	- [x] missing value problem
		- [x] change wave provider, assign the NaN values in source data to 0.0
		- [x] fix the sanitise function to handle the missing value -9999 to 9999
		- [x] implement the interpolation method for wave data

## [[2026-06-08]]
- [ ] renew the payment method for Swiss tele and Zotero
	- [x] swiss tele
	- [ ] zotero
- [ ] Pipeline: era5 wave forcing to ICON-o; Check information [[Regular-Meeting_Note#Meeting with Nils-Arne (DKRZ)]] #project/surfwaves 
	- [x] prepare wave data into yearly chunk `wave2d.json`, matching with same structure as `atm2d.json` under pool
	- [x] create new python provider for wave data
		- [x] first version
			- [x] try dry run (agent did that), save it into log file
			- [x] Modify run script
			- [x] try online test (maybe do this uses interactive mode, on lower resolution)

## [[2026-06-01]]
- [ ] explore the era5 force icon script #project/surfwaves 
	- [x] build icon executable on icon ocean 
	- [x] read and understand the python provider that read the data
	- [x] email to Nils
	- [x] prepare json files for both atm2d and wave2d
- [x] Finish reading Rühs et al., 2025
## [[2026-05-25]]
- [x] Download wave data using Helmuth’s script #project/surfwaves 
	- [x] test: 1980, 1981
	- [x] change to seasonal parallel?
	- [x] check data
	- [x] merge to yearly chunk
	- [x] check yearly chunk, do I have all NaN values?
- [x] Re-starting WMT project #project/WMT 
	- [x] Restart working
		- [x] Update Figure 4 
		- [x] Figure 5
			- [x] explicitly output Q_h, alpha
				- [x] piControl
				- [x] 4xCO2
				- [x] Check if match with previous results
			- [x] determine if decomposition matches with true variability
				- [x] annual mean climatology
				- [x] JFM mean climatology
			- [x] Absolute relative contribution map
	- [x] prepare slides
- [x] climate dynamic assignment 2 #project/PhD_general 
## [[2026-05-18]]
- [x] Summarise and assess the current Method: reconstructing Stokes profile
	- [x] DECOM: apply decomposition, when cannot, fallback to total-sea profile
	- [x] TOTAL: apply total-sea profile everywhere (i.e., no decomposition)

## [[2026-05-11]]
- [x] Re-starting WMT project #project/WMT 
	- [x] Guest account activate
	- [x] Re-login to JupyterLab
	- [x] Check status [[Meeting_Note_WMT]]
	- [x] Restart working
		- [x] find the maximum AMOC_sfc in picon and respo, saved to fileemen
		- [x] Update Figure 1
		- [x] Update Figure 2
		- [x] Update Figure 3
		- [ ] Continue in 2026-05-25
- [x] Stokes profile reconstruction: decomposition #project/surfwaves 
	- [x] Determine decomposition strategy
	- [x] Email to Chris for discussion
	- [x] Test on strategy
		- [x] if match with Stokes velocity magnitude
		- [x] masks: collinear case, non-collinear case (can apply/cannot apply decomposition)
	- [x] Reconstruction
		- [x] 2024 Jan
		- [x] Test

## [[2026-05-04]]
- [x] Retrieve Stokes profile from the ERA5 wave analysis according to (Breivik et al., 2014) #project/surfwaves 
	- [x] Download the hourly output for the correct wave data for reconstruction (test 2024)
	- [x] inverse depth scale to e-folding depth scale
		- [x] figure out why the current situation has negative inverse depth scale
		- [x] correct it, discussed with Chris
	- [x] Summarise the “Approximate Stokes drift”
		- [x] monochromatic
		- [x] Breivik 2014: Exponential integral
		- [x] Breivik 2016: Based on exact solution for the Phillips spectrum
	- [x] test full reconstruction on 2024 January
		- [x] No separation
			- [x] Exponential
			- [x] Phillips
		- [x] Separate wind waves and total swells
			- [x] test if can directly use → NO!, near zero denominator
			- [x] two regimes
				- [x] near-parallel and anti-parallel cases
				- [x] try 10/5 degrees 
			- [x] Test for 2024 Jan

## [[2026-04-27]]
- [ ] c_k experiments analysis #project/surfwaves 
	- [x] Interpolate atmospheric 3D variables to pressure coordinates
	- [x] Area averaged wind field
		- [x] monthly mean 10 wind speed 
		- [x] pressure gradient (using pyicon); the atmospheric tgrid (`r2b5_atm_r0030` has some key missing parameters which prevent the calculation of `cell2edge_coeff_cc`)
			- [x] can solve by updates the ds_IcD file using the DWD grid parameters
		- [x] monthly mean wind profile at lowest atmosphere
	- [ ] TKE-related difference: climatology/seasonality; surface and in-depth profile of sections
	- [ ] air-sea interaction (e.g., fluxes) difference: climatology/seasonality
- [x] Paper reading:
	- [x] review paper: Bremer & Breivik 2018
	- [x] Fujiwara et al. 2026

## [[2026-04-13]]
- [x] Prepare discussion note on Stokes Lagrangian transport to Nobu & Nils
- [x] Discuss with Chris: Reconstruct Stokes drift profile using ERA5
- [x] Update Figure 3 in project Peruvian Upwelling & Mesoscale Eddy #project/MesoEddy_Upwelling 
	- [x] Ask for the note
	- [x] Figure out what to do first
	- [x] Update the figure
	- [x] ‼️ correct the derivation of cross-shore velocity!
	- [x] compute climatological stratification (N2) and alongshore velocity & diff
	- [x] Write figure description and interpolation in the draft
- [x] Email to the buddy #project/PhD_general 
- [x] Read paper for discussion #project/PhD_general 
 
## [[2026-04-06]]
- [x] PhD registration complete (contact IMPRS office) #project/PhD_general 
- [x] Refine and summarise the updated idea: “***Isolating the Lagrangian transport effect of surface wave***” to an report #project/surfwaves 
	- [x] Stokes modified Lagrangian transport
	- [x] Stokes modified Lagrangian transport + Coriolis-Stokes?

## [[2026-03-16]]
- [x] Condense information for the idea of “Stokes transports of heat” to wave person #project/surfwaves 
	- [x] diagram of idea (background, significance and how to trying to implement)
	- [x] story line
- [x] Understanding idealised experiment #project/surfwaves 
- [x] Update the “Stokes transport of heat” slides #project/surfwaves 
	- [x] Answer Nils’ questions
		- [x] divergence free?
			- [x] global
			- [x] Pacific ocean specific, get a zonally meridional Stokes transport
		- [x] Stokes transport formula, staggering
		- [x] how I aggregate in time

## [[2026-03-09]]
- [x] compare ERA5 wave simulation to ICON-wave standalone simulation and update the plot in global case #project/surfwaves 
	- [x] compare surface stokes drift (ust, vst) with geological location;
		- [x] scatter plot (icon vs era5 points by points)
		- [x] global RMSE map
	- [x] compare significant wave heights for total waves, sea waves, and swells
		- [x] scatter plots
		- [x] global RMSE
	- [x] wave direction
- [x] preparing slides for “heat transport by Stokes drift and associated Lagrangian heat/mass convergence” #project/surfwaves 
- [x] register the Spring School #project/PhD_general 


## [[2026-03-02]]
- [x] update the Peru coast mask in atmospheric grid (r2b5) using the correct lat/lon grid point #project/surfwaves 
	- [x] first try using shape file
	- [x] ask Fraser to see if his script also apply to atmospheric grid: No, need to implement another method
- [x] check the example output to see if the data is successfully simulated #project/surfwaves 
	- [x] atm_2d
	- [x] atm_3d
	- [x] oce_flx
	- [x] oce_upo
	- [x] oce_tke
	- [x] oce_bgt
- [x] proceed longer simulation for default `c_k=0.1`
- [x] proceed to have 5-years simulation for `c_k=0.3`
- [x] Inquire Lorenz for the experiment with difference `c_k`

## [[2026-02-23]]
- [x] Studentship registration at UHH #project/PhD_general 
	- [x] Step 1: prepare documents
		- [x] copies of degree certificates (Bachelor+Master)
		- [x] transcripts of records
		- [x] Copy of identification document (Passport, VISA)
		- [x] CV
		- [x] diploma supplement
		- [x] high school certificate
		- [x] comparability form
		- [x] supervision agreement
		- [x] research project outline (check with Nils)
	- [x] Hand-in documents in DOCATA
- [x] prepare residence permit documents #project/PhD_general 
- [x] Summarise the estimation results for the “Stokes transport of heat across Indo-Pacific Equator” into sides #project/surfwaves 
	- [x] Cross-equatorial OHT by stokes drift in 
		- [x] Indo-Pacific 
		- [x] Pacific 
		- [x] Indian
	- [x] Climatological zonally integrated OHT by stokes drift in different basins, plot the OHT vs Latitude
- [x] Simulations with detailed output in different `c_k` experiments #project/surfwaves 
	- [x] Determine the final output list: strategy (listed in [[ICON_Output_Namelist]])
	- [x] 5-years test Simulation on `c_k=0.1 (default)`
	- [x] check if the simulation is okay
	- [x] Longer simulation
		- [x] `c_k=0.1`
		- [x] `c_k=0.3`

## [[2026-02-09]]
- [x] Record the idea thoroughly in Obsidian note #project/surfwaves 
	- [x] Update research questions recorded in [[ICON_Output_Namelist#Start from Research Questions]] to [[Research-Gaps]]
	- [x] Update the potential ideas
- [x] Analysis on ERA5 reanalysis with wave outputs (2000-2024) #project/surfwaves 
	- [x] ocean stress vs wind stress (normalised ocean stress)
	- [x] surface stokes drift velocity (magnitude + direction)
	- [x] Estimated Stokes Transport (see Eq. 31 in [[Breivik_2014&2016_Note_Stokes-Drift-Profile]]
	- [x] Estimated Stokes transport vs Estimated Ekman transport
		- [x] Estimate the total Stokes transport
		- [x] Estimate Ekman transport 
			- [x] without wave $\tau_{oc}=\tau_a$ 
			- [x] with wave $\tau_{oc}=\tau_a - \tau_{in} - \tau_{diss}$
	- [x] create the regional mask
	- [x] regional mean: analysis on seasonality

## [[2026-02-02]]
- [x] check the ocean output namelist, and make a summary for the future analysis #project/surfwaves 
- [x] Check the parameterisation scheme: “Gent McWilliams (GM90)” #project/surfwaves 
- [x] Update the atmospheric mask based on the new coordinates #project/surfwaves 
- [x] Discussion with Christopher #presenter/Christopher_Higgins about the current status if the wave-ocean coupling, and some basic physics related to the wave influence on ocean currents (e.g., refraction)


## [[2026-01-26]]
- [x] make the `individual section` in ocean r2b7 grid #project/surfwaves 
	- [x] Nils’ method
	- [x] Fraser's method
- [x] make the `adjacent_cells` along the pre-defined sections (in `edge` space) for the project: eddy and upwelling #project/MesoEddy_Upwelling 
- [x] crop the `tgrid` file based on the defined masks #project/surfwaves #project/MesoEddy_Upwelling 
	- [x] r2b9 - `pc_middle`
	- [x] r2b7 - all masks
- [x] adapt Xinyue’s script to match with Andrea’s `crop_function` #project/MesoEddy_Upwelling 
	- [x] calculate trends for T and N2
	- [x] figure of compare
- [x] **Note taking**: summarise the project #project/MesoEddy_Upwelling 
	- [x] Summarise how to efficiently use the defined sections and section-bounded regional mask. Check `/home/m/m301254/proj_surfwave/scripts/test.ipynb`
	- [x] summarise the story line for the project


## [[2026-01-19]]
- [x] finish the AI use test stated in Email #project/PhD_general 
- [x] make a namelist that I need for future simulation #project/surfwaves 
- [x] Create the Peru upwelling region masks for project mesoscale-upwelling #project/MesoEddy_Upwelling 
	- [x] ocean: `r2b9`
	- [x] atmos: `r2b8`
	- [x] make notes and mention this during meeting that the mask might need another version for the cross-sections
- [x] Ask Andrea for the script of cross-sections 
- [x] Creates the sections for two masks (`pc_all, pc_middle`) #project/MesoEddy_Upwelling #project/surfwaves 
	- [x] r2b7
	- [x] r2b9
- [x] Update the ocean regional masks based on the corresponding defined sections #project/surfwaves #project/MesoEddy_Upwelling 
	- [x] r2b7:
	- [x] r2b9


## [[2026-01-12]]
- [x] Define the shape files/masks for the cross-sections of the Peruvian coast #project/surfwaves 
	- [x] shape files → txt files
	- [x] save the mask into netcdf file
- [x] Comparison of atmospheric climatological fields between standard TKE scheme and modified viscosity parameters #project/surfwaves 
	- [x] wind speed at 10 m
	- [x] SLP
- [x] Learn how to upload and commit the `/home/m/m301254/proj_surfwaves/` to my *github* repository


## [[2026-01-05]]
- [x] Define regional mask #project/surfwaves 
	- [x] check Jin’s script
	- [x] apply land/ocean mask to ignore land components
	- [x] separate different parts
		- [x] general
		- [x] north, middle, south
		- [x] near shore
	- [x] define the masks on b5/b7 separately
		- [x] r2b5
		- [x] r2b7
	- [x] Save the masks into netcdf4 file
	- [x] Update the masks in ocean grid, by using the `cell_sea_land_mask` variable in `tgrid` file as reference
	- [x] new `oce_mask_3d`: considering the depth space, by using `wet_c` variable in `.fx` file


## [[2025-12-29]]
- [x] Comparison of oceanic climatological fields between standard TKE scheme (`mux0001_b5b7`) and modified viscosity parameters (`mux0001_b5b7_c_k-03` and `mux0001_b5b7_c_k-10`) #project/surfwaves 
	- [x] Differences in global climatological SST, SSS and MLD
		- [x] SST
		- [x] SSS
		- [x] MLD
	- [x] Differences in regional climatological SST, SSS, MLD
	- [x] Regional difference in profiles of potential Temperature, salinity, density
		- [x] Define Regional masks for the target area (`lon_reg=[-83, -76]; lat_reg=[-14,-4]`)
		- [x] Regional mean profiles
			- [x] to
			- [x] so
			- [x] rhopot
	- [x] Summarise into slides for meeting


## [[2025-12-22]]
- [x] Simulation: standard ICON-XPP piControl output; Year 1300-1350 #project/surfwaves 
	- [x] `mux0001_b5b7`
	- [x] `mux0001_b5b7_c_k-03`
	- [x] `mux0001_b5b7_c_k-10`
	
- [ ] Simulation with additional TKE output; Year 1350-1355 #project/surfwaves 
	- [x] `mux0001_b5b7` Simulation with additional TKE output: 1350-1355
	- [ ] `mux0001_b5b7_c_k-03` Simulation with additional TKE output: 1350-1355
	
- [x] Data post-processing: Select target 3D/2D variables, merge and time-average with `cdo` #project/surfwaves 
	- [x] select 3D variables: to, so, rhopot, mass_flux
	- [x] select 2D variables: tos, sos, mld
	- [x] select, merge and calculate time means 3D variables for last 20 years:
		- [x] `mux0001_b5b7`
		- [x] `mux001_b5b7_c_k-03`
		- [x] `mux001_b5b7_c_k-10`
	- [x] select and merge 2D variables for last 20 years
		- [x] `mux0001_b5b7`
		- [x] `mux001_b5b7_c_k-03`
		- [x] `mux001_b5b7_c_k-10`









