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
For details in this chapter, please referring to the Chapter 3 in IFS Wave model documentation (ECMWF, 2024) [@ecmwfIFSDocumentationCY49R1202411]

## Wind Input (Wind stress)
### Before CY46R1, June 2019 (Also the ICON-Wave version)
The **total air–sea momentum flux (wind stress → flux of horizontal momentum (per unit area) transferred from the wind to the air-sea interface.**) is written as:

$$ 
\begin{align} 
\tau_{\text{a}} &= \rho_a \mathbf{u_*} |\mathbf{u_*}| \tag{22}
\end{align}
$$

The formula is according to the ECWMF paper: (ECMWF, 2024); part of the understanding comes from the [ICON-Wave short overview]([[ICON-waves_Short-Overview_and_Current-Status]])

where:
- $\tau_a$: wind stress, is considered as the total momentum flux from the wind that applies to the air-sea interface
- $\rho_a$: density of the air in the atmospheric boundary layer
- $u_*$: air-side friction velocity

The Eq. (22) is the formula for the wind stress, and it is closely related to the friction velocity. This stress of air flow on sea waves depends on the sea state and from a consideration of the momentum balance of air:

$$
u_*^2=\bar\tau_a =\bigg(\frac{\kappa\mathbf{U}(z_{\text{obs}})}{\ln(\frac{z_{\text{obs}}+z_o}{{z_o}})}\bigg)^2 \tag{23}
$$

where the $\bar \tau_a$ is the density-normalised wind stress: $\tau_a = \rho \bar \tau_a$. The roughness length $z_o$ is represented as:

$$
z_o=z_b/\sqrt{1-\frac{\tau_w}{\bar\tau_a}} \tag{24}
$$


Here $z_{\text{obs}}$ is the mean height above the waves (currently 10 m), and $\tau_w$ is the stress induced by gravity wave, which is so-called “wave stress” and “wave-induced stress”):

$$
\tau_w=\epsilon^{-1}g\int \gamma N \mathbf{k}\;d\omega d\theta \tag{25}
$$

Where 
$$\gamma N=S_{\text{in}} \tag{26}$$

> [!Important] **“Wind stress” = “Wind-induced stress” = “Wind-to-wave stress”** 
> - The “wind stress”, despite has slightly different formula, it is the same as the “wind-to-wave stress” considered in the later sections using the source input term $S_{\text{in}}$. 
> - They all represents the momentum flux from the atmosphere (i.e., wind) to the surface waves, which is the momentum flux used in wave generation


$\gamma$ is the growth rate, which has the relationship: 

$$
\frac{\gamma}{\omega}=\epsilon \beta x^2 \tag{27}
$$
where $\omega$ is the angular frequency, $\epsilon$ is the air-water density ratio and $\beta$ the *Mile’s parameter*. $x$ is a parameter linked to the wave age ($u_* / c_p$): 

$$
x=(\frac{u_*}{c_p})\text{max}(\cos{(\theta-\phi),0}) \tag{28}
$$
Finally, $z_b$ is the background roughness representing the impact of gravity-capillary short waves. Since CY49R1, the wind stress Eq. (25) was evaluated for the high frequency with a $f^{-5}$ tail of for gravity waves range until the gravity-capillary range where a simplified model for the gravity-capillary spectrum is used instead. Ultimately resulting in an estimate for $z_b$.
In practice, ~={red}**wave stress points in the wind direction as it is mainly determined by the high-frequency waves which respond quickly to changes in the wind direction**=~ (ECMWF, 2024).

The background roughness length (Eq. (24)) can link to *Charnock* relation:

$$
z_b=\frac{\alpha \bar\tau_{\alpha}}{g} \tag{29}
$$

The dimensionless ***Charnock parameter*** $\alpha$ is not constant but depends on the sea state through the wave-induced stress before CY49R1:

$$
\alpha = \frac{\hat \alpha}{\tau}/{\sqrt{1-\frac{\tau_w}{\tau_a}}} \tag{30}
$$

where the $gz_b$ is the direct calculation of $z_b$. When combined with the renormalised growth rate, hese changes yields a reduction of the resulting *Charnock* parameter for storm wind conditions (above 20 m/s), which is correct to match the observational evidence that the *Charnock* parameter should reduce quite considerably under strong tropical winds.

#### Summary for ICON-Waves
- Surface **density-normalised wind stress** ($\bar \rho_a = \tau_a / \rho_a$):
  $$
	\bar \tau_a = u_*^2 =\bigg(\frac{\kappa\mathbf{U}(z_{\text{obs}})}{\ln(\frac{z_{\text{obs}}+z_o}{{z_o}})}\bigg)^2
	$$
- **Wave stress**:
  $$
	  \begin{align}
	  \tau_w&=\epsilon^{-1}g\int \gamma N \mathbf{k}\;d\omega d\theta \\
	  &=\epsilon^{-1}g\int_{0}^{2\pi}\int_{0}^{\omega_c} \frac{\mathbf{k}}{\omega}S_{\text{in}}\;d\omega d\theta
	  \end{align}
	$$
	
	where $\omega_c$ is the high frequency cutoff in numerical scheme. Will be discussed in later section.
