---
tags:
  - project/surfwaves
  - wave/surface_wave
  - Subject-Note
Last Eddited: 2025-12-06
---
The note is referring to multiple source of information: 
- (Zhao et al., 2022) [@zhaoEffectsOceanSurface2022]
- For details in 3rd generation wave model: WAM (and ECWAM), details can be found:
	- (Janssen et al., 2013) [@janssenAirSeaInteractionSurface2013]
	- (Janssen, 2004) [@janssenInteractionOceanWaves2004] → **background information for IFS**
	- (ECMWF, 2024) [@ecmwfIFSDocumentationCY49R1202411] → **IFS document**
- For the usage of WAM model, please find:
	- (Wu et al., 2022) [@wuRedistributionAirSea2022]
	- (Wu et al., 2019) [@wuWaveEffectsCoastal2019]

> [!Important] 
> The below is the note and my comprehension about the numerical details in the wave model from ECMWF (i.e., ECWAM model)

----------
# The Kinematic Part of the Energy Balance Equation
## Basic Equations
### Basic of Waves
- Relationship between wavelength, wavenumber and frequency
	- wavelength: $\lambda$
	- wavenumber: $k=\frac{2\pi}{\lambda}$
	- period: $T$ → time for one full cycle → crest-to-crest
	- Frequency:
	    - ordinary frequency (cycles per second): $f=\frac{1}{T}$
	    - angular frequency (radians per second): $\omega=2\pi f=\frac{2\pi}{T}$

Please find the below contents referenced to Janssen, 2004
Janssen, P. (2004). _The Interaction of Ocean Waves and Wind_. Cambridge University Press.
### Action density spectrum
The most elegant formulation of the “energy” balance equation is in terms of he **action density spectrum** $N(\mathbf{k})$ which is the **energy spectrum** divided by the so-called intrinsic frequency $\sigma$. 

Given the formula:

$$
N(\mathbf{k})=\frac{gF(\mathbf{k})}{\sigma},\quad \quad \sigma=\sqrt{gk\tanh{(kh)}} \tag{1}
$$

Where the $F(\mathbf{k})$ is the **wavenumber spectrum**, gives the distribution of wave energy over wavenumber $\mathbf{k}$. Its integration over the wavenumber equals the wave variance, expressed in the square of ocean **surface elevation** $\eta$:

$$
\langle \eta^2\rangle=\int_{-\infty}^{\infty}F(\mathbf{k})\;d\mathbf{k}=m_0 \tag{2}
$$

Where the bracket $\langle \rangle$ stands for the ensemble average. $m_0$ is the so-called zero order moment of the spectrum and $m_0\sim \:\text{units:}\:m^2$. The integral over the wave spectrum is a measure for wave height (e.g., *significant wave height* $H_s=4\sqrt{m_0}$) 
The wave variance $\eta^2$ can be linked to the wave energy through:

$$
\langle E \rangle =\rho_w g \langle \eta^2\rangle=\rho_wg\int_{-\infty}^{\infty}F(\mathbf{k})\;d\mathbf{k} \tag{3}
$$

**Thus**, for wave groups with action $N$ have energy $\sigma N=gF$ and momentum $kN$.

### Balance Equation for Action Density Spectrum

Give the *spatial dependency* to this action density spectrum, let:
$$
\mathbf{z}=(x_1,x_2,k_1,k_2)
$$
be the combined four-dimensional vector. Thus, the most fundamental form of the transport equation for the action density spectrum $N(\mathbf{k},\mathbf{x},t)$:

$$
\frac{\partial}{\partial t}N+\frac{\partial }{\partial z_i}(\dot{z_i}N)=0 \tag{4}
$$

where $\dot{z_i}$ denotes the propagation velocity of a wave group in the four-dimensional phase space of $\mathbf{x}$ and $\mathbf{k}$. This equations holds for any field $\dot{z_i}$, and also for velocity fields which are not divergence-free in four-dimensional phase space.

In the special case when $\mathbf{x}$ and $\mathbf{k}$ represent a canonical vector pair (e.g., when they are the usual Cartesian coordinates), the propagation equations for a wave group (also know as the Hamilton-Jacobi propagation equations) read:

$$
\begin{align} 
\dot{x_i} &=\frac{\partial }{\partial k_i}\Omega \\
& \tag{5} \\
\dot{k_i} &=-\frac{\partial }{\partial x_i}\Omega 
\end{align}
$$

where the $\Omega$ denotes the dispersion relation:

$$
\Omega = \mathbf{k}\cdot\mathbf{u} +\sigma \tag{5}
$$

Because the field $\dot{z}$ for a continuous ensemble of wave groups is divergence free in four-dimensional phase space, thus, the transport equation for the action density may be expressed in the advection form:

$$
\frac{d}{dt} = \frac{\partial N}{\partial t}+\dot{z_i}\frac{\partial }{\partial z_i}N=0\tag{6}
$$

since $\frac{\partial}{\partial z_i}\dot{z_i}=0$ as divergence free. Thus, along a path in four-dimensional phase space defined by the Hamilton-Jacobi equations, the action density $N$ is conserved. 

> [!Important]
> This property holds only for canonical coordinates for which the flow divergence vanishes!!!

However, one should turn to the form of the action density balance equation (Eq. 3) in the flux form since it is more general and has the advantage. When one transforms from one set of coordinates to another there is no guarantee that the flow remains divergence-free and therefore the flux form of the action balance equation is the preferred starting point.

### Balance Equation in Spherical Geometry
> [!Attention] **Action Density In Spherical Geometry**
> We now consider the action density flux balance equation in the spherical geometry, i.e., consider the ~={red}spectral action density $\hat N (\omega, \theta, \phi,\lambda, t)$=~ with respect to angular frequency $\omega$ and direction $\theta$ (measured clockwise relative to true north) as a function of latitude $\phi$ and longitude $\lambda$.
> 
> 
> - **choice of angular frequency instead of wavenumber**: 
> 	- for a fixed topography and current, the frequency $\Omega$ is conserved when following a wave group, therefore the transport equation simplifies
> 	- In situ observation, it is much easier to obtain frequency spectrum, just requires the analysis of time-series (i.e., Fourier-Transform). 
> 		- But, many observations of the wavenumber spectrum have been obtained through remote sensing techniques.

Then, the conservation equation (i.e., transport equation) for $\hat N$ thus reads:

$$
\frac{d\hat N}{dt}=\frac{\partial }{\partial t}\hat N+\frac{\partial }{\partial \phi}(\dot{\phi}\hat N)+\frac{\partial }{\partial \lambda}(\dot{\lambda}\hat N)+\frac{\partial}{\partial \omega}(\dot{\omega}\hat N)+\frac{\partial }{\partial \theta}(\dot{\theta}\hat N) = 0 \tag{7}
$$

and $\dot{\omega}=\partial \Omega/\partial t$ the term involving the derivative with respect to $\omega$ drops out in case of time-independent current and bottom.

