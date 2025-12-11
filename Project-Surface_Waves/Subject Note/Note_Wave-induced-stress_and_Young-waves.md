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
	- (Janssen, 2004) [@janssenInteractionOceanWaves2004]
	- (ECMWF, 2024) [@ecmwfIFSDocumentationCY49R1202411]
- For the usage of WAM model, please find:
	- (Wu et al., 2022) [@wuRedistributionAirSea2022]
	- (Wu et al., 2019) [@wuWaveEffectsCoastal2019]

> [!Important] 
> The below is the note and my comprehension about the numerical details in the wave model from ECMWF (i.e., ECWAM model)

# Wave-induced stress: Integral of wind-input source function
## Wind stress at the air-sea interface 

The **total air–sea momentum flux (wind stress → flux of horizontal momentum (per unit area) transferred from the wind to the air-sea interface.**) is written as:

$$ 
\begin{align} 
\tau_{\text{a}} &= \rho_a \mathbf{u_*} |\mathbf{u_*}| \\
&= \rho_a C_d \mathbf{U_{10}} |\mathbf{U_{10}}| \tag{1}
\end{align}
$$

The formula is according to the ECWMF paper: (ECMWF, 2024); part of the understanding comes from the [ICON-Wave short overview]([[ICON-waves_Short-Overview_and_Current-Status]])
where:
- $\tau_a$: wind stress, is considered as the total momentum flux from the wind that applies to the air-sea interface
- $\rho_a$: density of the air in the atmospheric boundary layer

while in operational model, we consider the **friction velocity**:

$$
u_*=\sqrt{C_d}\mathbf{U_{10}}
$$

This is based on the paper Edson et al. (2013) and corrigendum Edson et al. (2014). (See (ECMWF, 2024) Chapter 3.2.4). where:
- $C_d$: drag coefficient in terms of 10 m wind speed $\mathbf{U_{10}}$. It is a value depends on the 10 m wind speed. In ECWMF WAM model, $C_d=(c_1+c_2 \mathbf{U_{10}}^{p_1})^{p_2}$, where $c_1, c_2, p_1, p_2$ are constants.

### Early comment on “Momentum Flux into the Ocean”
- When considering the wave model, the wind stress (total momentum flux from the wind) at the ocean surface is not only transferred directly into ocean interior — part of it goes into surface gravity waves. The wave is numerically considered as a ‘mediator’ between the atmosphere and ocean (Wu et al., 2019, 2022)
- Hence, the surface stress (momentum flux) felt by the ocean interior is the total surface stress applied by the atmosphere minus the net stress going into the waves,  (Janssen et al., 2013)
- “The momentum flux to the ocean column, denoted by $\tau_{oc}$, is the sum of the flux transferred by turbulence across the air-sea interface which was not used to generate waves ($\tau_a - \tau_{in}$) and the momentum flux transferred by the ocean waves due to wave breaking $\tau_{diss}$.” (ECMWF, 2024, p. 99) 🔤海洋柱的动量通量用τoc表示，是未用于产生波浪的穿过海-气界面的湍流传递的通量τa-τin与海浪因波浪破碎而传递的动量通量τdiss的总和。🔤
 More details will be summarised in later section.


## Basic Equations

Please find the below contents referenced to Janssen, 2004
Janssen, P. (2004). _The Interaction of Ocean Waves and Wind_. Cambridge University Press.
### Action density spectrum *N*
The most elegant formulation of the “energy” balance equation is in terms of he **action density spectrum** $N(\mathbf{k})$ which is the **energy spectrum** divided by the so-called intrinsic frequency $\sigma$. 

Given the formula:

$$
N(\mathbf{k})=\frac{gF(\mathbf{k})}{\sigma},\quad \quad \sigma=\sqrt{gk\tanh{(kh)}} \tag{2}
$$

Where the $F(\mathbf{k})$ is the **wavenumber spectrum**, gives the distribution of wave energy over wavenumber $\mathbf{k}$. Its integration over the wavenumber equals the wave variance, expressed in the square of ocean **surface elevation** $\eta$:

$$
\langle \eta^2\rangle=\int_{-\infty}^{\infty}F(\mathbf{k})\;d\mathbf{k}=m_0
$$

Where the bracket $\langle \rangle$ stands for the ensemble average. $m_0$ is the so-called zero order moment of the spectrum and $m_0\sim \:\text{units:}\:m^2$. The integral over the wave spectrum is a measure for wave height (e.g., *significant wave height* $H_s=4\sqrt{m_0}$) 
The wave variance $\eta^2$ can be linked to the wave energy through:

$$
\langle E \rangle =\rho_w g \langle \eta^2\rangle=\rho_wg\int_{-\infty}^{\infty}F(\mathbf{k})\;d\mathbf{k}
$$

**Thus**, for wave groups with action $N$ have energy $\sigma N=gF$ and momentum $kN$.

### Balance Equation for Action Density Spectrum *N*

Give the *spatial dependency* to this action density spectrum, let:
$$
\mathbf{z}=(x_1,x_2,k_1,k_2)
$$
be the combined four-dimensional vector. Thus, the most fundamental form of the transport equation for the action density spectrum $N(\mathbf{k},\mathbf{x},t)$:

