---
tags:
  - TRR_181
  - Subject-Note
Last Eddited: 2025-11-27
---
The below section is referring to the *Atmospheric Dynamics*

# Some useful formula
Dispersion relationship of Rossby waves:
$$
\omega=k\langle u_g \rangle - \frac{k\frac{\partial \langle \pi \rangle}{\partial y}}{k^2+l^2+\frac{f_0^2}{N^2}m^2+\frac{1}{4L_{di}^2}} \tag{8.85}
$$
Thus, the meridional group velocity:
$$
c_{gy}=\frac{\partial \langle \pi \rangle }{\partial y}\frac{2kl}{\biggl(k^2+l^2+\frac{f_0^2}{N^2}m^2+\frac{1}{4L_{di}^2} \biggl)} \tag{9.210}
$$
where:
- $k$ and $l$ are the zonal and meridional wavenumber, respectively
- $\langle \pi \rangle$ is the zonal mean of potential vorticity, containing contributions from the atmospheric flow field; its meridional derivative is dominated by that of the planetary vorticity:
  $$\frac{\partial \langle \pi \rangle}{\partial y}\approx \beta >0$$
because the waves originate from the mid latitude source region, one finds that:
$$ 
\begin{align}
\text{to the north of the source region:}\quad c_{gy}>0 \rightarrow kl>0 \tag{9.212} \\
\text{to the south of the source region:}\quad c_{gy}<0 \rightarrow kl<0 \tag{9.213}
\end{align}
$$
$$
\begin{align}
\frac{\partial \langle u\rangle}{\partial t}-f_0\langle v \rangle \approx M \tag{9.166} \\
\frac{\partial \langle b\rangle}{\partial t}+\langle w \rangle N^2 \approx J \tag{9.173}
\end{align}
$$

$$
\begin{align}
\langle v \rangle &=\frac{1}{f_0}\frac{\partial}{\partial y}\langle u'v' \rangle \tag{9.191} \\
\langle w \rangle &=-\frac{1}{N^2}\frac{\partial}{\partial y}\langle b'v' \rangle \tag{9.192}
\end{align}
$$
$$
-f_0\langle v \rangle =-\frac{\partial}{\partial y}\langle u'v' \rangle + \frac{\partial }{\partial z} \biggl(K\frac{\partial \langle u \rangle}{\partial z} \biggl) \tag{9.193}
$$

$$
-f_0V \approx \biggr[ K\frac{\partial \langle u\rangle}{\partial z}\biggr]_{0}^{\Delta z} \tag{9.194}
$$

$$
V=\int_{0}^{\Delta z}\langle v \rangle\;dz >0 \tag{9.195}
$$


$$
\begin{align}
\text{incompressibility:}\quad\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y}&=0 \tag{9.200}\\
\text{zonal mean:}\quad \langle\frac{\partial u}{\partial x}\rangle + \frac{\partial}{\partial y}\langle v \rangle &=0 \rightarrow  \frac{\partial}{\partial y}\langle v \rangle =0 \tag{9.201}

\end{align}
$$

# [[2025-12-01]] Note
Two sessions and a seminar of GitHub will be given
Summary for the session:
- The Dynamics of the shallow-water equations
- Quasi-geostrophic Dynamics of the stratified Atmosphere
- Interaction between Rossby waves and Mean Flow
- The Meridional Circulation
## The Dynamics of the Shallow-Water Equations
- why shallow water equations
	- single layer model for a stratified ocean or atmosphere
	- captures
		- simplest model
		- free surface displacement
- Assumptions:
	- Boussinesq fluid with constant density in the later
	- hydrostatic balance
	- nor vertical shear inside the layer (barotropic mode only)
	- no diabatic heating
	- surface pressure at surface is given by atmospheric pressure
