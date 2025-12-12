---
tags:
  - project/surfwaves
  - Meeting
Last Eddited: 2025-12-11
---
This is the first panel meeting for Muxin Hu as a PhD candidate.

# Basic Information

The first meeting would be discussing the PhD research plan/proposal. Some relevant documentations are listed below:
- First draft for the plan/proposal is summarised in [[Research-Gap_Information&Summary]].  
- First progress report, which is the report to update the progress, status, and future plan to the panel members, is saved as a PDF file:
  ![[Panel_Meeting_1_Progress_Report_Muxin 1.pdf]]

# [[2025-12-11]] Meeting Note

- Comments from Chris on ICON-Wave #ICON-Wave:
	- One of the major challenge for the ICON-Waves is *“where to put Stokes drift in”*; This is important since the Stokes drift is related to many features. For example: momentum flux; tracer flux
	- Thus, to put this component in different field may need extra attention; One potential choice would be placing it in the tracer transport; **The key is to follow a energy consistent way**
- ~={red}==**First Stage of My PhD Work**===~:
  This is considered as the ICON-Waves fully coupling is currently not ready for use;
	- **Sensitivity test of coastal upwelling to TKE scheme** (without the coupling of wave) → ongoing
		- Can add *complexity* after the first synthesis; For example, consider a local parametric surface-wave model where the Stokes drift is derived from the wind field alone, assuming that the wave field is in equilibrium with the local wind → hence, no coupling between the atmosphere-wave-ocean. See details in [[Project-Surface_Waves-General_Proposal]]
		- One can also use this simple local parametric model to investigate the relative role of local wind/remotely generated swell regarding the generation of stokes drift and associated influence on air-sea exchanges.
- ==**Potential project: Running ICON-Sapphire configuration with surface wave model.**==
	- This is very attractive, since the Sapphire configuration can better resolve the wind field; Some research questions would be:
		1. what’s the implication on the upwelling system? 
		2. Influence on the wave-upwelling relationship?
	- Sapphire also better resolve the convection systems, including tropical cyclone; So if the ICON-wave is applied:
		1. What’s the contribution of surface waves in a better resolved tropical cyclones (TCs_? 
		2. What’s the feedback mechanisms between surface waves and TCs?
		3. Which wave-induced processes seize the strongest influence on simulation of TCs?
- Potential research questions:
	- Interactions between surface waves and sub-mesoscale phenomena (e.g., sub-mesoscale eddies)
- **==ICON-Waves can use same lower resolution as ICON-a==**
	- can use the same resolution as the atmospheric model (ICON-a) → r2b5; This is because the **wave model is primarily driven by the wind field**, even though the wave field is designed to interact with both atmosphere and ocean.
	- while the ocean model can have higher resolution (ICON-o) → r2b7
	- Aiming at shorter simulation time when focusing on upwelling systems (~30 years)
- ~={red}**ICON-Waves vs. WAM**=~  #ICON-Wave #WAM
	- Stokes drift profile is coupled to the atmosphere/ocean