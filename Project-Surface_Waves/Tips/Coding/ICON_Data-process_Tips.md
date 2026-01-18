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

## Consider `wet_c`, more percise
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
	- 