### Single-layer dynamics and conservation properties
horizontal momentum equation:
- hydrostatic equation; mean layer thickness and surface displacement
- continuity equation: incompressibility (strong assumption), get “*change in thickness are due to horizontal convergence or divergence of the depth-integrated flow*”
Energy Conservation:
- with continuity and momentum equation
- define the energy density as the integrated energy composed by kinetic and potential energy inside the water column
Potential Vorticity Conservation:
- Taking the vertical curl of the horizontal momentum
- links the stretching and planetary vorticity
- get the ~={red}*Potential Vorticity Conservation*=~: $(\zeta + f)/h\sim \text{const}$ 
Summary:
- shallow water equation conserve:
	- Total mass in a closed domain
	- total mechanical energy (Kinetic + potential) of the integrated water-column
	- potential vorticity for each fluid parcel
### Quasi-geostrophic dynamics
- beta-plane
- the material derivative and its decomposition: $\frac{D}{Dt}= \frac{\partial }{\partial t}+ \mathbf{u}\cdot\nabla()$ 
- Introduction of Rossby number: $R=\frac{U}{f_0L}$
- Derivation by Scale Asymptotics: decompose the flow into: 
	- geostrophic wind  
	- ageostrophic wind: divergence of ageostrophic wind is not zero
- get the conservation of quasi-geostrophic potential vorticity (==QGPV==):
	- decompose: q = relative vor + planetary vor + stretching + orography
	- also the geostrophic material derivative $\frac{\partial }{\partial t}+ \mathbf{u_g}\cdot\nabla()$
- can express the relative vorticity and surface elevation using the streamfunction

### Wave solution of the shallow water equation
tools needed:
- perturbation theory
- Fourier analysis: transfer the scalar quantity into the view of wave spectrum
	- translates the system of equations into an algebric one on the coefficients
	- can be reformulated in matrix form
	- leads to dispersion relations $\omega\sim$
- Helmholtz theorem for two dimensional vector fields

**Inertial gravity wave:**
- waves on a f-plane (f-plane approximation, no beta effect)
- linearised equations: why it is called linearised equations by simply removing the mean state? → because it removes the non-linear terms (like $u'v'$)
- with the **dispersion relationships**, can discuss:
	- large wavelengths inertia waves
		- $\omega\sim 0$: geostrophic, $\omega \ne 0$
	- small wavelengths high-frequency waves

**Rossby waves - perturbation theory**
- waves on beta-plane 
	- the beta get bigger when close to the equator
- QGPV conservation
- decompose the QGPV into stationary solution (independent on time) and small perturbations
	- and express the QGPV into wave form (anstarz)
- The results:
	- the phase propagation is negative → westward propagating RWs
	- phase velocity not equals to group velocity → RWs are highly dispersive
	- very low frequency compared to inertial gravity wave
- ~={red}==**Physical interpretation**=~
	- Absolute PV conservation: change in f (in beta-plane, moving N or S) will change the relative vorticity 
	  → then the positive relative vorticity anomaly will have the tendency of moving westward
	- What about QGPV conservation?
		- the QGPV is like a wavy-streamline
		- and if the relative vorticity change, the streamline will change (as perturbated)

### Geostrophic Adjustment
A fluid in aninitially unbalanced state naturally will has the tendency to be balanced

## Quasi-geostrophic Theory of the Stratified Atmosphere

### QG-theory and its potential vorticity
The QGPV is given by: Eq. 8.10 in the textbook

### Rossby Waves (RWs)
- Gravity waves propagate in all direction awya from the disturbance, but part of the disturbance remains (geostrophic balance)
- While if considering varying f (Coriolis), the remained disturbance move westwards, which is the Rossby waves
- Gravity waves are filtered out int he QG approximation, this makes the QG a obvious choice to study RWs dynamics
- What is the propagation mechanisms of RWs? → linked to potential vorticity



- Wave-action density is transported with its group velocity, and the corresponding flux is the EP flux
- the divergence of the EP flux is the meridional flux of the potential vorticity
