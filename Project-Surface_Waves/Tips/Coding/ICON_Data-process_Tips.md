---
tags:
  - ICON
  - ICON-O
  - ICON/experiment
  - code/tips
Last Eddited: 2026-01-13
---
# Calculate Spatial Average
- The ICON natural grids have different grid area (in $m^2$), which can be found in the corresponding `tgrid.nc` with the variable `[‘cell_area’]` 
- The difference is more significant in lower resolution configurations in ICON
- Thus, when calculating the spatial average of a scalar quantity, one need to consider the cell area to make sure the correct calculation. The usual workflow would be:
	- make sure the dimension `[‘ncell’]` or `['cell']` matches; Otherwise the data size will explode
```
to_ave = (to * ds_tg.cell_area * mask).sum(dims='ncell')/ (ds_tg.cell_area * mask).sum(dims='ncell')
```


