---
tags:
  - ICON
  - ICON-O
  - ICON/experiment
  - code/tips
Last Eddited: 2026-01-13
---
# Basics
## Calculate Spatial Average
### when not consider wet_c in fx.file
- The ICON natural grids have different grid area (in $m^2$), which can be found in the corresponding `tgrid.nc` with the variable `[‘cell_area’]` 
- The difference is more significant in lower resolution configurations in ICON
- Thus, when calculating the spatial average of a scalar quantity, one need to consider the cell area to make sure the correct calculation. The usual workflow would be:
	- **make sure the dimension `[‘ncell’]` or `['cell']` matches**; Otherwise the data size will explode
```
tos_ave = (tos * ds_tg.cell_area * mask).sum(dims='ncell')/ (ds_tg.cell_area * mask).sum(dims='ncell')
```

### Consider `wet_c`, more precise
- when calculating the regional mean value for 3D fields (especially over coastal region where the **bathymetry** is important), it need special treatment for safety reason.
- because the mask has 2 values: `mask==1.0 or mask==0.0 -> False`, so the regional average formula stated in above may encounter few problems:
	- **Zero values are ambiguous and contaminate the mean**
		- In ocean models, values of `tos = 0.0` can mean a real physical value or a placeholder for land / dry cells; The old formula cannot distinguish between the two. As a result:
			- Land points with `tos = 0.0` artificially reduce the regional mean
			- The bias increases near coasts and at deeper levels
	- **The formula can silently fail at some depth levels**
		- At depths where the region contains no ocean cells. This make the denominator to be zero
- Thus, a safer choice of doing the regional mean, based on the current version of ocean mask (1.0 for ocean, 0.0 for land):
```
# For 2d surface field (ncells)
tos_ave = (tos * cell_area * mask * wet_c.isel(depth=0)).sum(dim="ncell") / (cell_area * mask * wet_c.isel(depth=0)).sum(dim="ncell")

# For 3d field (depth, ncells)
to_ave = (to * cell_area * mask * wet_c).sum(dim="ncell") / (cell_area * mask * wet_c).sum(dim="ncell")
```
- Will the 2D `cell_area` and `mask` have problem when processing 3D fields (with dimension of (depth, ncells))?
	- NO! In `xarray`, operations are dimension-aware, not position-based



# Regional Cell Mask & Section Edge Mask
## Regional Mask (cell_mask)
**Defined using Fraser’s package: `iconspy`**, please find the script in:
`/home/m/m301254/project_surfwaves/scripts/make_sections_and_bounded_region.ipynb`
### Descriptions
the files (for r2b7) can be found:
```python
# regional mask (cell_mask)
regional mask_path = "/work/mh0033/m301254/proj_surfwaves/masks/secs_to_mask/pc_all_sections_masks_oce_r2b7.nc"

# boundary masks (edge_mask)
directions = ["north", "west", "south", "east"]
boundary_masks = {}
for direction in directions:
	boundary_masks[di] = xr.open_dataset(f"/work/mh0033/m301254/proj_surfwaves/masks/secs_to_mask/pc_all_sections_{direction}BoundarySection_oce_r2b7.nc")

# individual sections
all_sections = xr.open_dataset("/work/mh0033/m301254/proj_surfwaves/masks/secs_to_mask/individual_sec/all_sections_r2b9_NilsVersion.nc")
```

- **regional mask**:
    - the region name is `pc_all`, which stands for the _general Peruvian Coastal region_
    - Description:
        - `contained_cells`: the masked **cell index** is stored in the variable, with the shape of the number of selected cells
        - `edge_path` and `vertex_path`: the **edge index** and **vertex index** for the whole 4 boundaries
	        - Because the `contained_cells` is defined as the region enclosed by the 4 boundaries (i.e., "sections")
        - `path_orientation` is the array that contains the direction (degrees) of the corresponding individual edge, which can be used when calculating fluxes through the edge.
