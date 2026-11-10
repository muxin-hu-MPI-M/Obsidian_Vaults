---
tags:
  - ICON
  - ICON_parameterisation
  - project/Surfwaves
  - TKE
  - TKE_scheme
  - TKE_MLD
  - ICON-O
Last Eddited: 2025-11-29
---

# TKE-Scheme (ICON-o) #TKE_scheme 

## Introduction

To parameterise the Turbulent Kinetic Energy (TKE) in the mixed layer is complex, and couldn’t revolve in coarser resolution. Hence, parameterisation is needed. The note here will summarise several key TKE parameterisation schemes that the ICON implemented already, or will implement in the future.

## Summary

The TKE parameterisation scheme in ICON-O model.
![[Screenshot 2025-10-28 at 10.48.16.png |center]]


> [!Important] **At the right-hand-side, from 1st to 6th terms are:**
> 1. Vertical redistribution of TKE: vertical divergence of the diffusive TKE flux
> 2. Buoyancy production
> 3. Shear production
> 4. Surface forcing (might connect to surface wave breaking)
> 5. Turbulent dissipation into internal energy (heat)
> 6. Internal-wave energy input

| Term                                              | Physical interpretation                                        | Effect on TKE                                                                                   |
| ------------------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| (1) $\partial_z(k_e\partial_z E)$               | Vertical turbulent redistribution of TKE                       | Locally positive or negative; conserves column-integrated TKE in the absence of boundary fluxes |
| (2) $-k_vN^2$                                   | Buoyancy production or destruction                             | Positive if $N^2<0$; negative if $N^2>0$                                                    |
| (3) $A_v\|\partial_z\overline{\mathbf u}_h\|^2$ | Extraction of mean kinetic energy through vertical shear       | Nonnegative                                                                                     |
| (4) $F_{\mathrm{tke}}$                          | TKE input associated with surface forcing, often wave breaking | Usually positive                                                                                |
| (5) $-c_\epsilon E^{3/2}/L_{\mathrm{mix}}$      | Dissipation of turbulent kinetic energy into heat              | Negative                                                                                        |
| (6) $+\epsilon_{iw}$                            | TKE input associated with internal-wave energy dissipation     | Positive when active                                                                            |