- **Sea-state-dependent Charnock number:**
  $$
	\alpha = \frac{\hat \alpha}{\sqrt{1-\frac{\tau_w}{\bar\tau_a}}}
	$$
- **Background roughness length**:
  $$
	z_b=\frac{\alpha \bar\tau_{\alpha}}{g} 
	$$
- **Sea surface roughness length**:
  $$
	z_o=\frac{z_b}{\sqrt{1-\frac{\tau_w}{\bar\tau_a}}}=\frac{\alpha u_*^2}{g\sqrt{1-\frac{\tau_w}{u_*^2}}}
	$$


The formula is also summarised in [[ICON-waves_Short-Overview_and_Current-Status#Wave-ocean coupling]]

> [!Important] **Wave-atmosphere coupling: An angle from Momentum Flux**
> Thus, the roughness length is dependent on the “wave stress” and the “wind stress”, and this length is used in the determination of the friction velocity. All of these thus indicate a way <span style="background:#fff88f">how the wave-field will influence the atmospheric side.</span>

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
 
 More details will be summarised in later section.

## Dissipation due to wave breaking
**White capping dissipation** (often denoted $S_{ds}$​) is the loss of wave energy due to **wave breaking** in deep water, i.e. the formation of whitecaps on the ocean surface. Hasselmann (1973) summarised that: “It is generally believed that **white capping is the dominant dissipative mechanism in a wave field at moderate and higher wind speeds**, simply because other dissipative process such as molecular viscosity or turbulence appear to be inadequate to remove the energy which is know to be imparted to the waves by the wind”

Whitecapping occurs when:
- Wind-driven waves become too steep
- The crest becomes unstable
- The wave breaks in deep water, producing foam (“white caps”)
It is the wave breaking caused by wave steepness and wind–wave interaction.

Janssen et al. (1989) realised that the wave dissipation source function has to be adjusted in order to obtain a proper balance at the high frequencies. The dissipation source term of Hasselmann (1974) is thus extended as:

$$
S_{\text{ds}}=-C_{\text{ds}}\langle\omega\rangle(\langle k\rangle^2m_0)^2 \biggr[ \frac{(1-\delta)k}{\langle k \rangle}+\delta (\frac{k}{\langle k \rangle})^2 \biggr]N \tag{31}
$$

where $C_{\text{ds}}$ and $\delta$ are constants, $m_0$ is the total wave variance per square metre, $k$ the wavenumber, and $\langle\omega\rangle$ and $\langle k \rangle$ are the mean angular frequency and mean wavenumber, respectively. 

## Bottom dissipation




## Nonlinear Transfer
so called “Four-wave-interactions”



# Wave Forecasting and atmosphere-wave-ocean Interaction
## The Source Term in the Wave Energy Balance

### Action Balance Equation

Waves may grow because of the action of wind and they may loose energy because of dissipation due  to e.g. white capping, wave-breaking. Furthermore, finite amplitude ocean waves are subject to nonlinear four-wave interactions. As long as these perturbations are small they can be added and in the context of a statistical description of ocean waves.

When considering external sources, the **action density balance equation** is:

$$ \frac{d N}{dt}= S_{\text{in}} + S_{\text{ds}} + S_{\text{nl}} \tag{8}\ $$

In the case of spherical coordinates, the operator $d/dt$ is given by Eq. (7).
While the source terms:
- $S_{\text{in}}$: **wave-source from wind** (energy transferred from wind to waves)
- $S_{\text{ds}}$: **wave-dissipation source** (e.g., wave breaking), injecting momentum flux into the ocean
- **$S_{\text{nl}}$**: **nonlinear four wave interactions** (e.g., wave-wave interaction, redistribute energy)

Thus, the source terms are with the dimension as $S\sim \frac{d N}{d t}$, which stands for the *rate of change of action density*.

 

> [!Attention] **~={red}The General Form of momentum flux associated with wave:=~**
> The **rate of change of momentum** in the wave field equals the **integral of the source term divided by phase speed**, projected along the propagation direction:
> 
> $$ \begin{equation} \vec\tau = \int_{0}^{2\pi} \int_{0}^{\infty} \frac{\vec k}{\omega} S(\omega, \theta) \, d\omega \, d\theta \tag{5} \end{equation} $$
> 
> where:
> 
> - $\rho_w$: the seawater density
> - $\omega$: angular frequency
> - $\theta$: direction
> - $\vec k$: wavenumber
> 
> Hence, the integration of the source term $S_{\text{in} }$ represents **how the wind input at all frequencies and directions contributes to the total momentum flux into the wave field**.
> 
 

The Eq. (3) can be expressed equivalently as (scalar form along the wind direction):

$$ \begin{equation} \vec\tau_{\text{in}} = \rho_w g \int_{0}^{2\pi} \int_{0}^{\infty} \frac{S_{\text{in}}(\omega, \theta)}{c_p} \, d\omega \, d\theta \tag{4} \end{equation} $$

where the $c_p=\frac{\omega}{k}$ is the wave phase speed.
Same integrations of $S_{\text{dis}}$ can give other contributions to the momentum flux

Hence, the units for the total momentum flux (i.e., if in air-sea interface, e.g., wind → wave stress) is:
$$
\frac{\vec k}{\omega}E(\omega, \theta) \quad\quad \text{units}=\frac{(m^{-1})}{(s^{-1})}(J\;m^{-2})
$$

> [!Tip]
> **However**, the pre-factor (e.g., $\rho_w g$) depends on the spectral units of $S$!
> There are two common spectral conventions:
> - Surface elevation variance spectrum $S_{\eta}(\omega,\theta)$ (units $m^2\cdot s$)
> 	- energy per unit surface is $\frac{1}{2}\rho_w g S_{\eta}$
> 	- Then use $\tau \sim \frac{1}{2}\rho_w g (\text{Integral})$
> 	  
> - Energy density $E(\omega,\theta)$ (units $J\cdot m^{-2} \text{per Hz per rad}$)
> 	-  $E$ already contains the $\frac{1}{2}\ g$ factor
> 	- The use $\tau \sim \rho_w(\text{Integral})$



> [!Attention]
> Hence, one should rewrite the momentum flux (Eq (1)) in the air-sea interface (Zhao et al., 2022) :
> 
> $$ \begin{align} \vec \tau_a &= \vec \tau_{\text{oc}} + \vec \tau_{\text{in}} + \vec \tau_{\text{ds}} \\ &= (\vec \tau_{\text{oc}}+ \vec \tau_{\text{ds}})+\vec \tau_{\text{in}} \\ &=\vec \tau_{\text{ocean}} + \vec \tau_{\text{wave}}    \tag{5}\end{align} $$
> 
> where:
> 
> - $\vec \tau_{a}$: wind stress, can be parameterised by $\vec \tau_{a}=\rho_a u_*^{2}$ (Monin-Obukhov Theory; $u_*$ is the friction velocity)
> - $\vec \tau_{\text{oc}}$: momentum flux obtained by ocean from wind source
> - $\vec \tau_{\text{in}}$: momentum flux from wind source to ocean
> - $\vec \tau_{\text{ds}}$: momentum flux injected from breaking waves to the ocean
>   
> **Related to Eq. (1): $\vec \tau_{\text{wave}}= \vec \tau_{\text{in}}$, as:** 
> - **the $\vec \tau_{\text{ds}}$ should contributes to $\tau_{oc}$**, since dissipation transfer momentum from the waves to the ocean. Not from wind → wave anymore
> - the $\tau_{nl}=0$, since nonlinear interaction ONLY redistribute energy/momentum within the spectrum but conserve total momentum.
> - Then, **the $\tau_{\text{wave}}$ is the net stress (air-sea momentum flux) going into the wave**, which is equals to the wind input $\tau_{in}$


## Physical interpretation

- The **wind input source term** $S_{\text{in}}$ quantifies how much _energy_ the wind gives to each spectral component.
- Dividing by the **phase speed** $c_p$ gives the _momentum flux_ associated with that energy input.
- Integrating over all frequencies and directions gives the **total stress** imparted to the wave field (the “form drag”).

Hence, the **wave-induced stress** represents the portion of the atmospheric stress that is **carried by the growing waves** rather than **immediately transferred to ocean currents**.


## Summary

“Detailed description of surface waves modulated air-sea momentum flux could be found in Janssen (1989, 1991) and Breivik et al. (2016)” (Zhao et al., 2022)





# Young Ocean and Young Surface Wave

The terms refer to the development stage of the wave field relative to the local wind.
## (a) Wave-age

Define the wave age (dimensionless)

$$ \begin{equation} \text{Wave age} = \frac{c_p}{U_{\text{10}}} \tag{6}\end{equation} $$

where:
- $c_p$: phase speed of the dominant (spectral peak) waves
- $U_{\text{10}}$: wind speed at 10 m height (input from atmospheric model)

## (b) Interpretation

| **Wave age** | **Description**               | **Physical Meaning**                                                                                                                                                                                                                        |
| ------------ | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| < 1          | Young waves                   | - Wind is strong, waves are short and steep, still growing, not yet in equilibrium with wind <br>- The sea surface is rough, wave-induced stress is large<br>- **The wave-induced stress can be a large fraction of the total wind stress** |
| ~ 1          | Developing/transitional waves | Growing but approaching equilibrium                                                                                                                                                                                                         |
| > 1          | Old Waves (i.e., Swell)       | Waves outrun the local wind, swell propagating away from the source region                                                                                                                                                                  |

- _**“For young surface waves, almost all of momentum flux from atmosphere are absorbed by surface waves. Only after the waves are fully developed, the residual momentum flux will be transferred to ocean.”**_ (Zhao et al., 2022, p. 6)