Finally, the action density $\hat N$ is related to the normal spectral density $N$ with respect to a local Cartesian frame $(x,y)$ through: $\hat Nd\omega d\theta d\phi d\lambda=Nd\omega d\theta dx dy$, or $\hat N=NR^2\cos{\phi}$ where $R$ is the radius of the earth. 

Hence, the transportation with respect to normal spectral density $N$ is:

$$
\frac{dN}{dt}=\frac{\partial N}{\partial t}+(\cos{\phi})^{-1}\frac{\partial }{\partial \phi}(\dot{\phi}\cos{\phi}\;N)+\frac{\partial }{\partial \lambda}(\dot{\lambda}\hat N)+\frac{\partial}{\partial \omega}(\dot{\omega}\hat N)+\frac{\partial }{\partial \theta}(\dot{\theta}\hat N) = 0 \tag{8}
$$

where, with the $c_g$ the magnitude of the wave group velocity:
- $\dot{\phi}=(c_g \cos{\phi}+V_0)/R$
- $\dot{\lambda}=(c_g \sin{\phi}+U_0)/R\cos{\phi}$
- $\dot{\theta}=(c_g \sin{\theta} \tan{\phi})/R+(\dot{\mathbf{k}}\times \mathbf{k})/k^2$
- $\dot{\omega}=\partial \Omega/\partial t$
represent the rates of change of the position and propagation direction of a wave packet. $U_0,V_0$ are the components of the current in northerly and easterly direction. ~={red}==**Eq. (8) is the basic transport equation which is used in numerical wave prediction**=~== (Find the Eq. (8) in (Eq. (2.81), Janssen, 2004))

## Properties of the Basic Transport Equation
### Great circle propagation on the globe
From Eq. (8) and the corresponding rates of change of the position and propagation direction, one can infer that in spherical coordinates the flow is not divergence-free. Considering the case of no depth refraction and no explicit time dependence, the divergence of the flow becomes non-zero because the wave direction, measured with respect to the true north, changes while the wave group propagates over the globe along a **great circle**. As a consequence wave groups propagate along a great circle. This type of refraction is therefore entirely apparent and only related to the choice of coordinate system.

### Shoaling
First finite depth effect in the absence of currents is discussed by considering some simple topographies. 
In he case of wave propagation parallel to the direction of the depth gradient. In this case, depth refraction does not contribute to the rate of change of wave direction $\dot{\theta}$ , because with Eq. (5), $\mathbf{k}\times\dot{\mathbf{k}}=0$. In addition, we take the wave direction $\theta$ to be zero so that the longitude is constant ($\dot{\lambda}=0$) and $\dot{\theta}=0$. For time-independent topography (hence $\partial \Omega / \partial t = 0$) the transport equation becomes:

$$
\frac{\partial N}{\partial t}+(\cos{\phi})^{-1}\frac{\partial }{\partial \phi}(\dot{\phi}\cos{\phi}\;N) = 0 \tag{9}
$$

where 

$$
\phi=c_g \cos \theta R^{-1}=\frac{c_g}{R} \tag{10}
$$

and the group speed only dependents on latitude $\phi$. If we focus on steady wave (hence $\frac{\partial N}{\partial t}=0$), we immediately find conservation of the action density flux in the latitude direction so that:

$$
\frac{c_g \cos \phi}{R}N=\text{const} \tag{11}
$$

If, in addition, it is assumed that the variation of depth with latitude occurs on a much shorter scale than the variation of $\cos \phi$, the latter term may be taken constant for present purposes. 
It is then found that the action density is inversely proportional to the group speed $c_g$ so

$$
N\sim\frac{1}{c_g} \tag{12}
$$

And if the depth is decreasing for increasing latitude, ==**conservation of flux requires an increase of the action density as the group speed decreases for decreasing depth**== (The normal wisdom now is that the group speed decreases for decreasing depth (Janssen, 2004)). This is called shoaling effect
### Refraction
The second example of finite depth effects that we consider is he refraction. Again, we assume no current and a time-independent topography. In the steady state the action balance equation becomes:

$$
(\cos \phi)^{-1}\frac{\partial}{\partial \phi}(\frac{c_g}{R}\cos \theta \cos \phi N)+\frac{\partial }{\partial \lambda} (\frac{c_g\sin \theta}{R \cos \phi}N)+\frac{\partial }{\partial \theta}(\dot \theta_o N)=0 \tag{13}
$$

where

$$
\dot \theta_o=(\sin\theta \frac{\partial}{\partial \phi}\Omega-\frac{\cos \theta}{\cos \phi}\frac{\partial}{\partial \lambda}\Omega)(kR)^{-1} \tag{14}
$$

The role of the $\dot \theta_o$ terms for the simple case of waves propagating along the shore can be discussed. Consider, therefore, waves propagating in a northerly direction (hence $\theta=0$) parallel to the coast. Suppose that the depth only depends on longitude such that is decreases towards the shore. The rate of change of wave direction is then positive as

$$
\dot \theta_o=-\frac{1}{kR \cos \phi}\frac{\partial }{\partial \lambda}\Omega>0 \tag{15}
$$

since $\partial \Omega / \partial \lambda<0$ (as approach towards the shore, topography changed, shallower water depth). Therefore, waves which are propagating initially parallel to the coast will turn towards the coast. ==**This illustrates that, in general, wave rays will bend towards shallower water**== resulting in, for example, focusing phenomena and caustics. This is called refraction effect.

> [!Attention] **Shoaling and Refraction in Coastal region**
> When waves are approaching shallow waters:
> - conservation of flux requires an increase of the action density. This phenomenon, which occurs in coastal areas, is called <font color="#ff0000">shoaling</font>.
> 	- Its most dramatic consequences may be seen when tidal waves, generated by earthquakes, approach the coast resulting in tsunamis.
> - Topography change with space, the dispersion relationship changed with space, the rate of change of wave direction will also change. This phenomenon is called <font color="#ff0000">refraction</font>
> 	- In general, wave rays will bend towards shallower water

### Current Effects
A horizontal shear may result in wave refraction; the rate of change of wave direction follows Eq. (13) by taking the current into account:

$$
\dot{\theta}_{c}=\frac{1}{R}\left(\sin \theta\left[\cos \theta \frac{\partial}{\partial \phi} U_{\phi}+\sin \theta \frac{\partial}{\partial \phi} U_{\lambda}\right]-\frac{\cos \theta}{\cos \phi}\left[\cos \theta \frac{\partial}{\partial \lambda} U_{\phi}+\sin \theta \frac{\partial}{\partial \lambda} U_{\lambda}\right]\right) \tag{16}
$$

where $U_{\phi}$ and $U_{\lambda}$ are the components of the water current in latitudinal and longitudinal directions. Considering the same example as in the case of depth refraction, we note that the rate of change of the direction of waves propagating initially along the shore is given by:

$$
\dot \theta_c=-\frac{1}{R\cos \phi}\frac{\partial }{\partial \lambda}U_{\phi} \tag{17}
$$

