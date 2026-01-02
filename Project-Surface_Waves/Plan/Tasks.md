---
tags:
  - project/surfwaves
  - PhDs
  - task
---
# Status Tracking


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
	- [ ] select, merge and calculate time means 3D variables for last 20 years:
		- [x] `mux0001_b5b7`
		- [x] `mux001_b5b7_c_k-03`
		- [ ] `mux001_b5b7_c_k-10`
	- [ ] select and merge 2D variables for last 20 years
		- [x] `mux0001_b5b7`
		- [x] `mux001_b5b7_c_k-03`
		- [ ] `mux001_b5b7_c_k-10`
	
- [ ] Comparison of climatological fields between standard TKE scheme (`mux0001_b5b7`) and modified viscosity parameter (`mux0001_b5b7_c_k-03`) #project/surfwaves 
	- [ ] Differences in global climatological SST, SSS and MLD
	- [ ] Regional difference in profiles of potential Temperature, salinity, density
		- [ ] Define Regional masks

## [[2025-12-29]]