- **separate masks for 4 boundaries**:
    - the `{direction}` in the above path can be: north, west, south, east. Each boundary has one nc file.
    - Description:  
        - similar to the **regional mask**, it contains the `edge_path`, `vertex_path`, `path_orientation`. However, Unlike those listed in the regional mask (i.e., master file), each of these variables contain the information only for the corresponding boundary.
- **individual sections perpendicular to the coast**: 
    - the file is generated through Nils' `pyicon` script.
    - Description:
        - it contains the `edge_mask` for each individual section, with the name as e.g., `mask_sectionX`. Be aware that each `edge_mask` contains the full dimension, and has the edge orientation attached. Thus, each `edge_mask` is an **array of -1, 0, +1, in the shape of number of all edges in r2b9 grid**.
        - the `ie_sectionX` and `iv_sectionX` are the **indexes for selected edge**. Notice that only the **valid edge/vertex has positive index**, invalid edge/vertex has negative `ie` and `iv`

### Apply in Calculation
#### crop `tgrid` file
For more efficient usage in **ICON native grid (“tgrid”)**, one need to crop the original tgrid file to the region masked. The `pyicon` provides the function to do so:
```python
def crop_tgrid_to_region(tgrid, mask):
	"""Crop tgrid to the region defined by the mask.
	Parameters
	----------
	tgrid : xarray.Dataset
	The tgrid dataset.
	mask : xarray.DataArray
	The mask defining the region to crop to.
	
	Returns
	-------
	xarray.Dataset
	The cropped tgrid dataset.
	"""
	# contained_cells: cell index of masked area
	ireg_c = mask["contained_cells"].astype(int)
	crop_tg = pyic.xr_crop_tgrid(tgrid, ireg_c)
	# build icon-readable interpolated grid
	ds_IcD = pyic.convert_tgrid_data(crop_tg)

	return ds_IcD, crop_tg

# example usage
tgrid = xr.open_dataset(fpath_tgrid["oce"])
mask_pc_all = xr.open_dataset("/work/mh0033/m301254/proj_surfwave/masks/secs_to_mask/pc_all_sections_masks_oce_r2b7.nc")
ds_IcD_pc_all, crop_tg_pc_all = crop_tgrid_to_region(tgrid, mask_pc_all)
```
- the `crop_tg` can be saved as individual netcdf file for later usage


#### Spatial Average
- the usual workflow is summarised below:
```python
# read cropped tgrid files
crop_tg = xr.open_dataset("/work/mh0033/m301254/proj_eddy_upwelling/masks/secs_to_mask/pc_all_mask_cropped_tgrid_oce_r2b7.nc") 
# transfer to ICON readable dataset 
ds_IcD = pyic.convert_tgrid_data(crop_tg) print("cropped tgrid has been created.") 

# Optional: keep selected ncells index (still 0-based) 
ncells_selected = crop_tg.cell.rename({"cell": "ncells"})

# select the masked cell and guarantee they are ocean cell (i.e., "wet cell")
to_selected = to.isel(ncells=ncells_selected).compute()   # when using dask
cell_area_selected = tgrid["cell_area"].isel(ncells=ncells_selected)
wet_c_selected = tgrid["wet_c"].isel(ncells=ncells_selected)

# calculate the regional mean by considering the slightly different cell_area
to_ave = (to_selected * cell_area_selected * wet_c_selected).sum(dim="ncells") /  (cell_area_selected * wet_c_selected).sum(dim="ncells")
```

### Plotting
When plotting using the `pyic.plot()`, it usually uses the full dimension of the data, so if using the `isel(ncells = ncells_selected`, the dimension doesn’t match. For this, a good way to avoid this is to expand the mask into full dimension.
The expanded masks can be found at:
```python
# example region: general Peru Coastal region
region_name = "pc_all"
full_mask = xr.open_dataset(f"/work/mh0033/m301254/proj_surfwave/masks/secs_to_mask/full_masks_{region_name}_oce_r2b7.nc")
```


