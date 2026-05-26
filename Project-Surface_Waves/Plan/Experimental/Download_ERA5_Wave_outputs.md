---
tags:
  - project/surfwaves
  - wave/surface_wave
Last Eddited: 2026-05-25
---
# Download use Helmuth’s script
Target: download the below parameters from year 1980-2010
```python
declare -a variables
variable[0]='mean_wave_direction'
variable[1]='mean_direction_of_wind_waves'
variable[2]='mean_direction_of_total_swell'
variable[3]='mean_wave_period_based_on_first_moment'
variable[4]='mean_wave_period_based_on_first_moment_for_wind_waves'
variable[5]='mean_wave_period_based_on_first_moment_for_swell'
variable[6]='significant_height_of_combined_wind_waves_and_swell'
variable[7]='significant_height_of_wind_waves'
variable[8]='significant_height_of_total_swell'
variable[9]='u_component_stokes_drift'
variable[10]='v_component_stokes_drift'

declare -a var_short
var_short[0]="mwd"
var_short[1]="mdww"
var_short[2]="mdts"
var_short[3]="mp1"
var_short[4]="p1ww"
var_short[5]="p1ps"
var_short[6]="swh"
var_short[7]="shww"
var_short[8]="shts"
var_short[9]="ust"
var_short[10]="vst"
```

## Curren stage
- year `2000` seems to have problem:
	- `mdts` has no 20001001-20001231
	- `mdww` has no 20001001-20001231
	- `uts` has no 20000701-20000931
	- `vts` has no 20000701-20000931
- `p1ww_19970701_19970931` has 379 duplicate time
	- it has unique correct time range, but with duplicate content
	- the duplicate starts from 1997-09-01T00:00:00 to 1997-09-16T18:00:00