**The above equation is the same as below, referring to the ==Eq. 75== in Note:** [[Chapter_11-Small_Scale_Turbulence#Models based on the TKE Equation TKE_scheme]]:

$$ \begin{equation} \frac{\partial \varepsilon}{\partial t}=\frac{\partial}{\partial z}\bigg( c_E\varepsilon^{1/2}L\frac{\partial \varepsilon}{\partial z}\bigg)+ c_u\varepsilon^{1/2}L\bigg(\frac{\partial \overline{\mathbf u_h}}{\partial z} \bigg)^2-c_b\varepsilon^{1/2}L\frac{\partial \overline{b}}{\partial z}-c_\epsilon\frac{\varepsilon^{3/2}}{L} \end{equation} $$

Some explanation of the TKE-Scheme is listed in the following section.

## Each Terms
### (1) TKE Flux (Divergence) → Vertical redistribution
==**It describes the vertical divergence of the diffusive TKE flux**==. 
The TKE redistribution in the mixed layer is expressed in the first term on the right-hand side of the above two equations. The term is simplified by assuming all three fluxes in the TKE flux term (i.e., divergence term) into one turbulent diffusion term:
$$ \frac{\partial}{\partial z}\bigg( k_e\frac{\partial E_{tke}}{\partial z}\bigg)

=\frac{\partial}{\partial z}\bigg( c_E E_{tke}^{1/2}L_{mix}\frac{\partial E_{tke}}{\partial z}\bigg) $$
with:
$$ k_e=c_E E_{tke}^{1/2}L_{mix}=\alpha_{tke}A_v $$

The turbulent viscosity $Av$ expression is summarised below.

If we define
$$ F_E=-k_e\partial_z E_{\mathrm{tke}}, $$
then
$$ \partial_tE_{\mathrm{tke}}\big|_{\mathrm{transport}} =-\partial_zF_E. $$
Integrating over a water column gives
$$ \int_{z_b}^{z_s}\partial_z(k_e\partial_z E)\,dz = -F_E(z_s)+F_E(z_b). $$
Therefore, **this term does not create or destroy energy within the interior**, but it can change the total TKE of a column through boundary fluxes.

This distinction matters particularly for surface-wave-driven turbulence: energy can enter at the surface and then be transported downward by this term.

### (2) Buoyancy Production
==**This is an energy exchange term between the turbulent kinetic and potential energy**==, not necessarily a pure production term. See details in [[Chapter_11-Small_Scale_Turbulence#Exchange with Mean Potential Energy]]
Buoyancy production term in TKE equation (for mixed layer), which can be related to mean buoyancy profile (i.e., mixing length concept):
$$ \overline{b'w'}=-K_b\frac{\partial \overline {b}}{\partial z} $$
> [!Cite] **Turbulent (eddy) diffusivity for buoyancy $k_v$**
> Where the $K_b$ is the eddy diffusivity. With TKE closure:
> $$ K_b=k_v=c_bE_{tke}^{1/2}L_{mix} $$
> - Where $$c_b=c_u/Pr$$, thus $Pr=c_u/c_b$, is the turbulent Prandtl number
> - Represents the ability of turbulence to mix scalar quantities (e.g., temperature, salinity, density (or buoyancy)).
> - The term ‘eddy diffusivity’ and ‘eddy viscosity’ is just two different names for the same concept originated in the mixing length concept in Turbulent mixing.
> - The turbulent viscosity/diffusivity both represents the magnitude of mixing. Thus, a simple from of the turbulent flux can be obtained by **assuming that mixing is mainly caused by eddies with maximum energy, with a length scale** $L$ (integral length scale for large eddies), which yields $K\sim UL \sim \varepsilon^{1/2}L$
> - The main idea is to represent the turbulent flux into the mean gradient of the scalar quantity. While in the mixed layer, we assume the horizontal homogeneity, which only consider the vertical mean gradient.
> - for $z$ increasing upward.
> 	- When $N^2>0$, the ocean is stably stratified. Turbulence must work against buoyancy to mix water vertically, converting TKE into potential energy.
> 	- When $N^2<0$, the water column is gravitationally unstable. Buoyancy drives convection, converting potential energy into TKE.

And because the buoyancy and the buoyancy frequency is linked:
$$ b=-g\frac{\rho'}{\rho_0}\quad \text{and} \quad N^2=-\frac{g}{\rho_0}\frac{D\rho}{Dz}=\frac{\partial \overline{b}}{\partial z} $$
> [!Important] Thus, the buoyancy production term: 
> $$ \boxed{\overline{b'w'}=-k_vN^2} $$


### (3) Shear Production
==**Represents a transfer of energy from resolved mean kinetic energy into unresolved turbulent kinetic energy**==. In TKE scheme, its parameterisation describes the interaction of turbulence with a mean shear (see [[Chapter_11-Small_Scale_Turbulence#Exchange with Mean kinetic Energy]]). The direction of this exchange is normally such that energy flows from mean to turbulent kinetic energy. 

Considering horizontal homogeneous, leading to $\overline{w}=0$). Also considering mixing length concept (See [[Chapter_11-Small_Scale_Turbulence#Mixing Length]]):
$$ \begin{split} -\overline{\mathbf u_h'w'}\frac{\partial \overline{\mathbf u_h}}{\partial z}&=-(-K_m\frac{\partial \overline{\mathbf u_h}}{\partial z})\frac{\partial \overline{\mathbf u_h}}{\partial z} \\

&=K_m(\frac{\partial \overline{\mathbf u_h}}{\partial z})^2 \end{split} $$
> [!Cite] **Eddy (Turbulent) Viscosity for momentum $A_v$**
> Where the $K_m$ is the eddy viscosity. In the TKE-Scheme, this viscosity is represented as:
> $$ K_m=A_v=c_uE_{tke}^{1/2}L_{mix} \sim \mathrm m^2 \mathrm s^{-1}$$
> Represents the **ability of turbulence to mix momentum**, which is a turbulence analogue of molecular viscosity.

> [!Important] Thus, the shear production term (turbulent momentum flux):
> $$ \boxed{-\overline{u'w'}\frac{\partial \overline{\mathbf u}}{\partial z}=A_v(\partial_z(\overline{\mathbf u_h}))^2} $$

#### Mixing length
Here the mixing length is defined as the displacement of a particle that is entirely supported by the amount of TKE (per mass), which will be transferred to potential energy (as the particle moves).

Derivation process:
First we care only the vertical displacement $\Delta z\approx L$, the density profile can be expressed as:
$$ \rho(z)=\rho_0+\frac{d\rho}{dz}\Delta z $$
Hence the buoyancy force is:
$$ F_b=-g\frac{\rho-\rho_0}{\rho_0}=-g\frac{d\rho}{dz}\frac{\Delta z}{\rho_0} $$
Since the buoyancy frequency is defined as:
$$ N^2=-\frac{g}{\rho_0}\frac{d\rho}{dz}\approx-\frac{g}{\rho_0}\frac{\partial \overline{\rho}}{\partial z} $$
The buoyancy acceleration becomes:
$$ F_b=-N^2\Delta z\approx -N^2L $$

The potential energy change is:
$$ E_p=\int_{0}^{\Delta z}N^2z'dz'=\frac{1}{2}N^2(\Delta z)^2\approx \frac{1}{2}N^2L^2 $$
Thus the mixing length (which is the displacement trigger by the TKE)
$$ L_{mix}=\sqrt{\frac{2E_{tke}}{N^2}} $$
### (4) Surface forcing
In ICON-O, the ==**surface forcing can represent energy input associated with surface wave breaking**==. Wind stress can also enter through a surface boundary condition on TKE or its vertical flux. But it is a simple parameterisation only related to the wind stress:
$$F_{\text{tke}}\sim f(|\tau_0|^{3/2})$$
This TKE forcing represents the rate at which energy enters the turbulent field


### (5) Turbulent dissipation
==**Represents the conversion from TKE to internal (thermal) energy through the turbulent energy cascade and ultimately molecular viscosity**==. Hence, **always negative**. The expression comes straight from ==🔴***Kolmogorov’s theory***== (see [[Chapter_11-Small_Scale_Turbulence#11.1 Kolmogorov’s Theory of Homogeneous Turbulence]]) of turbulence and the idea of energy cascade:
$$ \boxed{\epsilon_{\text{tke}}=c_{\epsilon}\frac{E_{tke}^{3/2}}{L_{mix}}}$$

The $L_{mix}$ is the mixing length. Larger the $L_{mix}$, smaller the dissipation rate. 

> [!Cite] **Derivation of turbulent dissipation in TKE-scheme**
> the turnover time for eddy:
> $$\tau_{\text{eddy}}\sim L_{mix}/\sqrt{E_{tke}}$$
> which is the time an eddy of a size $L_{mix}$ takes to mix with its surroundings with the velocity $U\sim \sqrt{E_{tke}}$. 
> Then the dissipation rate:
> $$\epsilon \sim E_{tke}/\tau_{\text{tke}} \sim \frac{E_{\text{tke}}}{L_{\text{mix}}/\sqrt{E_{\text{tke}}}}\sim \frac{E_{\text{tke}}^{3/2}}{L_{\text{mix}}}$$.
> The scaling argument does not determine an exact numerical prefactor. Then, a dimensionless coefficient $c_{\epsilon}$ is introduced, depends on the definitions of the characteristic turbulent velocity and mixing length, as well as the assumptions and calibration of the closure. Finally, the turbulent dissipation term in TKE scheme is: 
> $$ \boxed{\epsilon_{\text{tke}}=c_{\epsilon}\frac{E_{tke}^{3/2}}{L_{mix}}}$$

It says, the dissipation rate is proportional to the turbulent velocity scale cubed divided by the mixing length scale (to match the correct dimension)

**It assumes:**
- Equilibrium (quasi-steady) state between energy production and dissipation
- Homogeneous, isotropic turbulence at small scales
- Single dominant length scale $L_{mix}$ governs the energy-containing eddies

### (6) Internal-wave energy input
This one is particularly important for ocean interior mixing. Internal waves can lose energy through breaking and other dissipative processes. Some of that energy is transferred into small-scale turbulence, supplying TKE.

Schematically,
$$ E_{\mathrm{internal\ waves}} \xrightarrow{\epsilon_{iw}} E_{\mathrm{tke}} \xrightarrow{\epsilon_{\mathrm{tke}}} E_{\mathrm{internal\ heat}}. $$

This explains why $\epsilon_{iw}$ enters the TKE equation with a positive sign, despite often being called internal-wave dissipation. In coupled TKE–IDEMIX formulations, this term links the internal-wave energy budget to the turbulence budget.

For the actual ICON output of TKE and TKE tendency (change rate of TKE), please refers to the **“TKE Output Table” section in ICON Output Reference**

# Gent-McWilliams parameterisation (GM90) (ICON-o)
## Summary (AI-generated)
The **Gent-McWilliams (GM) parameterisation** (often referred to as Get-McWilliams or GM90) is a fundamental mathematical technique used in ocean general circulation models (OGCMs) and climate models to represent the effects of unresolved, small-scale ocean eddies (mesoscale eddies) on large-scale ocean currents and tracer transport. 

Its primary potential meaning and function is to ==**act as a sink for available potential energy by "flattening" isopycnals (surfaces of constant density) and reducing horizontal density gradients**==, simulating how eddies mix the ocean without relying on computationally expensive, high-resolution simulations. 

Here is a breakdown of the potential meanings and impacts of the GM parameterisation: 
1. Core Purpose: Representing Eddy Activity 
	- ==**Adiabatic Transport:**== The GM parameterisation models the adiabatic (meaning it does not create or destroy density, but moves it) transport of tracers, such as temperature and salinity, by mesoscale eddies.
	- ==**Bolus Velocity:**== It introduces an "eddy-induced velocity" or "bolus velocity" ($u^*$) that moves tracers, which is added to the normal ocean current velocity ($u$)
	- ==**Flattening Isopycnals:**== The main physical effect is to flatten sloping isopycnals, which releases available potential energy (APE) created by baroclinic instability, a process critical for balancing the ocean's large-scale circulation. 
2. Major Impacts on Climate Models 
	- **Improved Ocean Circulation:** Implementing GM reduces the need for "flux corrections" (adjustments made to keep models from drifting) in climate simulations.
	- **Better Simulation of the Ocean:** It significantly improves the representation of the Meridional Overturning Circulation (MOC), the Antarctic Circumpolar Current (ACC), and deep water formation.
	- **Reduction of Spurious Mixing:** Unlike traditional horizontal diffusion, which creates false, artificial mixing (upwelling), GM provides a physically grounded way to mix tracers along isopycnal surfaces. 
3. Key Components and Variations 
	- **Thickness Diffusivity ($\kappa_{\text{GM}}$)**:often called the isopycnal "thickness diffusivity" or "GM diffusivity".
	- **GM/Redi Framework:** GM is frequently combined with the **Redi parameterization** (Redi, 1982). While GM handles the _advective_ transport (movement), Redi handles the _diffusive_ mixing along isopycnals.
	- **Modern Advancements:** While the original 1990 GM scheme used a constant $\kappa$, modern models often use spatially and temporally varying $\kappa$ values (e.g., Visbeck et al. or GEOMETRIC) that depend on local ocean conditions (like stratification or eddy kinetic energy).