## Individual Section Mask (edge mask)
The section mask can be created through: 
- Nils’ `pyicon` script: `/home/m/m301254/Download/pyicon/tools/tool_icon_section_transport_mask.py`
- Fraser’s `iconspy` script: `/home/m/m301254/project_surfwaves/scripts/make_sections_and_bounded_region.ipynb`
	- Fraser’s script is more useful when defining the regions based on the section. See details in [[ICON_Data-process_Tips#Descriptions]]

`pyicon` is a powerful tool to regulate difference kinds of masks, including the edge mask (or so-called sections). It can select the needed edge/vertex and modify the original **tgrid** file, and create a ICON-readable dataset, adjacent cells to the section for later usage.

For this, #presenter/Andrea_Mosso write a ~={red}**python script** which contains some useful functions=~ to:
1. **Maps a scalar from cell centres to edges** using the logic of the equivalent vector `pyicon` function
2. **Build interpolated tgrid (IcD) for a given section.**
3. **Interpolate scalar from cell to edges and restricted to a section.**
4. **Add a 'distance' coordinate to a section** DataArray based on edge longitude/latitude.
5. **Remap horizontal velocity components from cell centres to cell edges, returning the velocity normal to each edge on a cropped ICON grid**. This function takes zonal (`uo`) and meridional (`vo`) velocity components defined at cell centres and:
	- Ensures that the velocity fields are defined on the same (possibly reduced) set of cells as the cropped grid.
	- Rotates the horizontal velocity vector into the local grid coordinate system to obtain the normal velocity at cell centres.
	- Interpolates the cell-centred normal velocity to cell edges.
6. **Bin multiple section DataArrays by distance and stack along a new ‘section’ dimension.** Useful when calculating mean quantities between different sections.
### Example: 2D potential temperature trend
only core ideas are written below:
```python
from distributed import Client, LocalCluster,
import funcs_crop_section as functions

# read sections file
sections = xr.open_dataset("/work/mh0033/m301254/proj_eddy_upwelling/masks/secs_to_mask/individual_sec/all_sections_r2b9_NilsVersion.nc")

# build ICON section grids
ds_IcD_s1, adj_cells_s1, crop_tg_s1 = functions.build_section_IcD(ds_tgrid, sections, "mask_section1")

# ----------------------------------------------- #
# Functions:
#  - Select section related cells, edges
#  - Calculation: trend
#  - interpolate cell to edge
# ----------------------------------------------- #

# select the section
def select_section_TS(ds, wet_c, adj_cells, t0, t1, depth, var_name="to"):
	"""
	pre-select the adjacent points along the defined section (edge)
	and clean the data to make topography (bathymetry) = NaN.
	"""
	var = ds[var_name].sel(time=slice(t0, t1), ncells=adj_cells).isel(depth=slice(0,depth))
	wet = wet_c.sel(ncells=adj_cells).isel(depth=slice(0,depth)) # oce points == 1.0
	# clean the data; bathymetry=NaN
	var = var.where(wet > 0).load() # as .compute()
	# load to DataArray
	da = xr.DataArray(var,
					  dims=("time", "depth", "ncells"),
					  coords={"time": var.time, "depth": var.depth, "ncells": var.ncells},
					  name=var_name)
	return da

# calculation: trend
def trend_per_decade(da):
	"""
	Linear trend per decade.
	da: DataArray(time, depth, cell)
	"""
	t = da["time"].dt.year + (da["time"].dt.dayofyear - 1) / 365.25
	t = t.astype("float64")
	t0 = t - t.mean("time")
	y0 = da - da.mean("time")
	slope = (t0 * y0).sum("time") / (t0**2).sum("time")
	return slope * 10.0

# interpolates center variable (trend) to section edge
def section_trend(da_trend, sections, ds_IcD, crop_tg, section_name, var_name):
	"""
	interpolate the cell center data to cell edge
	using imported functions.center_to_section_edges_new
	"""
	tr_e = functions.center_to_section_edges_new(
		ds_IcD = ds_IcD,
		ds = da_trend,
		ds_tgrid = ds_tgrid,
		muxin_sec = sections,
		crop_tg = crop_tg,
		section_name = section_name,
		var_name=var_name,
	)
	tr_e = functions.add_section_distance(tr_e)
	return tr_e
	
# ----------------------------------------------- #
# Call functions
# ----------------------------------------------- #
# select
da_to_H_s1 = select_section_TS(ocn_H_m, ds_fx["wet_c"], adj_cells_s1, "1980-01-01", "2010-12-31", k300, var_name="to")
# calculate trend
T_to_H_s1 = trend_per_decade(da_to_H_s1)
# interpolate trend to section; add distance coordinate
T1_H = section_trend(T_to_H_s1, sections, ds_IcD_s1, crop_tg_s1, "mask_section1", "to")
```

### Boundary edge using Fraser’s script
We used **Fraser’s script** to define the regional boundaries and the corresponding regional cell mask. As a result, ~={red}the data structure of these boundary sections defined using this approach differ slightly from those produced by **Nils’ script**=~. In general, the output from Fraser’s script only contains the valid cell/edge indexes in the array.

Since Andrea’s Python script (see [[ICON_Data-process_Tips#Example 2D potential temperature trend]]) is designed to operate on the output format of Nils’ script, which contains the full-dimension for the section edges (i.e., full edge dimension in the corresponding configuration), **the output generated by Fraser’s method must be modified** to ensure compatibility for subsequent section analysis.

#### Modification
The below function is served to do the modification. Specifically, it does:
- expand the output from valid cell/edge index to the full dimension
- add boundary information (edge mask, edge index, vertex index) for each boundary section
- for the edge mask, attach the edge orientation information (degrees: 0.0/1.0)
The function can be found at: `/home/m/m301254/proj_surfwaves/scripts/make_mask_from_sections.ipynb`

```python
def build_cellmask_sectionmask_to_dataset(region_name, fpath_dir, grid_info, fpath_tgrid,):
	"""
	Build a dataset containing:
	1) A cell mask (ncells) defining the interior of a region
	2) Signed edge masks (edge) defining the region boundaries
	Cell mask:
	- Dimension: ncells
	- Values: 0.0 (outside), 1.0 (inside)
	- Land cells are removed
	  
	Edge masks:
	- Dimension: edge
	- Values: 0 → not part of the boundary; -1/+1 → boundary edge with orientation
	
	Parameters
	----------
	region_name : str
	Name of the region (e.g. "pc_all").
	fpath_dir : str
	Directory containing section mask NetCDF files.
	grid_info : str
	Grid identifier (e.g. "oce_r2b7").
	fpath_tgrid : str
	Path to ICON ocean tgrid file.

	Returns
	-------
	ds : xr.Dataset
	Dataset containing:
	- {region_name} (ncells)
	- {region_name}_north_boundary (edge)
	- {region_name}_south_boundary (edge)
	- {region_name}_east_boundary (edge)
	- {region_name}_west_boundary (edge)
	"""

	# read file
	sec_mask = xr.open_dataset(f"{fpath_dir}{region_name}_sections_masks_{grid_info}.nc")
	sec_north = xr.open_dataset(f"{fpath_dir}{region_name}_sections_NorthBoundarySection_{grid_info}.nc")
	sec_west = xr.open_dataset(f"{fpath_dir}{region_name}_sections_WestBoundarySection_{grid_info}.nc")
	sec_south = xr.open_dataset(f"{fpath_dir}{region_name}_sections_SouthBoundarySection_{grid_info}.nc")
	sec_east = xr.open_dataset(f"{fpath_dir}{region_name}_sections_EastBoundarySection_{grid_info}.nc")
	tgrid = xr.open_dataset(fpath_tgrid)
	ds = xr.Dataset()
	
	# Cell mask (ncells)
	mask_bool = tgrid.cell.isin(sec_mask["contained_cells"])
	mask = mask_bool.astype("float32").rename({"cell": "ncells"})
	
	# Remove land cells
	sea_mask = tgrid["cell_sea_land_mask"].rename({"cell": "ncells"})
	sea_mask_valid = (sea_mask == -2) | (sea_mask == -1)
	mask_corr = mask * sea_mask_valid
	ds[region_name] = xr.DataArray(
					mask_corr.data,
					dims=mask_corr.dims,
					coords=mask_corr.coords,
					attrs={
						"standard_name": f"{region_name}_mask",
						"long_name": f"Cell mask for {region_name} region",
						"units": "0.0, 1.0",
						"grid_type": "unstructured",
					},)
	
	# Signed edge masks
	boundaries = [
		(f"north_boundary", sec_north),
		(f"south_boundary", sec_south),
		(f"east_boundary", sec_east),
		(f"west_boundary", sec_west),]
	
	for boundary_name, sec in boundaries:
		edge_orientation = xr.zeros_like(tgrid["edge"], dtype="float64")
		edge_orientation.loc[dict(edge=sec["edge_path"])] = sec["path_orientation"]
		ds[boundary_name] = xr.DataArray(
			edge_orientation.data,
			dims=edge_orientation.dims,
			coords=edge_orientation.coords,
			attrs={
			"standard_name": boundary_name,
			"long_name": f"{boundary_name} edge mask (with direction) for {region_name}",
			"units": "degrees (-1.0, 1.0)",
			"grid_type": "unstructured",
			},)
		
		# edge index
		ie = xr.full_like(tgrid["edge"], fill_value=-1, dtype="int32")
		ie.loc[dict(edge=sec["edge_path"])]=sec["edge_path"]
		ds[f"ie_{boundary_name}"] = xr.DataArray(
			ie,
			dims=ie.dims,
			coords=ie.coords,
			attrs={
			"long_name": f"{boundary_name} valid edge index for {region_name} region",
			"values": "-1 (not in section), index (in section)",
			"grid_type": "unstructured",
			},)
	
		# vertex index
		iv = xr.full_like(tgrid["vertex"], fill_value=-1, dtype="int32")
		iv.loc[dict(vertex=sec["vertex_path"])] = sec["vertex_path"]
		ds[f"iv_{boundary_name}"] = xr.DataArray(
			iv,
			dims=iv.dims,
			coords=iv.coords,
			attrs={
			"long_name": f"{boundary_name} valid vertex index for {region_name} region",
			"values": "-1 (not in section), index (in section)",
			"grid_type": "unstructured",
		},)
	
	sec_mask.close()
	sec_north.close()
	sec_south.close()
	sec_east.close()
	sec_west.close()
	tgrid.close()
	
	return ds
```

the output for r2b7 configuration are saved in:
```python
outpath = "/work/mh0033/m301254/proj_surfwave/masks/secs_to_mask"
pc_all_masks.to_netcdf(f"{outpath}/full_masks_pc_all_oce_r2b7.nc")
pc_middle_masks.to_netcdf(f"{outpath}/full_masks_pc_middle_oce_r2b7.nc")
pc_north_masks.to_netcdf(f"{outpath}/full_masks_pc_north_oce_r2b7.nc")
pc_south_masks.to_netcdf(f"{outpath}/full_masks_pc_south_oce_r2b7.nc")
```

#### Use Andrea’s python script after modification
```python
# import Andrea's python script
import funcs_crop_section as functions

# read mask, tgrid for r2b7
mask_and_sections = xr.open_dataset("/work/mh0033/m301254/proj_surfwave/masks/secs_to_mask/full_masks_pc_all_oce_r2b7.nc")
ds_tgrid = xr.open_dataset("/home/m/m301254/pyicon_data/grids/r2b7_oce_r0005/r2b7_oce_r0005_tgrid.nc")

# use the script
ds_IcD, cells, crop_tg = functions.build_section_IcD(ds_tgrid, mask_and_sections, "north_boundary")

```
