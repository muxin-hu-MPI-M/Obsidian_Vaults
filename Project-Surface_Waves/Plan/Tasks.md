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

# [[2026-01-12]]
- [x] Define the shape files/masks for the cross-sections of the Peruvian coast #project/surfwaves 
	- [x] shape files → txt files
	- [x] save the mask into netcdf file
- [ ] Comparison of atmospheric climatological fields between standard TKE scheme and modified viscosity parameters #project/surfwaves 
	- [ ] wind speed at 10 m
	- [ ] SLP
- [x] Learn how to upload and commit the `/home/m/m301254/proj_surfwaves/` to my *github* repository