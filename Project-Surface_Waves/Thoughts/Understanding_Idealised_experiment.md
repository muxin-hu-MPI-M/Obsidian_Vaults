---
tags:
  - project/surfwaves
  - project/MesoEddy_Upwelling
Last Eddited: 2026-03-16
---
# Concepts
## “Restoring”, “Nudging” and “Relaxation”
- Restoring (also called _nudging_ or _relaxation_) means the model **artificially pushes a variable toward a prescribed reference value**.
	- ~={red}**Example: (Kosaka & Xie, 2013):** =~
	  “In POGA experiments, the deep tropical eastern Pacific SST was restored to the model climatology plus historical anomaly by overriding the surface sensible heat flux to ocean (F) with: 
	  $$F = (1-\alpha)F_*+\alpha (cD/\tau)(T'-T_*') $$
	  where a prime refers to the anomaly, asterisks represent model-diagnosed values, and $T$ denotes SST. The reference temperature anomaly $T'$ is based on observation. The model anomaly $T_*'$ is the derivation from the climatology of a 300-year control experiment. $c$ is the specific heat of seawater, $D=50 \;m$ is the typical depth of the ocean mixed later, and tau is the restoring timescales. A weight $\alpha=1$ within the inner box, linearly reduced to zero in the buffer zone.”
	- **==Here the ~={red}restoring target variable=~ is SST in the deep tropical eastern Pacific==**. 
	- Mathematically, restoring means adding a forcing proportional to the difference between model SST anomaly and observed SST anomaly. The system is pushed toward $T'_* \rightarrow T'$ over a timescale $\tau$ by:
		- If the model SST anomaly is too warm ($T'_* \gt T'$), the restoring term removes heat from the ocean
		- If the model SST anomaly is too cold ($T'_* \lt T'$), the restoring term adds heat to the ocean
	- Even though the target variable is SST, **the model cannot directly set temperature inside the ocean every time step because that would break the ocean dynamics**. Instead, models usually restore SST through surface heat fluxes. The surface heat flux is the **energy input into the ocean mixed layer**, so it controls SST evolution through the heat budget
