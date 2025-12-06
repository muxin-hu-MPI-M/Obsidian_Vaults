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
# Introduction

To parameterise the Turbulent Kinetic Energy (TKE) in the mixed layer is complex, and couldn’t revolve in coarser resolution. Hence, parameterisation is needed. The note here will summarise several key TKE parameterisation schemes that the ICON implemented already, or will implement in the future.

# The TKE-Scheme (ICON-o)

The TKE parameterisation scheme in ICON-O model.
![[Screenshot 2025-10-28 at 10.48.16.png |center]]


**The above equation is the same as below, referring to the ==Eq. 75== in Note:** [[Chapter_11-Small_Scale_Turbulence#Models based on the TKE Equation TKE_scheme]]:

$$ \begin{equation} \frac{\partial \varepsilon}{\partial t}=\frac{\partial}{\partial z}\bigg( c_E\varepsilon^{1/2}L\frac{\partial \varepsilon}{\partial z}\bigg)+ c_u\varepsilon^{1/2}L\bigg(\frac{\partial \overline{\mathbf u_h}}{\partial z} \bigg)^2-c_b\varepsilon^{1/2}L\frac{\partial \overline{b}}{\partial z}-c_\epsilon\frac{\varepsilon^{3/2}}{L} \end{equation} $$

Some explanation of the TKE-Scheme is listed in the following section.

## TKE Flux (Divergence) → Redistribution
The TKE redistribution in the mixed layer is expressed in the first term on the right-hand side of the above two equations. The term is simplified by assuming all three fluxes in the TKE flux term (i.e., divergence term) into one turbulent diffusion term:
$$ \frac{\partial}{\partial z}\bigg( k_e\frac{\partial E_{tke}}{\partial z}\bigg)

=\frac{\partial}{\partial z}\bigg( c_E E_{tke}^{1/2}L_{mix}\frac{\partial E_{tke}}{\partial z}\bigg) $$
with:
$$ k_e=c_E E_{tke}^{1/2}L_{mix}=\alpha_{tke}A_v $$

The turbulent viscosity $Av$ expression is summarised below.


## Eddy (Turbulent) Viscosity $A_v$
Represents the ==**ability of turbulence to mix momentum**==, which is a turbulence analogue of molecular viscosity.

It is appeared in the mean momentum equation as a parameterisation of the turbulence momentum flux (i.e., shear production term. Considering horizontal homogeneous, leading to $\overline{w}=0$). Also considering mixing length concept (See [[Chapter_11-Small_Scale_Turbulence#Mixing Length]]):
$$ \begin{split} -\overline{\mathbf u_h'w'}\frac{\partial \overline{\mathbf u_h}}{\partial z}&=-(-K_m\frac{\partial \overline{\mathbf u_h}}{\partial z})\frac{\partial \overline{\mathbf u_h}}{\partial z} \\

&=K_m(\frac{\partial \overline{\mathbf u_h}}{\partial z})^2 \end{split} $$
==**Where the $K_m$ is the eddy viscosity==.** In the TKE-Scheme, this viscosity is represented as:
$$ K_m=A_v=c_uE_{tke}^{1/2}L_{mix} $$
Thus, the shear production term (turbulent momentum flux):
$$ -\overline{u'w'}\frac{\partial \overline{\mathbf u}}{\partial z}=A_v(\partial_z(\overline{\mathbf u_h}))^2 $$


## Eddy (Turbulent) Diffusivity $k_v$
Represents the ability of turbulence to mix scalar quantities (e.g., temperature, salinity, density (or buoyancy)).

Which appears in the scalar transport equations. buoyancy production term in TKE equation (for mixed layer), which can be related to mean buoyancy profile (i.e., mixing length concept):
$$ \overline{b'w'}=-K_b\frac{\partial \overline {b}}{\partial z} $$
**Where the $K_b$ is the eddy diffusivity.** With TKE closure:
$$ K_b=k_v=c_bE_{tke}^{1/2}L_{mix} $$
And because the buoyancy and the buoyancy frequency is linked:
$$ b=-g\frac{\rho'}{\rho_0}\quad \text{and} \quad N^2=-\frac{g}{\rho_0}\frac{D\rho}{Dz}=\frac{\partial \overline{b}}{\partial z} $$
Thus, the buoyancy production term:
$$ \overline{b'w'}=-k_vN^2 $$
As you can see, the term ‘eddy diffusivity’ and ‘eddy viscosity’ is just two different names for the same concept originated in the mixing length concept in Turbulent mixing.
- The turbulent viscosity/diffusivity both represents the magnitude of mixing. Thus, a simple from of the turbulent flux can be obtained by ==**assuming that mixing is mainly caused by eddies with maximum energy, with a length scale== $L$** (integral length scale for large eddies), which yields $K\sim UL \sim \varepsilon^{1/2}L$
The main idea is to represent the turbulent flux into the mean gradient of the scalar quantity. While in the mixed layer, we assume the horizontal homogeneity, which only consider the vertical mean gradient.


## Mixing length
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

## Dissipation
Represents the conversion from TKE to thermal energy. Hence, **always negative**. The expression comes straight from ==***Kolmogorov’s theory***== (see [[Chapter_11-Small_Scale_Turbulence#11.1 Kolmogorov’s Theory of Homogeneous Turbulence]]) of turbulence and the idea of energy cascade:
$$ \epsilon=c_{\epsilon}\frac{E_{tke}^{3/2}}{L_{mix}}

$$

The $L_{mix}$ is the mixing length. Larger the $L_{mix}$, smaller the dissipation rate. And this is related to the turnover time for eddy $T_L\sim L_{mix}/\sqrt{E_{tke}}$, which is the time an eddy of a size $L_{mix}$ takes to mix with its surroundings. Then the dissipation rate $\epsilon \sim E_{tke}/T_L$.

It says: **the dissipation rate is proportional to the turbulent velocity scale cubed divided by the mixing length scale (to match the correct dimension)**

**It assumes:**

- Equilibrium (quasi-steady) state between energy production and dissipation
- Homogeneous, isotropic turbulence at small scales
- Single dominant length scale $L_{mix}$ governs the energy-containing eddies

For the actual ICON output of TKE and TKE tendency (change rate of TKE), please refers to the **“TKE Output Table” section in ICON Output Reference**
