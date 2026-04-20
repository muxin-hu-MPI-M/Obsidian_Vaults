---
tags:
  - project/PhD_general
  - project/General
  - climate_dynamics
  - Subject-Note
Last Eddited: 2026-04-20
---
# Course Notes
## [[2026-04-20]] 2nd Course
### Global Energy balance
#### Intro
- TSI total solar irradiance ~ $S_0 \approx 1360\;W\;m^{-2}$(used to solar constant) → shortwave radiation reaches the earth
- earth area absorb the radiation using the interception area: $A_{in}=\pi R^2$, where $R$ is the earth radius
- energy goes out area (the spherical area): $A_{out}=4 \pi R^2$ 
- Thus the $S_0/4=340\; Wm^{-2}$ at unit area
	- Considering solar variability, you have to consider the factor of 4
- the fraction of the reflecting shortwave radiation is called albedo, with the global average $\alpha = 0.3$
	- thus, available energy (what is coming in): $\frac{S_0}{4}(1-0.3)=238\;Wm^{-2}$
	- What is going out, long-wave radiation: $LW: H_{LW}=\sigma\tau_s^4$
		- according to Stefan-Boltzmann law: $\sigma=5.67\times10^{-8} Wm^{-2}$
- Then the **global energy balance**: $$\frac{S_0}{4}(1-\alpha)=\sigma\tau_s^4$$
	- where $\tau_s$ is the surface temperature in $K$, steady-state temperature: $$\tau_s=(\frac{S_0}{4}(1-\alpha))^{1/4}=255 \;K$$
	- clearly, something is missing, as the global mean surface temperature is about 278 K → **atmosphere**. the temperature of 255 K should be the earth is radiated as a blackbody
- Atmosphere has greenhouse gases: 
	- N2 (80%), O2(19%), Ar (→ noble gas): very hard to excite the gas by the phontos, as the structure is simple and stable, the gaps to break is high. → ~={red}“99% of the atmosphere is invisible to LW”=~
	- much easier to excite: CO2, H2O, NH3, CH4, ~={red}“absorb LW”=~, and also ~={red}“re-emit up & down”=~
		- ==**downward part is the Greenhouse (GH) effect**== → the downward radiation (SW and the additional re-emitted LW)
#### Idealised Layer(s) Model
- **Atmosphere-Layer idealised model**
	- cannot have layer that is entirely opaque to the LW
	- Assume a layer with temperature $\tau_A$ which is transparent to SW, opaque to LW (not penetrable). Below is the surface with temperature $\tau_s$
		- the layer radiates:
			- upward & downward: $\sigma \tau_A^4$
			- receives the LW from below: $\sigma \tau_s^4$ 
		- the surface receives: $\frac{S_0}{4}(1-\alpha)$ 
		- one solution is that, the net outgoing LW is initiated by the temp of $\tau_A$, thus: $\frac{S_0}{4}(1-\alpha)=\sigma\tau_A^4$, then the surface will receive the downward re-emitted radiation + the shortwave incoming radiation: $2\frac{S_0}{4}(1-\alpha)=\sigma\tau_s^4, \tau_s=303 K$
	- one more layer with $\tau_{A,1}$ and $\tau_{A,2}$, both transparent to SW and opaque to LW
		- above one: $\tau_{A,2}=255 K$, re-emit up & down
		- below one: $\tau_{A,2}=303 K$, re-emit up & down
		- surface: $\sigma\tau_s^4 = 3\frac{S_0}{4}(1-\alpha)$, the number 3 is the combination of SW, and 2 more re-emitted LW from the above two layers
- Axel Kleidon calculation:
	- absorbed SW: $SW_{abs}=165 Wm^{-2}$ 
	- re-emitted downward LW: $H_{LW,down}=343 Wm^{-2}$
	- the surface temperature: $\sigma \tau_s^4=SW_{abs}+H_{LW,down}=508 Wm^{-2}, \tau_s=308 K$ 
	- if we have surf turbulence: $\sigma \tau_s^4=SW_{abs}+H_{LW,down}-(H_s+H_L)=508-102=406 Wm^{-2}, \tau_s=291 K$
		- ~={red}turbulence heat transport: sensible heat flux and latent heat flux, which acts to transport heat from the surface to the atmosphere=~
		- the importance is introduced by Manabe (1964)
#### Grey atmosphere
- Grey atmosphere: $H_{LW}=\epsilon\sigma \tau_s^4$,where $\epsilon$ is the effective emissivity, ranging btw 0 and 1
- $\epsilon=1/2$ for single layer (transparent to SW, opaque to LW), as the energy received by the surface is $2\frac{S_0}{4}(1-\alpha)=\sigma \tau_s^4$, as the $H_{LW}$ has to be the total incoming radiation $\frac{S_0}{4}(1-\alpha)$, then the one can update the effective emissivity to $1/2$
- Observed effective emissivity: $\epsilon_{obs}=\frac{S_0}{4\sigma \tau_{obs}^4}(1-\alpha)=0.61$, bigger than the one-layer model
#### Climate Sensitivity parameter
- surf temp: $\tau_s = \overline \tau_s + T’$, where $|T'| \ll \overline \tau_s$ 
- the long-wave heat decomposition: $$\begin{align}H_{LW}(\tau_s)&=\epsilon\sigma(\overline \tau_s + T’)^4\\&=\epsilon\sigma\overline\tau_s^4+4\epsilon\sigma\overline\tau_s^3T'+...\\&=\overline H_{LW} + H_{LW}'\end{align}$$
- where $H_{LW}’=4\epsilon\sigma\overline\tau_s^3T'=BT'$ 
- ==climate feedback parameter:== → most important parameter in climate variability, a parameter connect how much warming to how much the anomalous long-wave radiation $$\lambda=B^{-1}=\frac{1}{4\epsilon\sigma\overline\tau_s^3}$$
- Empirical equation: $H_{LW}=A+BT$ (Budyko (1969)), where $A=200 W/m2$ and $B=2 W/m2/s$ is the inverse climate feedback parameter, then derive the function of albedo to temperature
	- 3 stages:
		- $T<T_1$: ice-covered → $\alpha(T)=\alpha_1$
		- $T>T_2$: ice-free → $\alpha(T)=\alpha_2$
		- $T_1<T<T_2$: partial ice cover → $\alpha(T)=\alpha_1-\frac{\alpha_1-\alpha_2}{T_2-T_1}(T-T_1)$, the albedo decreases with increasing temperature
	- $\frac{S_0}{4}(1-\alpha)=A+BT$ 

## [[2026-04-13]] 1st Course