which is positive for an along-shore current which decreases towards the coast ($\frac{\partial}{\partial \lambda}U_{\phi}<0$). In this condition, the waves will turn towards the shore.
The most dramatic effects may be found when the waves propagate against the current. For sufficiently large current, wave propagating is prohibited and wave reflection occurs, the group velocity vanishes. Because of the vanishing group velocity, a large increase of energy at that location may be expected suggesting that wave breaking plays a role. 

## Summary for the Action Balance Equation
Note that a global 3rd generation wave model solves the action balance equation in spherical coordinates. By combining previous results, the action balance equation becomes:

$$
\frac{dN}{dt}=\frac{\partial N}{\partial t}+(\cos{\phi})^{-1}\frac{\partial }{\partial \phi}(\dot{\phi}\cos{\phi}\;N)+\frac{\partial }{\partial \lambda}(\dot{\lambda}\hat N)+\frac{\partial}{\partial \omega}(\dot{\omega}\hat N)+\frac{\partial }{\partial \theta}(\dot{\theta}\hat N) = S \tag{18}
$$

where
$$
\begin{align}
\dot{\phi}&=(c_g \cos{\phi}+U|_{\text{north}})R^{-1} \tag{19a} \\
\dot{\lambda}&=(c_g \sin{\phi}+U|_{\text{east}})(R\cos{\phi})^{-1} \tag{19b} \\
\dot{\theta}&=(c_g \sin{\theta} \tan{\phi})R^{-1}+\dot\theta_{\text{D}} \tag{19c} \\
\dot{\omega}&=\partial \Omega/\partial t \tag{19d}
\end{align}
$$
and 

$$
\dot \theta_D=\bigg( \sin \theta \frac{\partial }{\partial \phi}\Omega - \frac{\cos \theta}{\cos \phi}\frac{\partial}{\partial \lambda}\Omega \bigg)(kR)^{-1} \tag{20}
$$
and the $\Omega = \mathbf{k}\cdot\mathbf{u} +\sigma$ is the dispersion relationship. The right-hand-side is the source term, which is given by:

$$
S=S_{\text{in}}+S_{\text{ds}}+S_{\text{nl}}+S_{\text{bot}} \tag{21}
$$

representing the physics of wind inpuy, dissipation, nonlinear wave-wave interaction, and bottom friction.


-------------------------------
# Parameterisation of Source terms
For details in this chapter, please referring to the Chapter 3 in IFS Wave model documentation (ECMWF, 2024)

## Wind Input (“Wind stress”, “atmospheric stress”)
### Before CY46R1, June 2019 (Also the ICON-Wave version)
#### Input Source Term
The main conclusion in studies of the numerical solution of the momentum balance of air flow over growing surface gravity waves (by Janssen et al) was that the growth rate of the waves generated by wind depends on the ratio of friction velocity and phase speed and on a number of additional factors, such as the atmospheric density stratification, wind gustiness and wave age.

A realistic parameterisation of the interaction between wind and wave was given by (Janssen, 1991), a summary of which is given below. The basic assumption was that even for young wind sea, the wind profile has a logarithmic shape, though with a roughness length that depends on the wave-induces stress. 

The growth rate of gravity waves ($\gamma$) due to wind then only depends on two parameters, namely
$$x=(\frac{u_*}{c_p})\text{max}(\cos{(\theta-\phi),0}) \tag{22a}$$
and
$$\Omega_{\text{m}}=\frac{g\kappa^2z_o}{u_*^2} \tag{22b}$$
where:
- $u_*$ denotes the friction velocity, $c_p$ the phase speed of the waves, $\phi$ the wind direction and $\theta$ the direction in which the waves propagate. 
- $x$ is the parameter relates to the wave age ($u_*/c_p$). 
- The so-called **profile parameter** $\Omega_m$ **characterises the state of the mean air flow** through its dependence on the roughness $z_o$. Thus, through $\omega_m$ the growth rate depends on the roughness of the air flow, which, in its turn, depends on the sea state

A simple ~={red}==**parameterisation of the growth rate**===~ ($\gamma$) of the waves follows from a fit of numerical results presented in Janssen (1991). One finds:
$$ \frac{\gamma}{\omega}=\epsilon \beta x^2 \tag{23} $$
where:
- $\omega$ is the angular frequency
- $\epsilon$ is the air-water density ratio 
- $\beta$ the *Mile’s parameter*. In terms of the dimensionless critical height $\mu=kz_c$ (with $k$ the wavenumber and $z_c$ the critical height defined by $U_o(z=z_c)=c$) the Mile’s parameter becomes as recently extended to any water depth $h$: 
  $$\beta=\frac{\beta_m}{\kappa^2}\tanh{(kh)}\mu \ln^4(\mu), \quad \mu \le 1 \tag{24}$$
	- where the $\kappa$ is the von Karman constant and $\beta_m$ a constant.
	- In terms of wave and wind quantities $\mu$ is given as:
	  $$\begin{align}\mu &=\frac{1}{\kappa^2}(\frac{u_*}{c_p})^2\tanh{(kh)}\Omega_m\;\text{exp}(\frac{\kappa}{\hat x})\\ \hat x&=((u_*/c_p)+z_{\text{ff}})\cos(\theta-\phi) \end{align}$$
		- with $z_{\text{ff}}$ the wave age tuning parameter

Then, the ~={red}==**input source term**===~ $S_{\text{in}}$ of the *ecWAM* (also for ICON-waves) is given by:

$$ S_{\text{in}}=\gamma N \tag{25}$$

Where the growth rate $\gamma$ follows from Eq. (23) and N is the action density spectrum from Eq. (1).

#### Stress of Air Flow
The stress of air flow over sea waves, also called the **total air–sea momentum flux (wind stress → flux of horizontal momentum (per unit area) transferred from the wind to the air-sea interface.**) is written as:

$$ 
\begin{align} 
\tau_{\text{a}} &= \rho_a u_* |u_*| \tag{26}
\end{align}
$$

where:
- $\tau_a$: wind stress, is considered as the total momentum flux from the wind that applies to the air-sea interface
- $\rho_a$: density of the air in the atmospheric boundary layer
- $u_*$: air-side friction velocity

This ~={red}==**stress of air flow on sea waves**===~ depends on the sea state and from a consideration of the momentum balance of air it is found that the kinematic stress is given as (Janssen, 1991)

$$
u_*^2=\bar\tau_a =\bigg(\frac{\kappa\mathbf{U}(z_{\text{obs}})}{\ln(\frac{z_{\text{obs}}+z_o}{{z_o}})}\bigg)^2 \tag{27}
$$

where:
- $\bar \tau_a$ is the density-normalised wind stress: $\tau_a = \rho \bar \tau_a$. 
- $z_{\text{obs}}$ is the mean height above the waves (usually $10\;m$); $\mathbf{U}(z_{\text{obs}})$ is the wind velocity at this height.
- $z_o$ is the roughness length:
  $$\begin{align}z_o&=z_b/\sqrt{1-\frac{\tau_w}{\bar\tau_a}}\\&=\frac{\alpha u_*^2}{g\sqrt{1-\frac{\tau_w}{u_*^2}}} \tag{28}\end{align}$$

