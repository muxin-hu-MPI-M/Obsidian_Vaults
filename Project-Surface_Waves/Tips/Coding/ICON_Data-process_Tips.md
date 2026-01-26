---
tags:
  - ICON
  - ICON-O
  - ICON/experiment
  - code/tips
Last Eddited: 2026-01-13
---
# Calculate Spatial Average
## when not consider wet_c in fx.file
- The ICON natural grids have different grid area (in $m^2$), which can be found in the corresponding `tgrid.nc` with the variable `[‘cell_area’]` 
- The difference is more significant in lower resolution configurations in ICON
- Thus, when calculating the spatial average of a scalar quantity, one need to consider the cell area to make sure the correct calculation. The usual workflow would be:
	- **make sure the dimension `[‘ncell’]` or `['cell']` matches**; Otherwise the data size will explode
```
tos_ave = (tos * ds_tg.cell_area * mask).sum(dims='ncell')/ (ds_tg.cell_area * mask).sum(dims='ncell')
```

## Consider `wet_c`, more precise
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



# Mask defined using Fraser’s method
## Descriptions
the files (for r2b7) can be found:
- **regional mask**: `/work/mh0033/m301254/proj_surfwaves/masks/secs_to_mask/pc_all_sections_masks_oce_r2b7.nc`
    - the region name is `pc_all`, which stands for the _general Peruvian Coastal region_
    - Description:
        - the masked **cell index** is stored in the variable `contained_cells`, with the shape of the number of selected cells
        - Because the `contained_cells` is defined as the region enclosed by the 4 boundaries (i.e., "sections"), the variable `edge_path` contains the **edge index** for the whole 4 boundaries; Similarly, the variable `vertex_path` contains all the **vertex index** that connect all 4 boundaries.
        - the `path_orientation` is the array that contains the direction (degrees) of the corresponding individual edge, which can be used when calculating fluxes through the edge.
    - Usage:
        - select the masked cell by e.g., `tos.sel(ncells=regional_mask["contained_cells"])`.
- **separate masks for 4 boundaries**: `/work/mh0033/m301254/proj_surfwaves/masks/secs_to_mask/pc_all_sections_{direction}BoundarySection_oce_r2b7.nc`
    - the `{direction}` in the above path can be: north, west, south, east. Each boundary has one nc file.
    - Description:  
        - similar to the **regional mask**, it contains the `edge_path`, `vertex_path`, `path_orientation`. However, Unlike those listed in the regional mask (i.e., master file), each of these variables contain the information only for the corresponding boundary.
- **individual sections perpendicular to the coast**: `/work/mh0033/m301254/proj_surfwaves/masks/secs_to_mask/individual_sec/all_sections_r2b9_NilsVersion.nc`
    - the file is generated through Nils' `pyicon` script.
    - Description:
        - it contains the `edge_mask` for each individual section, with the name as e.g., `mask_sectionX`. Be aware that each `edge_mask` contains the full dimension, and has the edge orientation attached. Thus, each `edge_mask` is an **array of -1, 0, +1, in the shape of number of all edges in r2b9 grid**.
        - the `ie_sectionX` and `iv_sectionX` are the **indexes for selected edge**. Notice that only the **valid edge/vertex has positive index**, invalid edge/vertex has negative `ie` and `iv`

## Wrap up
For efficient calculation on both regional mean or edge flux transport, using the cell_index and edge_index by:
```python
# regional mask (cell_mask)
region_mask = xr.open_dataset("/work/mh0033/m301254/proj_surfwaves/masks/secs_to_mask/pc_all_sections_masks_oce_r2b7.nc")

# boundary mask (edge_mask)
north_boundary_mask = xr.open_dataset("`/work/mh0033/m301254/proj_eddy_upwelling/masks/secs_to_mask/pc_middle_sections_NorthBoundarySection_oce_r2b7.nc")

# doing the regional average

# select the masked cell and guarantee they are ocean cell (i.e., "wet cell")
to_selected = to.isel(ncells=ncells_selected).compute()   # when using dask
cell_area_selected = tgrid["cell_area"].isel(ncells=ncells_selected)
wet_c_selected = tgrid["wet_c"].isel(ncells=ncells_selected)

# calculate the regional mean by considering the slightly different cell_area
to_ave = (to_selected * cell_area_selected * wet_c_selected).sum(dim="ncells") /  (cell_area_selected * wet_c_selected).sum(dim="ncells")
```
