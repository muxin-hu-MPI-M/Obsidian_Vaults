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
# Weekly Plan 
## [[2025-12-22]]
- [x] Simulation: standard ICON-XPP piControl output; Year 1300-1350 #project/surfwaves 
	- [x] `mux0001_b5b7`
	- [x] `mux0001_b5b7_c_k-03`
	- [x] `mux0001_b5b7_c_k-10`
	
- [ ] Simulation with additional TKE output; Year 1350-1355 #project/surfwaves 
	- [/] `mux0001_b5b7` Simulation with additional TKE output: 1350-1355
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

## [[2025-12-29]]
- [ ] Comparison of climatological fields between standard TKE scheme (`mux0001_b5b7`) and modified viscosity parameters (`mux0001_b5b7_c_k-03` and `mux0001_b5b7_c_k-10`) #project/surfwaves 
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
	- [ ] Summarise into slides for meeting