Here $z_{\text{obs}}$ is the mean height above the waves (currently 10 m), and $\tau_w$ is the stress induced by gravity wave, which is also so-called ~={red}==**wave stress** and **wave-induced stress**===~:

$$
\tau_w=\epsilon^{-1}g\int \gamma N(\omega, \theta) \mathbf{k}\;d\omega d\theta \tag{29}
$$
For the wave stress, it follows the below relation:
$$\begin{align} \tau_w&=\epsilon^{-1}g\int \gamma N(\omega, \theta) \mathbf{k}\;d\omega d\theta \\ &=\epsilon^{-1}g\int\frac{\gamma F(\omega, \theta)}{\sigma} \mathbf{k}\;d\omega d\theta \\&=\epsilon^{-1}g\int\frac{\mathbf{k}}{\omega}\gamma F(\omega, \theta)\;d\omega d\theta \end{align}$$
Since considering deep water limit, the intrinsic frequency $\sigma=\sqrt{gk}=\omega$. And it seems that $\gamma F=\hat S_{\text{in}}$, which will be discussed in [[Note_Wave-induced-stress_and_Young-waves#Recall the Action Density Balance Equation]]

In the roughness length term (Eq. (28)), the **background roughness** $z_b$ is discussed, it represents the impact of gravity-capillary short waves. Since CY49R1, the wind stress Eq. (25) was evaluated for the high frequency with a $f^{-5}$ tail of for gravity waves range until the gravity-capillary range where a simplified model for the gravity-capillary spectrum is used instead. Ultimately resulting in an estimate for $z_b$.
In practice, ~={red}**wave stress points in the wind direction as it is mainly determined by the high-frequency waves which respond quickly to changes in the wind direction**=~ (ECMWF, 2024).
The background roughness length (Eq. (24)) can link to *Charnock* relation:

$$
z_b=\frac{\alpha \bar\tau_{\alpha}}{g} \tag{30a}
$$

The dimensionless ***Charnock parameter*** $\alpha$ is not constant but depends on the sea state through the wave-induced stress before CY49R1:

$$
\alpha = \frac{\hat \alpha}{\tau}/{\sqrt{1-\frac{\tau_w}{\bar\tau_a}}} \tag{30b}
$$

where the $gz_b$ is the direct calculation of $z_b$. When combined with the renormalised growth rate, hese changes yields a reduction of the resulting *Charnock* parameter for storm wind conditions (above 20 m/s), which is correct to match the observational evidence that the *Charnock* parameter should reduce quite considerably under strong tropical winds.

### Since CY49R1, November 2024
Many things changed compare to the CY46R1:
- background roughness length $z_b$, roughness length $z_o$ 
- enhanced version of dimensionless Charnock parameter $\alpha$
- growth rate $\gamma=\gamma_0\frac{1+\rho_{\text{sp}}N_1}{1+\rho_{\text{sp}}N_2}$ 
- wind stress $\tau_a=\tau_v +\tau_{w,lf} +\tau_{w, hf}$

The idea for improvement is because:
- It was shown that the background roughness length (zb) from the original approach of Janssen can instead be estimated explicitly. For young, steep wind sea, zb almost vanishes, giving a reduced drag.
- it was also shown that for steep waves, the slowing down of the wind by waves is a nonlinear process
- Hence, the **growth rate of the waves by wind depends in a nonlinear fashion on the wave spectrum**. For strong winds, it is found that, as waves are typically steep, this nonlinear effect gives a further reduction of the wind input without the need for the adhoc sheltering coefficient tauwshelter.


**Early comment on “Momentum Flux into the Ocean”**
- When considering the wave model, the wind stress (total momentum flux from the wind) at the ocean surface is not only transferred directly into ocean interior — part of it goes into surface gravity waves. The wave is numerically considered as a ‘mediator’ between the atmosphere and ocean (Wu et al., 2019, 2022)
- Hence, the surface stress (momentum flux) felt by the ocean interior is the total surface stress applied by the atmosphere minus the net stress going into the waves,  (Janssen et al., 2013)
- “The momentum flux to the ocean column, denoted by $\tau_{oc}$, is the sum of the flux transferred by turbulence across the air-sea interface which was not used to generate waves ($\tau_a - \tau_{in}$) and the momentum flux transferred by the ocean waves due to wave breaking $\tau_{ds}$.” (ECMWF, 2024, p. 99)

## Dissipation due to wave breaking
**White capping dissipation** (often denoted $S_{ds}$​) is the loss of wave energy due to **wave breaking** in deep water, i.e. the formation of whitecaps on the ocean surface. Hasselmann (1973) summarised that: “It is generally believed that **white capping is the dominant dissipative mechanism in a wave field at moderate and higher wind speeds**, simply because other dissipative process such as molecular viscosity or turbulence appear to be inadequate to remove the energy which is know to be imparted to the waves by the wind”

Whitecapping occurs when:
- Wind-driven waves become too steep
- The crest becomes unstable
- The wave breaks in deep water, producing foam (“white caps”)
It is the wave breaking caused by wave steepness and wind–wave interaction.

Janssen et al. (1989) realised that the wave dissipation source function has to be adjusted in order to obtain a proper balance at the high frequencies. The dissipation source term of Hasselmann (1974) is thus extended as:

$$
S_{\text{ds}}=-C_{\text{ds}}\langle\omega\rangle(\langle k\rangle^2m_0)^2 \biggr[ \frac{(1-\delta)k}{\langle k \rangle}+\delta (\frac{k}{\langle k \rangle})^2 \biggr]N \tag{31a}
$$

where $C_{\text{ds}}$ and $\delta$ are constants, $m_0$ is the total wave variance per square metre (see [[Note_Wave-induced-stress_and_Young-waves#Action density spectrum]]), $k$ the wavenumber, and $\langle\omega\rangle$ and $\langle k \rangle$ are the mean angular frequency and mean wavenumber, respectively. 

**However**, this parameterisation gives unrealistic variations of the wind sea dissipation in the presence of swell (Ardhuin et al., 2007): the windsea dissipation can be much reduced by the addition of swell. This spurious effect contributes to the larger scatter in the western part of the ocean basins where are dominated by wind seas, with the occasional presence of swells.
Today’s understanding of wave breaking and swell dissipation processes, although not complete, have led to parameterisations in which the steepness is more local in spectral space (Ardhuin, 2024)

## Bottom dissipation
Dissipation owing to bottom friction is not discussed here because the details of its parameterisation were presented in Komen et al. (1994, chapter II) as well as the relative merits of this approach being fully discussed. We merely quote the main result:

$$
S_{\text{bot}}=-2C_{\text{bot}}\frac{k}{\sinh{(2kh)}}N \tag{32}
$$

Where the constant $C_{\text{bot}}=0.038/g$ and $h$ the water depth.

## Nonlinear Transfer
In Komen et al. (1994) the derivation of the source function $S_{nl}$, describing the **nonlinear energy transfer**, was given from first principles. For surface gravity waves the nonlinear energy transfer is caused by four resonantly interacting waves, obeying the usual resonance conditions for the angular frequency and the wave numbers.
owing to resonant four-wave interactions the rate of change of the action density spectrum $N=gF(\mathbf{k})/\omega$ (where $F$ is the wave variance spectrum) is given by:

$$
S_{\text{nl}}=4\int T^2_{1,2,3,4}\delta(\mathbf k_1+\mathbf k_2 - \mathbf k_3 -\mathbf k_4)R_i(\Delta \omega, t)[N_1N_2(N_3+N_4)-N_3N_4(N_1+N_2)]\;dk_{1,2,3} \tag{33}
$$

where the resonant waves $R_i(\Delta \omega,t)=\pi\delta(\omega_1+\omega_2-\omega_3-\omega_4)$ and $T_{1,2,3,4}$ is a known interaction coefficient. The evaluation of $S_{\text{nl}}$ therefore requires an enormous amount of computation because a 3D integral needs to be evaluated. In the past several attempts have been made to try to obtain a more economical evaluation of the nonlinear transfer. The approach that was most successful to date is the one by Hasselmann et al. (1985) [@hasselmannComputationsParameterizationsNonlinear1985a], reasons:
- the parameterisation is both fast
- respects the basic properties of the nonlinear transfer, such as conservation of momentum, energy and action
- produces proper high-frequency spectrum
See details in Chapter 3.3 in (ECMWF, 2024) [@ecmwfIFSDocumentationCY49R1202411]


# Wave Forecasting and Sea-state Impacts on Atmosphere and ocean

## Two-dimensional wave spectrum
In previous section, we discussed about the **wavenumber spectrum** $F(\mathbf{k})$ of the wave energy, which gives the distribution of wave energy over wavenumber $\mathbf{k}$ (see details in chapter “Action density spectrum” ([[Note_Wave-induced-stress_and_Young-waves#Action density spectrum]])). However, similar to the wave action density, it is much easier to obtain the frequency spectrum because this just requires the analysis of time series at a certain location. 

Thus, we can apply the two dimensional frequency spectrum at each grid cell, defined as:

$$
F(\omega,\theta)\;d\omega d\theta=F(\mathbf{k})\;d\mathbf{k}=F(k,\theta)k\;dkd\theta \tag{34}
$$

hence

$$
F(\omega,\theta)=\frac{k}{c_g}F(k,\theta) \tag{35}
$$

where $c_g=\frac{\partial \omega}{\partial k}$ is the group velocity. ==**The frequency spectrum is thus giving the energy distribution of the ocean waves over angular frequency $\omega$ and propagation direction $\theta$.==** Regarding the directional distribution of waves conventional buoys provide only limited information. It is more common to observe the one-dimensional spectrum defined as:

$$
F(\omega)=\int F(\omega, \theta)\; d\theta \tag{36}
$$

==**The frequency spectrum is obtained by means of a straightforward *Fourier transformation* of the time series for the surface elevation**== $\eta$ . As far as notation is concerned we will use the same symbol for the various forms of the  spectrum, namely F ; the distinction should be clear from their arguments, $F(\mathbf{k}), F(\omega,\theta), F(\omega)$.


## Recall the Action Density Balance Equation
we recall **action density balance equation (Eq. (18))**, but this time we utilising the frequency spectrum via the relation: 

$$
F(\omega,\theta)=\sigma N(\omega, \theta) \tag{37}
$$
Where the $\sigma$ is the intrinsic frequency (see also Eq. (1)). This relation is in accordance with the analogy between wave packets and particles, since particles with action $N$ have energy $\sigma N$ and momentum $kN$. Noted that this relation is different from that of wavenumber spectrum, specified in **Eq.(1)** in [[Note_Wave-induced-stress_and_Young-waves#Action density spectrum]].

For deep water, and with additional source terms, the balance equation can become;

$$ 
\frac{d F}{dt}=\frac{\partial}{\partial t}F+\frac{\partial}{\partial \mathbf{x}}\cdot(c_gF) =\hat S_{\text{in}} + \hat S_{\text{ds}} + \hat S_{\text{nl}} + \hat S_{\text{bot}} \tag{38}
$$

In the case of spherical coordinates, the operator $d/dt$ is given by Eq. (7).

Previously, we discuss each source term in the framework of wave action density $N(\omega, \theta)$). Since now we utilise the frequency spectrum $F(\omega, \theta)$, the representations of source term should change accordingly: for example: $\hat S_{\text{in}}=\sigma S_{\text{in}}$.
- $\hat S_{\text{in}}$: describes the **generation of ocean waves by wind** and therefore represents the momentum and energy transfer from air to ocean waves.
- $\hat S_{\text{ds}}$: describes the **dissipation of waves** by processes such as white-capping, large scale breaking eddy-induced damping. Also represents the injecting of momentum flux from ocean waves into the ocean
- **$\hat S_{\text{nl}}$**: denotes **nonlinear transfer by resonant four-wave interactions.** <span style="background:#fff88f">The nonlinear transfer conserves total energy and momentum</span> and is important in shaping the wave spectrum and in the spectrum down-shift towards lower frequencies (i.e., wave-wave interaction, redistribute energy)
- $\hat S_{\text{bot}}$: **bottom dissipation** due to bottom friction
Find the parameterisations of source terms above in [[Note_Wave-induced-stress_and_Young-waves#Parameterisation of Source terms]]

Noted that Eq. (38) is the one considering the influence of currents and bottom drag to the ocean waves. <span style="background:#fff88f">When consider the surface stresses, the bottom dissipation source term is neglected</span>.


## Momentum/Energy Flux at Air-sea Interface
At air-sea interface, the bottom dissipation due to bottom friction is neglected.
The total wave momentum $M$ depends on the variance spectrum $F(\omega, \theta)$ and is defined as;

$$
\mathbf{M}=\rho_wg\int_{0}^{2\pi} \int_{0}^{\infty} \frac{\mathbf{k}}{\omega}F(\omega, \theta)\;d\omega d\theta \tag{39}
$$

where $\rho_w$ is the water density and $g$ the acceleration due to gravity. The first term inside the integral is sometimes expressed as phase speed of wave: $\frac{\mathbf k}{\omega}=\mathbf{c_p}$. Notice here the phase speed should consider different wavenumber.

The momentum fluxes to and from the wave field are given by the rate of change in time of wave momentum, and **one may distinguish different momentum fluxes depending on the different physical processes**:

### Wind-induced stress & Energy Flux From Wind to Waves
The wind-induces stress, which is also called “wind-stress” or “wind-to-wave stress”. It represents the momentum flux from the wind ($S_{\text{in}}$) that are used for the generation of ocean waves. 
Given by:

$$ \begin{equation} \mathbf{\tau_{\text{in}}} = \rho_w g \int_{0}^{2\pi} \int_{0}^{\infty} \frac{\mathbf{k}}{\omega}\hat S_{\text{in}}(\omega, \theta) \,\; d\omega \, d\theta \end{equation} \tag{40} $$

Similarly, the energy flux from wind to waves is defined by:

$$
 \Phi_{\text{in}} = \rho_w g \int_{0}^{2\pi} \int_{0}^{\infty}\hat S_{\text{in}}(\omega, \theta) \,\; d\omega \, d\theta \tag{41} 
$$

### Dissipation stress & Energy Flux From Waves to Ocean
The Dissipation stress (at the surface) describes the dissipation of waves by processes at the air-sea interface. It shares the same structure as the wind-induced stress:

$$
 \mathbf{\tau_{\text{ds}}} = \rho_w g \int_{0}^{2\pi} \int_{0}^{\infty} \frac{\mathbf{k}}{\omega}\hat S_{\text{ds}}(\omega, \theta) \,\; d\omega \, d\theta \tag{42} 
$$

Similarly, the energy flux from waves to ocean is defined by:

$$
 \Phi_{\text{ds}} = \rho_w g \int_{0}^{2\pi} \int_{0}^{\infty}\hat S_{\text{ds}}(\omega, \theta) \,\; d\omega \, d\theta \tag{43} 
$$


### Separation: Low-frequency and High-frequency
It is important to note that <span style="background:#fff88f">while both the wind stress direction and momentum fluxes are mainly determined by the high-frequency part of the wave spectrum, the energy flux is to some extent also determined by the low-frequency waves </span>(ECMWF, 2024)

The ~={red}**prognostic frequency range**=~ is limited by practical considerations such as restrictions on computation time, but also by the consideration that the high-frequency part of the dissipation source function is not well-known. IN the ECWMF wave model the **high-frequency limit** $\omega_c=2\pi f_c$ is set as:

$$
f_c=\text{min} [ f_{\text{max}}, 2.5\langle f \rangle_{\text{windsea}}] \tag{44}
$$

Thus, the high-frequency extent of the prognostic region is scaled by the mean frequency $\langle f \rangle_{\text{windsea}}$ of the local wind-sea. A **dynamic** high-frequency cut-off, $f_c$, rather than a fixed cut-off at $f_\text{max}$, corresponding to the last discretised frequency, is necessary to avoid excessive disparities in the response time scales within the spectrum. 
This high-frequency cut-off is also discussed in the study of Stokes drift profile by (Breivik et al., 2014). Can find the note in [[Note_Stokes-Drift-Profile-Formula_and_Langmuir-Turbulence_Breivik_2014#High-Frequency Contribution to the Profile]]

In the ~={red}**diagnostic frequency range**=~, $\omega >\omega_c$, the wave spectrum is given by Phillips’s $\omega^{-5}$ power law. For this to be the case, **it is assumed that there is a balance between input, dissipation and the flux due to non linear wave interactions in the diagnostic frequency range**. In practice, this means that all energy and momentum going into the high-frequency rage of the spectrum, either by wind input or non-linear transfer, is dissipated, and is therefore directly transferred to the ocean column:

$$
\int_{0}^{2\pi}\int_{\omega_c}^{\infty} \frac{\mathbf{k}}{\omega}(\hat S_{\text{in}}+\hat S_{\text{ds}}+\hat S_{\text{nl}})\;d\omega d\theta=0 \tag{45}
$$

and for the energy flux:

$$
\int_{0}^{2\pi}\int_{\omega_c}^{\infty} (\hat S_{\text{in}}+\hat S_{\text{ds}}+\hat S_{\text{nl}})\;d\omega d\theta=0 \tag{46}
$$

### Conservation of Momentum/Energy When Integrate Nonlinear Transfer
An important aspect of the nonlinear wave-wave interaction is that when integrate the nonlinear transfer source term $\hat S_{\text{nl}}$ over all frequency and directions, the total momentum, energy and wave-action are conserved. 

This statement comes from fundamental properties of resonant nonlinear wave-wave interactions in the theory of surface gravity waves. (Hasselmann, 1962) explicitly shows that four-wave resonant interactions conserve total energy, total momentum and total wave action. The information can also be found in (Dynamics and Modelling of Ocean Waves, n.d.) by Komen et al. 1994.

The formula is given by:

$$
\int_{0}^{2\pi} \int_{0}^{\infty} \frac{\mathbf k}{\omega}\hat S_{\text{nl}}\;d\omega d\theta=0 \tag{47}
$$

The above relation can be separated into high- and low-frequency range:

$$
\int_{0}^{2\pi} \int_{0}^{\omega_c} \frac{\mathbf k}{\omega}\hat S_{\text{nl}}\;d\omega d\theta + \int_{0}^{2\pi} \int_{\omega_c}^{\infty} \frac{\mathbf k}{\omega}\hat S_{\text{nl}}\;d\omega d\theta =0 \tag{48}
$$

### Momentum/Energy flux to the Ocean Column
The momentum flux to the ocean column, denoted by $\tau_{\text{oc}}$, is the sum of the flux transferred by turbulence across the air-sea interface which was not used to generate waves ($\tau_{\text{a}} - \tau_{\text{in}}$) and the momentum flux transferred by the ocean waves due to wave breaking $\tau_{\text{ds}}$.

As a consequence:

$$
\tau_{\text{oc}}=\tau_{\text{a}} - \tau_{\text{in}}-\tau_{\text{ds}} \tag{49}
$$

Utilising:
- The assumed balance at the high frequencies (**Eq. (45)**)
- the conservation of momentum for $\hat S_{\text{nl}}$ when integrated over all frequencies and directions (**Eq. (48)**)
one can finds:

$$
\begin{align}
\tau_{\text{oc}}
&=\tau_{\text{a}} - \tau_{\text{in}}-\tau_{\text{ds}} \\
&=\tau_{\text{a}} - (\tau_{\text{in}}+\tau_{\text{ds}}) \\
&=\tau_{\text{a}} - \rho_wg\bigg(\int_{0}^{2\pi}\int_{0}^{\infty} \frac{\mathbf{k}}{\omega}(\hat S_{\text{in}} +\hat S_{\text{ds}})\,\;d\omega \, d\theta  \bigg) \\
&=\tau_{\text{a}} - \rho_wg \bigg(\int_{0}^{2\pi}\int_{0}^{\omega_c} \frac{\mathbf{k}}{\omega}(\hat S_{\text{in}}+\hat S_{\text{ds}})\,\;d\omega \, d\theta + \int_{0}^{2\pi}\int_{\omega_c}^{\infty} \frac{\mathbf{k}}{\omega}(\hat S_{\text{in}}+\hat S_{\text{ds}})\,\;d\omega \, d\theta \bigg) \\
&=\tau_{\text{a}} - \rho_wg \bigg(\int_{0}^{2\pi}\int_{0}^{\omega_c} \frac{\mathbf{k}}{\omega}(\hat S_{\text{in}}+\hat S_{\text{ds}})\,\;d\omega \, d\theta - \int_{0}^{2\pi}\int_{\omega_c}^{\infty} \frac{\mathbf{k}}{\omega}\hat S_{\text{nl}}\,\;d\omega \, d\theta \bigg) \\
&=\tau_{\text{a}} - \rho_wg \bigg(\int_{0}^{2\pi}\int_{0}^{\omega_c} \frac{\mathbf{k}}{\omega}(\hat S_{\text{in}}+\hat S_{\text{ds}})\,\;d\omega \, d\theta + \int_{0}^{2\pi}\int_{0}^{\omega_c} \frac{\mathbf{k}}{\omega}\hat S_{\text{nl}}\,\;d\omega \, d\theta \bigg) \\
&=\tau_{\text{a}} - \rho_wg \int_{0}^{2\pi}\int_{0}^{\omega_c} \frac{\mathbf{k}}{\omega}(\hat S_{\text{in}}+\hat S_{\text{ds}} + \hat S_{\text{nl}})\,\;d\omega \, d\theta \tag {50}
\end{align}
$$

Where the $\tau_{\text{a}}$ is the atmospheric stress, whose magnitude is given by $\tau_{\text{a}}=\rho_{\text{a}}u_*|u_*|$, with $u_*$ the air-side friction velocity.
Notice that the dissipation source term $\hat S_{\text{ds}}$ is negative. This dissipated momentum/energy is transferred from surface waves into the ocean column.

Eq. (50) can then be summarised as:

$$
\tau_{\text{oc}}=\tau_{\text{a}}-\tau_{\text{transient}} \tag{51}
$$

The term $\tau_{\text{transient}}$, separates itself from the atmospheric stress $\tau_{\text{a}}$, can be considered as the transient impacts from ongoing (i.e., transient) surface wave processes. 
However, careful interpretation is needed since the atmospheric stress term $\tau_{\text{a}}=\rho_{\text{a}}u_*|u_*|$ is also influenced by the surface wind, as the air-side friction velocity is dependent on wind stress (see details in previous chapter: [Wind Input]([[Note_Wave-induced-stress_and_Young-waves#Wind Input (“Wind stress”, “atmospheric stress”)]])

While for the energy flux, we ignore the direct energy flux from air to ocean currents, because it is small, the energy flux to the ocean, denoted by $\Phi_{\text{oc}}$, is therefore given by $-\Phi_{\text{ds}}$. Again, utilising the assumed high frequency balance (Eq. (45)) and the conservation of energy when $S_{\text{nl}}$ is integrated over all frequencies and directions, one obtain:

$$
\begin{align}
\Phi_{\text{oc}}
&=-\Phi_{\text{ds}} \\
&=-\rho_w g \int_{0}^{2\pi} \int_{0}^{\infty}\hat S_{\text{ds}} \,\; d\omega \, d\theta \\
&=-\rho_w g \bigg(\int_{0}^{2\pi} \int_{0}^{\omega_c}\hat S_{\text{ds}} \,\; d\omega \, d\theta +\int_{0}^{2\pi} \int_{\omega_c}^{\infty}\hat S_{\text{ds}} \,\; d\omega \, d\theta \bigg) \\
&= -\rho_w g \bigg(\int_{0}^{2\pi} \int_{0}^{\omega_c}\hat S_{\text{ds}} \,\; d\omega \, d\theta -\int_{0}^{2\pi} \int_{\omega_c}^{\infty}(\hat S_{\text{in}}+\hat S_{\text{nl}}) \,\; d\omega \, d\theta \bigg) \\
&= -\rho_w g \bigg(\int_{0}^{2\pi} \int_{0}^{\omega_c}\hat S_{\text{ds}} \,\; d\omega \, d\theta -\int_{0}^{2\pi} \int_{\omega_c}^{\infty}\hat S_{\text{in}} \,\; d\omega \, d\theta - \int_{0}^{2\pi} \int_{\omega_c}^{\infty}\hat S_{\text{nl}} \,\; d\omega \, d\theta \bigg) \\
&= -\rho_w g \int_{0}^{2\pi} \int_{0}^{\omega_c}(\hat S_{\text{ds}}+\hat S_{\text{nl}}) \,\; d\omega \, d\theta +\int_{0}^{2\pi} \int_{\omega_c}^{\infty}\hat S_{\text{in}} \,\; d\omega \, d\theta  \\
&= -\rho_w g \int_{0}^{2\pi} \int_{0}^{\omega_c}(\hat S_{\text{ds}}+\hat S_{\text{nl}}) \,\; d\omega \, d\theta - \bigg(\int_{0}^{2\pi} \int_{0}^{\omega_c}\hat S_{\text{in}} \,\; d\omega \, d\theta \bigg)\\ & \quad\quad\quad\quad\quad\quad\quad\quad\quad\:\quad\quad\quad\quad\quad\quad+\int_{0}^{2\pi} \int_{\omega_c}^{\infty}\hat S_{\text{in}} \,\; d\omega \, d\theta + \bigg(\int_{0}^{2\pi} \int_{0}^{\omega_c}\hat S_{\text{in}} \,\; d\omega \, d\theta \bigg) \\
&=\rho_w g\int_{0}^{2\pi} \int_{0}^{\infty}\hat S_{\text{in}} \,\; d\omega \, d\theta -\rho_w g \int_{0}^{2\pi} \int_{0}^{\omega_c}(\hat S_{\text{in}}+\hat S_{\text{ds}}+\hat S_{\text{nl}}) \,\; d\omega \, d\theta \\ 
&=\Phi_{\text{in}} -\rho_w g \int_{0}^{2\pi} \int_{0}^{\omega_c}(\hat S_{\text{in}}+\hat S_{\text{ds}}+\hat S_{\text{nl}}) \,\; d\omega \, d\theta \tag{52}
\end{align}
$$

Similar to Eq. (51), we can also rewrite the Eq. (52) to:

$$
\Phi_{\text{oc}}=\Phi_{\text{in}}-\Phi_{\text{transient}} \tag{53}
$$

The term $\Phi_{\text{transient}}$, separates itself from the energy flux from wind to wave $\Phi_{\text{in}}$, can be considered as the impacts from transient surface wave processes.

Furthermore, the energy flux from wind to wave can be written to separate high- and low-frequency components:

$$
\begin{align}
\Phi_{\text{in}}&=\rho_w g\int_{0}^{2\pi} \int_{0}^{\infty}\hat S_{\text{in}} \,\; d\omega \, d\theta \\
&=\rho_w g\int_{0}^{2\pi} \int_{0}^{\omega_c}\hat S_{\text{ds}} \,\; d\omega \, d\theta + \rho_w g\int_{0}^{2\pi} \int_{\omega_c}^{\infty}\hat S_{\text{in}} \,\; d\omega \, d\theta \\
&= {\Phi_{\text{in}}}_{\text{lf}}+{\Phi_{\text{in}}}_{\text{hf}} \tag{54}
\end{align}
$$

The high-frequency ($\omega>\omega_c$) contribution to the energy flux is parameterised following the same approach as for the kinematic wave induced stress (Eq. (5.15) in (ECMWF, 2024)):

$$
{\Phi_{\text{in}}}_{\text{hf}}=\rho_a\frac{(2\pi)^4f_c^5}{g}u_*^2\int_{0}^{2\pi}F(f_c,\theta)[\text{max}(\cos{(\theta-\phi),0})]^2 \frac{\beta_m}{\kappa^2}\int_{\omega_c}^{\infty}\frac{d\omega}{\omega^2}\mu_{\text{hf}}\ln^4{(\mu_{\text{hf}})} \, \; d\theta \tag{55}
$$

In Eq. (55), the integral over directions can be evaluated using the prognostic part of the spectrum, whereas the second integral is only function of $u_*$ and the Charnock parameter. It can therefore be tabulated beforehand. Note that the integration is bounded because $\mu_{\text{hf}}\le 1$. The high-frequency component of energy flux goes into the ocean column is:

$$
{\Phi_{\text{oc}}}_{\text{hf}}=\frac{\beta_m}{\kappa^2}\sqrt{\frac{z_0}{g}}\int_{\gamma_c}^{\infty}\frac{d\gamma}{\gamma^2}\mu_{\text{hf}}\ln^4{(\mu_{\text{hf}})} \, \; d\theta, \quad \quad \gamma_c=\text{max}\big(\omega_c, x_0\frac{g}{u_*} \big)\sqrt{\frac{z_0}{g}}  \tag{56}
$$

where for typical values of the Charnock parameter, $x_0\sim0.05$. Since CY45R1 (June 2018), it was found that it was as numerically efficient to compute the integral in Eq. (56) following the variable transformation $X = \ln(\gamma)$ and only a few discretised points using the Simpson integration method.

The archived energy fluxes were originally normalised by the product of the air density and the cube of the friction velocity in the air ($u_∗$). Hence the normalised energy flux into waves (parameter  140211) is obtained from Eq. (54) divided by $\rho_a u_∗^3$. However, we now also produce the actual energy flux (parameter 140105). Similarly, the normalised energy flux into ocean (parameter 140212) is obtained from normalising Eq. (52) and we now also produce the U- and V- components of the ocean side stress (parameters 140103 and 140104). The normalised stress into ocean (parameter 140214) is derived from (10.35) by dividing it with the atmospheric stress $\tau_a=\rho_a u_*^2$.


---
# Concluding Remark 
## ICON-Waves
### Summarised Formula set
Formulas are referenced from [ICON-Short-Overview]([[ICON-waves_Short-Overview_and_Current-Status#Wave-ocean coupling]])
- Surface **density-normalised wind stress** ($\bar \rho_a = \tau_a / \rho_a$):
  $$
	\bar \tau_a = u_*^2 =\bigg(\frac{\kappa\mathbf{U}(z_{\text{obs}})}{\ln(\frac{z_{\text{obs}}+z_o}{{z_o}})}\bigg)^2 \tag{57}
	$$
- **Wave stress**:
  $$
	  \begin{align}
	  \tau_w=\epsilon^{-1}g\int \gamma N \mathbf{k}\;d\omega d\theta 
	  &=\epsilon^{-1}g\int_{0}^{2\pi}\int_{0}^{\omega_c} \frac{\mathbf{k}}{\omega}S_{\text{in}}\;d\omega d\theta \tag{58}\\
	  &=\frac{1}{\rho_a}(\tau_{\text{in}})
	  \end{align}
	$$
	where $\omega_c$ is the high frequency cutoff in numerical scheme. Will be discussed in later section.
	
- Wind input source function:
  $$
	  S_{\text{in}}=\gamma N= \omega\epsilon \beta x^2 \tag{59}
	$$
	$x$ is the parameter that associates with the reciprocal of wave age $x\sim \frac{u_*}{c_p}$. See details in [Eq. (28)]([[Note_Wave-induced-stress_and_Young-waves#Wind Input (“Wind stress”, “atmospheric stress”)]])
	
- **Sea-state-dependent Charnock number:**
  $$
	\alpha = \frac{\hat \alpha}{\sqrt{1-\frac{\tau_w}{\bar\tau_a}}} \tag{60}
	$$
- **Background roughness length**:
  $$
	z_b=\frac{\alpha \bar\tau_{\alpha}}{g} \tag{61}
	$$
- **Sea surface roughness length**:
  $$
	z_o=\frac{z_b}{\sqrt{1-\frac{\tau_w}{\bar\tau_a}}}=\frac{\alpha u_*^2}{g\sqrt{1-\frac{\tau_w}{u_*^2}}} \tag{62}
	$$
	


### Important Comment
>[!Important] **“Wave stress”, “Wave-induced stress” and “Wind-to-wave stress”** 
> - The “wave stress” is the “wind-to-wave stress” $\tau_{\text{in}}$ divided by air density (as $\epsilon^{-1}=\rho_w/\rho_a$) considered in the later sections using the source input term $S_{\text{in}}$. The wave stress in Eq. (25) is specifically used in determination of air-side friction velocity
> - They all relate the momentum flux from the atmosphere (i.e., wind) to the surface waves, which is the momentum flux used in wave generation.
> - All three terms represent the exact same thing, but slightly modified to meet special requirements.

> [!Important] **Wave-atmosphere coupling: An angle from Momentum Flux**
> Thus, the roughness length is dependent on the “wave stress” and the “wind stress”, and this length is used in the determination of the friction velocity. All of these thus indicate a way <span style="background:#fff88f">how the wave-field will influence the atmospheric side.</span>




---
# Young Ocean and Young Surface Wave

The terms refer to the development stage of the wave field relative to the local wind.
## Wave-age

Define the wave age (dimensionless)

$$ \begin{equation} \text{Wave age} = \frac{c_p}{U_{\text{10}}} \end{equation} $$

where:
- $c_p$: phase speed of the dominant (spectral peak) waves
- $U_{\text{10}}$: wind speed at 10 m height (input from atmospheric model)

## Interpretation

| **Wave age** | **Description**               | **Physical Meaning**                                                                                                                                                                                                                        |
| ------------ | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| < 1          | Young waves                   | - Wind is strong, waves are short and steep, still growing, not yet in equilibrium with wind <br>- The sea surface is rough, wave-induced stress is large<br>- **The wave-induced stress can be a large fraction of the total wind stress** |
| ~ 1          | Developing/transitional waves | Growing but approaching equilibrium                                                                                                                                                                                                         |
| > 1          | Old Waves (i.e., Swell)       | Waves outrun the local wind, swell propagating away from the source region                                                                                                                                                                  |

- _**“For young surface waves, almost all of momentum flux from atmosphere are absorbed by surface waves. Only after the waves are fully developed, the residual momentum flux will be transferred to ocean.”**_ (Zhao et al., 2022, p. 6)