$$
\frac{\partial}{\partial t}N+\frac{\partial }{\partial z_i}(\dot{z_i}N)=0 \tag{3}
$$

where $\dot{z_i}$ denotes the propagation velocity of a wave group in the four-dimensional phase space of $\mathbf{x}$ and $\mathbf{k}$. This equations holds for any field $\dot{z_i}$, and also for velocity fields which are not divergence-free in four-dimensional phase space.

In the special case when $\mathbf{x}$ and $\mathbf{k}$ represent a canonical vector pair (e.g., when they are the usual Cartesian coordinates), the propagation equations for a wave group (also know as the Hamilton-Jacobi propagation equations) read:

$$
\begin{align} 
\dot{x_i} &=\frac{\partial }{\partial k_i}\Omega \\
& \tag{4} \\
\dot{k_i} &=-\frac{\partial }{\partial x_i}\Omega 
\end{align}
$$

where the $\Omega$ denotes the dispersion relation:

$$
\Omega = \mathbf{k}\cdot\mathbf{u} +\sigma
$$

Because the field $\dot{z}$ for a continuous ensemble of wave groups is divergence free in four-dimensional phase space, thus, the transport equation for the action density may be expressed in the advection form:

$$
\frac{d}{dt} = \frac{\partial N}{\partial t}+\dot{z_i}\frac{\partial }{\partial z_i}N=0\tag{5}
$$

since $\frac{\partial}{\partial z_i}\dot{z_i}=0$ as divergence free. Thus, along a path in four-dimensional phase space defined by the Hamilton-Jacobi equations, the action density $N$ is conserved. 
> [!Tip]
> This property holds only for canonical coordinates for which the flow divergence vanishes!!!

However, one should turn to the form of the action density balance equation (Eq. 3) in the <span style="background:#fff88f">flux form since it is more general and has the advantage.</span> When one transforms from one set of coordinates to another there is no guarantee that the flow remains divergence-free and therefore the flux form of the action balance equation is the preferred starting point.

### Balance Equation in Spherical Geometry

> [!Attention] Action Density In Spherical Geometry
> We now consider the action density flux balance equation in the spherical geometry, i.e., consider the ~={red}spectral action density $\hat N (\omega, \theta, \phi,\lambda, t)$=~ with respect to angular frequency $\omega$ and direction $\theta$ (measured clockwise relative to true north) as a function of latitude $\phi$ and longitude $\lambda$.
> 
> 
> - **choice of angular frequency instead of wavenumber**: 
> 	- for a fixed topography and current, the frequency $\Omega$ is conserved when following a wave group, therefore the transport equation simplifies
> 	- In situ observation, it is much easier to obtain frequency spectrum, just requires the analysis of time-series (i.e., Fourier-Transform). 
> 		- But, many observations of the wavenumber spectrum have been obtained through remote sensing techniques.

Then, the conservation equation (i.e., transport equation) for $\hat N$ thus reads:

$$
\frac{d\hat N}{dt}=\frac{\partial }{\partial t}\hat N+\frac{\partial }{\partial \phi}(\dot{\phi}\hat N)+\frac{\partial }{\partial \lambda}(\dot{\lambda}\hat N)+\frac{\partial}{\partial \omega}(\dot{\omega}\hat N)+\frac{\partial }{\partial \theta}(\dot{\theta}\hat N) = 0 \tag{6}
$$

and $\dot{\omega}=\partial \omega/\partial t$ the term involving the derivative with respect to $\omega$ drops out in case of time-independent current and bottom.

Finally, the action density $\hat N$ is related to the normal spectral density $N$ with respect to a local Cartesian frame $(x,y)$ through: $\hat Nd\omega d\theta d\phi d\lambda=Nd\omega d\theta dx dy$, or $\hat N=NR^2\cos{\phi}$ where $R$ is the radius of the earth. 

Hence, the transportation with respect to normal spectral density $N$ is:

$$
\frac{dN}{dt}=\frac{\partial N}{\partial t}+(\cos{\phi})^{-1}\frac{\partial }{\partial \phi}(\dot{\phi}\cos{\phi}\;N)+\frac{\partial }{\partial \lambda}(\dot{\lambda}\hat N)+\frac{\partial}{\partial \omega}(\dot{\omega}\hat N)+\frac{\partial }{\partial \theta}(\dot{\theta}\hat N) = 0 \tag{7}
$$

where, with the $c_g$ the magnitude of the wave group velocity:
- $\dot{\phi}=(c_g \cos{\phi}+U_0)/R$
- $\dot{\lambda}=(c_g \sin{\phi}+V_0)/R\cos{\phi}$
- $\dot{\theta}=(c_g \sin{\theta} \tan{\phi})/R+(\dot{\mathbf{k}}\times \mathbf{k})/k^2$
- $\dot{\omega}=\partial \Omega/\partial t$
represent the rates of change of the position and propagation direction of a wave packet. $U_0,V_0$ are the components of the current in northerly and easterly direction. ~={red}==**Eq. (7) is the basic transport equation which is used in numerical wave prediction**=~==

Find the Eq. (7) in (Eq. (2.81), Janssen, 2004) 


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

