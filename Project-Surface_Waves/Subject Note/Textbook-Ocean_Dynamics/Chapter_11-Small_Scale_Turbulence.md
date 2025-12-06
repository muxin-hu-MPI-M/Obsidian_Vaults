---
tags:
  - project/surfwaves
  - Theory
  - TKE
  - Ocean_Dynamics
Last Eddited: 2025-11-28
---
# Introduction

Turbulent flows are characterised by large fluctuations in space and time and occur on many space and timescales, from the large-scale oceanic and atmospheric circulation down to small-scale processes such as in the planetary boundary layer of the atmosphere or the surface mixed layer of the ocean.

The figure below shows the population of oceanic energy of the dominating motion in a space-time scale diagram.
![[Screenshot 2025-10-16 at 14.05.05.png |center]]^Space-time scales of important oceanic processes (pink areas) and scales explicitly resolved by ocean models (grey rectangular areas). Vertical dotted lines indicate the external (Ro) and first internal (Ri) Rossby radii and horizontal dotted lines indicate stability frequency (N) and Earth rotation rate (f).

- The relevant wave processes of the large-scale mean circulation are characterised by the non-dispersive long wave branch of the baroclinic planetary waves.
- The mesoscale eddy field is characterised by the short wave branch of the baroclinic planetary waves.
- The time scales of baroclinic inertial-gravity waves are rather sharply defined as being in between the stability frequency $N$ and Earth rotation frequency $f$, while spatial scales can range from global scale in case of long barotropic gravity wave down to a couple of 10 m for the baroclinic gravity wave branch.

## Pre-loding Information
### Turbulent flows are governed by the Navier-Stokes equations:
$$ 
\frac{D\mathbf u}{Dt}=-\frac{1}{\rho}\nabla p-(2\Omega\times \mathbf u)-g'k+F^{**} 
$$
### The ultimate cause of turbulence lies in the instability of flows
which tends to occur when inertial forces become large compared to other forces in the momentum balance. This circumstance can often be expressed by the Reynolds number:

$$ 
\begin{equation} 
Re=\frac{UL}{\nu}=\frac{\rho UL}{\mu} \tag{1}
\end{equation} 
$$

- where the $\nu$ is the kinematic viscosity, $\mu$ is the dynamic viscosity, $\nu=\frac{\mu}{\rho}$.
- The Reynolds number compares the magnitude of inertial to frictional forces and is always very large in turbulence flow and very low in laminar flow.

### Turbulent flows are highly sensitive to small changes in initial and boundary conditions
The evolution of turbulent flows is not predictable in detail, but has certain predictability in certain average features. This depends on the scale we are interested in:

- on time-scales of a few days: the evolution of mesoscale eddies in the ocean is of a fairly deterministic nature
- on time-scales relevant to ocean climate change, only certain mean features of the eddying system are relevant and eventually predictable.

### Diffusive nature
In a turbulent flow, the separation of two particles that are initially close together will on average increase with time. The starting point of any theory of turbulence is to accept the impossibility of predicting turbulent flows. **A consequence is that all flow variables have to be considered as stochastic, so that only statistical parameters of the flow are meaningful**.

### Small-scale Turbulence
- For turbulence on small spatial and temporal scales, Earth’s rotation is not important, and the length scales in horizontal and vertical directions are comparable → **the small-scale turbulence can often be idealised as isotropic in 3 dimensions.**
- Inertial forces are larger than viscous or gravity forces.
- Small-scale turbulence dominates the motion in the oceanic surface layer and is also important in other boundary layers

### Use of Boussinesq approximation
The Boussinesq approximation is employed throughout the chapter, so few assumptions are made:
- **The flow is incompressible (i.e., mass conservation)**
- **The density variations are small and can be neglected everywhere expect where they multiply by gravity** (i.e., in the buoyancy term)
    - Inertia term: density is constant $\rho_{o}$
    - Buoyancy term: keep density variations $\rho'$ in the gravitational term where the buoyancy is defined as
      $$ \begin{equation} b=-g\frac{\rho'}{\rho_{0}} \tag{2} \end{equation} $$
Then, the Navier-Stokes equation (simplified form, neglect rotation):
$$ 
\begin{equation} 
\frac{\partial \mathbf u}{\partial t}+(\mathbf u \cdot \nabla)\mathbf u=-\frac{1}{\rho}\nabla p+ \mathbf g+ \nu \nabla^{2}\mathbf u  \tag{3}
\end{equation} 
$$

where $\mathbf g$ is the gravity vector $\mathbf g=-g\vec{k}$
Will replace the density and pressure to $p=p_{0}(z)+p'(x,y,z,t)\quad and\quad \rho=\rho_{0}+\rho'(x,y,z,t)$, then the background mean flow is approximately hydrostatic equilibrium: $\nabla p_{0}=\rho_{0}g$. The N-S equation with other required equations are therefore in the form:

$$ 
\begin{equation} \tag{4}
\begin{aligned} 
\frac{\partial \mathbf u}{\partial t}+(\mathbf u \cdot \nabla)\mathbf u&=-\frac{1}{\rho_{0}}\nabla p'+b+\nu \nabla^{2}\mathbf u \\ 
\nabla \cdot \mathbf u&=0 \\ \rho_{0}\frac{D}{Dt}
\begin{pmatrix} S' \\ 
\theta' \end{pmatrix}&=\begin{pmatrix} \zeta_{S} \\ \zeta_{\theta} \end{pmatrix} \\
\rho'
&=F(S_{c}+S', \theta_{c}+\theta', p_{0}(z))-F(S_{c}, \theta_{c}, p_{0}(z)) \\ 
&=( \frac{\partial \rho}{\partial S})_{T,p}S' + (\frac{\partial \rho}{\partial \theta})_{S,p}\theta' \end{aligned} 
\end{equation} 
$$

**The buoyancy term $b$ will only be included in the vertical component of the momentum equation** (since the $\rho'$ cannot be ignored)


**The third equation in equation set (4) represents the change of salinity and potential temperature of a parcel as it moves** with the flow is in balance with the net input terms. While the $\zeta_{S}$ and $\zeta_{\theta}$ are the non-conservative source or sink terms for the salinity and potential temperature (e.g., small-scale diffusion).

The fourth equation stands for the “Equation of state”. For small perturbations, this state equation is often linearised (see Chapter 11.3, Eq (56))


# 11.1 Kolmogorov’s Theory of Homogeneous Turbulence

The idealised “Homogeneous” turbulence, some assumptions:

- all statistical properties of the turbulent flow are independent of location
- all physical variables are considered as homogeneous and stationary random variables
- homogeneity → absence of mean density stratification → assume density variations are unimportant $\rho \approx const$
- Coriolis force is dominated by the inertial force → neglected

### Covariance Tensor
Consider as the random function, the turbulent velocity $\mathbf u'(\mathbf x,t)=\mathbf u(\mathbf x,t)-\overline{\mathbf U}(\mathbf x)$ can be described by its mean and its covariance tensor (or so called Reynolds stress tensor when multiplied by density):

$$ \begin{equation} R_{ij}(\mathbf r,t)=\overline{u'_{i}(\mathbf x,t)u'_{j}(\mathbf x+\mathbf r,t)} \tag{5}\end{equation} $$

Here:

- $u'_{i}(\mathbf x,t)$ is the fluctuating velocity component in the i-direction at position $\mathbf x$ and time $t$
- the over-line denotes an ensemble average or long-time average
- $r$ is the separation vector between 2 points

The over-bar average out the randomness, leaving only the statistical structure of turbulence (which is the covariance of the turbulent velocity). this covariance term will be the nonlinear term as an additional stress that acts like a turbulent viscosity

> [!Attention]
> **Physical meaning:**
> - This tensor describes how velocity fluctuations at one point in space are correlated with those at another
>     - When $r=0$: this is the Reynolds stress tensor, quantifying the covariance between difference velocity component at the same location
>     - When $r\not= 0$: measures the spatial correlation between fluctuations at two points separated by $r$. It provides information about the **size of turbulent eddies** and how quickly correlations decay with distance.

The covariance tensor, or equivalently the spectral energy tensor $E_{ij}(\mathbf k,t)$, $\mathbf k$ is the wavenumber vector. Thus, the **covariance tensor in terms of spectral density** (or the Fourier transform of the $R_{ij}$) is:

$$ \begin{equation} R_{ij}(\mathbf r,t)=\int E_{ij}(\mathbf k,t)e^{i\mathbf k\cdot r}d\mathbf k \tag{6}\end{equation} $$

where the $e^{i\mathbf k\cdot r}$ is the Fourier kernel, connecting spectral space ($\mathbf k$) and physical space ($\mathbf r$). This form contains all relevant information about the turbulent field. In particular, the scalar energy spectrum:

$$ \begin{equation} E(\mathbf k,t)=\frac{1}{2}\int E_{ii}(\mathbf k,t)df(\mathbf k) \tag{7}\end{equation} $$

Where the $E_{ii}$ is the sum over diagonal components, and the **$E(\mathbf k,t)$ here is the spectral density of Turbulent Kinetic energy (TKE) per unit mass (or the spectral contribution to TKE at wavenumber k).** This means that the TKE is obtained by integrating the contributions of all scales (all wavenumber)

### Covariance Tensor and TKE
> [!Attention]
> Specifically, the Turbulent kinetic energy (TKE) per unit mass is given by:
> $$ \begin{equation}\tag{8} \begin{split} \varepsilon=TKE &=\frac{1}{2}\overline{u'_{i}u'_{i}} \\ &= \frac{1}{2}\overline{(u_{x}')^{2}}+\overline{(u_{y}')^{2}}+\overline{(u_{z}')^{2}} \\ &=\frac{1}{2}R_{ii}(r=0) \quad \quad \text{physical space} \\ &=\int_{0}^{\infty} E(k)dk \quad \quad \text{ spectral space} \end{split} \end{equation} $$

Hence, the TKE is the sum of the variances of the velocity fluctuations in all directions

Because the turbulent velocity field has to satisfy the N-S equations. The continuity equation (i.e., the conservation of mass; incompressible flow) leads to constraints for both $R_{ij}$ and $E_{ij}$:
$$ \frac{\partial R_{ij}}{\partial r_{i\;or\;j}}=0 $$

## 11.1.1 Isotropy

Formally, isotropy means that the statistical variables are invariant against rotation around an arbitrary axis, against reflection at an arbitrary plane and against translation in an arbitrary direction.

**Isotropic turbulence**, therefore, implies that **all statistical properties, including the mean and the covariance tensor, are also invariant against any possible combination of rotation, mirroring (反射）, and translation （平移）**.
- statistical properties are identical in all directions
- equal variances, zero cross correlation

As an isotropic scalar statistical property $\lambda$, it cannot depend on the direction of the vector $\mathbf r$, hence $\lambda=\lambda(r)$. ==**While for a single vector field (e.g., $\mathbf u$), isotropy of the vector implies zero mean ($\overline{\mathbf u}=0$)**==. **Hence, for turbulent fluctuations $\mathbf u'=\mathbf u-\overline{\mathbf U}$, it is isotropy by construction as $\overline{\mathbf u'}=0$**.

The isotropic form of the energy spectral tensor has the general form:
$$ \begin{equation} E_{ij}(\mathbf k)=A(k)k_{i}k_{j}+B(k)\delta_{ij}=A(k)[k_{i}k_{j}-k^{2}\delta_{ij}] \tag{9}\end{equation} $$

The second term is valid since the continuity requires that $k_{i}E_{ij}=0$ (in Fourier space) which implies $B(k)=-k^{2}A(k)$. With [Eq (7)](https://www.notion.so/Ocean-Dynamic-Chapter-11-Small-Scale-Turbulence-28e69691c52b800780bff3677d418d3b?pvs=21), the spectral density of TKE at wavenumber $k$ per unit mass is given:
$$ \begin{equation} E(\mathbf k)=\frac{1}{2}4\pi k^{2}E_{ii}(k)=-4\pi k^{4}A(k) \tag{10}\end{equation} $$

with:
$$ \begin{equation} E_{ij}(k)=\frac{E(k)}{4 \pi k^{4}}(k^{2}\delta_{ij}-k_{i}k_{j}) \tag{11} \end{equation} $$

In spherical coordinates, integrating over directions in k-space gives the factor $4 \pi k^2$ (the area of a spherical shell in k-space). This scalar energy spectrum $E(k)$ is the preferred variable for a discussion of isotropic turbulent flows

## 11.1.2 Momentum and Kinetic Energy in Homogeneous Turbulence

The dynamics of turbulent motion is governed by:
- **Continuity equation:** $\nabla \cdot \mathbf u=0$
- **Momentum equation, neglect Earth’s rotation** as the small-scale motions with time-scales much shorter than one day
### Momentum equation for Homogenous Turbulence
For constant density $\rho$ (except the buoyancy term), the momentum equation in Eq. (4) can be written in tensor notion as:
$$ \begin{equation} \frac{\partial u_{i}}{\partial t}=-\frac{\partial u_{l}u_{i}}{\partial x_{l}}-\frac{\partial p^{*}}{\partial x_{i}}+\kappa_{m}\frac{\partial^{2}u_{i}}{\partial x_{l}^{2}} \tag{12} \end{equation} $$
where
- **Inertial force**: $\frac{\partial u_{l}u_{i}}{\partial x_{l}}$ is the advection term, which is the nonlinear part of $\mathbf u \cdot\nabla \mathbf u$ in vector form
    $$ \frac{\partial u_{l}u_{i}}{\partial x_{l}}=u_{i}\cdot \nabla \mathbf u $$
    
- **Pressure force**: $\frac{\partial p^{*}}{\partial x_{i}}$ is the pressure gradient, with the $p^{*}$ as the combined scale pressure (dynamic + modified hydrostatic pressure in Boussinesq approximation, divided by density). $\Phi$ is the gravity potential
    $$ p^{*}=\frac{p}{\rho} + \Phi $$
    
- **Friction force**: The third term is the viscous diffusion term, **$\kappa_{m}$ denotes the kinematic viscosity of seawater ($\kappa_{m}=\nu/\rho$)**

The statistical mean of the Eq. (12) is given by:
$$ \begin{equation} \frac{\partial \overline{u_{i}}}{\partial t}=-\frac{\partial \overline{u_{l}u_{i}}}{\partial x_{l}}-\frac{\partial \bar p^{*}}{\partial x_{i}}+\kappa_{m}\frac{\partial^{2} \overline{u_{i}}}{\partial x_{l}^{2}}=0 \tag{13}\end{equation} $$

as the mean of the vector is zero as isotropy.

==**Hence the mean velocity of homogenous turbulence is constant in time and space and can be ignored.**

### Equations for Velocity Covariance
The evolution of Covariance tensor $R_{ij}$ is:
$$ \begin{equation} \frac{\partial R_{ij}(\mathbf r)}{\partial t} = T_{ij}(\mathbf r) + P_{ij}(\mathbf r) + 2\kappa_{m}\frac{\partial^{2} R_{ij}(\mathbf r)}{\partial r_{l}\partial r_{l}} \tag{14}\end{equation} $$

Three terms on the right-hand side of Eq. (14) correspond directly to the inertial, pressure, and frictional terms in the momentum equation. This formula introduce two more tensors: the inertial tensors $T_{ij}$ and pressure tensor $P_{ij}$

### Spectral Description
All tensors in Eq. (14) can be represented by the spectral description; Thus, introduce the inertial spectral tensor $\Gamma_{ij}$ and the pressure spectral tensor $\Pi_{ij}$ and thus the spectral balance is obtained by multiplication of Eq. (14) with $e^{-ik\cdot \mathbf r}$ and integration over $\mathbf r$, yielding:
$$ \begin{equation} \frac{\partial E_{ij}(\mathbf k)}{\partial t}=\Gamma_{ij}(\mathbf k)+\Pi_{ij}(\mathbf k)-2\kappa_{m}k^{2}E_{ij}(\mathbf k) \tag{15}\end{equation} $$

The spectral inertial tensor $\Gamma_{ij}$/ $\Pi_{ij}$ describes the effect of inertial forces/pressure forces on the energy tensor $E_{ij}$.

> [!Attention]
> They follows that:
> - - Inertial forces cannot change the momentum as well as the total energy, but can redistribute energy between different wave numbers.
> - - Pressure forces cannot change the kinetic energy at any wave number, but can redistribute kinetic energy between different velocity components.

### Scalar Energy Balance
> [!Attention]
> The energy balance can be written as:
> $$ \begin{equation} \frac{\partial}{\partial t}E(k)=-\frac{\partial F(k)}{\partial k}-2\kappa_{m}k^{2}E(k) \tag{16}\end{equation} $$

where the $F(k)$ is the spectral energy transport in wave-number space. Which is defined as convergence of an energy flux in wave-number space:
$$ \begin{equation} F(k)=-\int_{0}^{k} \Gamma(k')dk' \tag{17}\end{equation}

$$

And the $\Gamma(k)$ is the scalar projection of the inertial tensor:
$$ \begin{equation} \Gamma(k)=\frac{1}{2}\int \Gamma_{ii}(\mathbf k)df(\mathbf k) \tag{18}\end{equation} $$

and because the inertial force cannot change the momentum, $\int \Gamma(k)dk=0;\quad F(0)=0$.

==**The flux $F(k)$ redistributes energy among different wave numbers. Since dissipation is more effective at small scales, it is plausible that energy has to be transported by $F(k)$ from large scales (small k) to small scales (large k).**== 

Integration of Eq. (16) over all wave numbers k yield:
$$ \begin{equation} \frac{d\varepsilon}{dt}=-\epsilon \tag{19}\end{equation} $$

Which stands for “_**The rate of decrease of turbulent kinetic energy (TKE) per unit mass equals the rate of energy dissipation.**_”

Using the definition for mechanical dissipation of Section 2.4.2 in the Boussinesq approximation and for isotropic turbulence, it can be written as:
$$ \begin{equation} \epsilon=2\kappa_{m}\overline{D^{2}_{ij}}=\kappa_{m}\overline{\frac{\partial u_{j}}{\partial x_{i}}\frac{\partial u_{j}}{\partial x_{i}}} \tag{20}\end{equation} $$

with the deformation tensor $D_{ij}=(1/2)(\partial u_{i}/\partial x_{j}+\partial u_{j}/\partial x_{x})$


## 11.1.3 Large and Small length scales

Assume that the flux $F(k)$ in wave-number space transports energy from large (small $k$) to small scales (large $k$), since dissipation is more effective at small scales.

To further discuss the concept of such an energy flux (or cascade) we need to specify the definitions of large and small scales, which will become the integral length scale and the micro scale, respectively.

### Integral Length Scale
The integral length scale $L$ (also called macro scale or longitudinal) is defined as:
$$ \begin{equation} L=\int_{0}^{\infty}f(r)dr \tag{21}\end{equation} $$
where $f$ is the form of a random function. $L$ **describes the distance over which correlation is essentially nonzero**. Hence $L$ must be roughly equivalent to the scale of the largest coherent fluctuations (i. e. to the largest eddies) → **$L$ measures the typical size of energy-containing eddies (which will be discussed in [11.2.1]** In terms of the energy spectrum:
$$ \begin{equation} L=\frac{3\pi}{4}\frac{\int k^{-1}E(k)dk}{\int E(k)dk} \tag{22}\end{equation} $$

Where:
- The numerator $\int k^{−1}E(k) dk$ gives an energy-weighted average length scale.
- The denominator $\int E(k)dk=\frac{1}{2}\langle u'^2 \rangle$ gives the total turbulent kinetic energy per unit mass.

While the integral time scale is closely related to the integral length scale, which is given by:
$$ T_{int}=\int_{0}^{\infty}f(\tau)d\tau $$
==$T_{int}$ **describes the time over which the velocity of a fluid parcel remains correlated with itself**==, a measure of how long an eddy remembers its motion.

==**Large eddies with length scale $L$ will predominantly contribute to the total energy $E$.== On the other hand, the total energy (i. e. its rate of decrease if there is no energy input, or its forcing if there is) and thus the properties of the large eddies are directly related to the dissipation.** Therefore, one can assume that the dissipation is determined by the energy and scale of the large eddies → $\epsilon=f(\varepsilon,L)$. A simple dimensional argument shows that the only possible combination between $E$ and $L$ to form the dimension of $\epsilon$ is given by:
$$ \begin{equation} \epsilon=c_{\epsilon}\frac{\varepsilon^{3/2}}{L} \tag{23}\end{equation} $$

Where the $c_{\epsilon}$ denotes a dimensionless constant which is experimentally found. The timescale for the decrease of kinetic energy is: 
$$ \begin{equation} T_{0}=\frac{\varepsilon}{\frac{d\varepsilon}{dt}}=\frac{\varepsilon}{c_{\epsilon}\varepsilon^{3/2}/L}=\frac{1}{c_{\epsilon}}\frac{L}{\varepsilon^{1/2}}=\frac{1}{c_{\epsilon}}\frac{L}{U} \tag{24}\end{equation} $$
where the $L/U=T_{adv}$ denotes an advective time-scale of the large eddies. Therefore, the energy content is changing on the time-scale $T_{adv}$. As a consequence, there must be energy input on the scale $L$ to sustain the turbulent flow.

### Micro Scale
For small $r$, the longitudinal correlation $f(r)$ can be expanded in a Taylor series, one can get:
$$ \begin{equation} \lambda^{2}=-\frac{1}{f''(0)} \tag{25}\end{equation} $$
The length scale $\lambda$ reflects the curvature of $f(r)$ at $r=0$ and is referred to as TAYLOR’s micro scale, which can also be expressed in terms of energy spectrum:
$$ \begin{equation} \frac{1}{\lambda^{2}}=\frac{1}{5}\frac{\int k^{2}E(k)dk}{\int E(k)dk} \tag{26}\end{equation} $$
Replacing integral with $\varepsilon$ and $\epsilon$ in Eq. (19), one gets:
$$ \begin{equation} \lambda^{2}=10\kappa_{m}\varepsilon\epsilon^{-1} \tag{27}\end{equation} $$

It follows that the micro scale is related to dissipation. But it turns that this micro scale at which viscous forcing dominates inertial forces, it is still much larger.

### Relation between Integral Length Scale and Micro Scale
In Eq. (23) and Eq. (27) they both have the expression of the dissipation $\epsilon$. Thus two equations can be connected, yielding:
$$ \begin{equation} \frac{L^{2}}{\lambda^{2}}=c_{\epsilon}\frac{\varepsilon^{1/2}L}{10\kappa_{m}}=c_{\epsilon}\frac{Re_{L}}{10} \tag{28}\end{equation} $$
where the Reynolds number of large eddies are introduced:
$$ Re_{L}=\frac{UL}{\kappa_{m}}=\frac{\varepsilon^{1/2}}{\kappa_{m}} $$
Hence:
- Turbulent flow: $Re_{L}>> 1$, we have $L>>\lambda$
- For an energy spectrum with strong exponential decay for $k>1/L$, the length scales $L,\lambda$ have the same order of magnitude.


## 11.1.4 Equilibrium Range and Inertial Subrange

In smaller scales, there is no need to have energy input to sustain the turbulence, hence the Energy balance equation in Eq. (16) can reduce to:
$$ \begin{equation} \frac{\partial F(k)}{\partial k}=-2\kappa_{m}k^{2}E(k) \quad \text{or}\quad F(k)=2\kappa_{m}\int_{k}^{\infty}k'^{2}E(k')dk' \tag{29}\end{equation} $$
or any wave number k in the equilibrium range.

Here $F(k)>0$, i.e., **energy, is indeed transported through the spectrum toward higher wave numbers and is finally dissipated at small scales**.

We further assume that the energy is transported between wave numbers which are close together (turbulent cascade), so that in the equilibrium range the details of the flow at the large scale $L$ are irrelevant. It is then clear that the spectrum $E(k)$ must depend on $k$, $\kappa_{m}$ and $\epsilon$ in the equilibrium range. In the absence of other, e. g. geometric, constraints, that dependence must be universal, i.e., $E=\Phi(k, \epsilon, \kappa_{m})$.

> [!Attention]
> 🔥 These variables in this universal relation have the dimensions:
> - - Turbulent energy spectrum at per unit mass $E=m^{2}s^{-2}$;
> - - Turbulent kinetic energy per unit mass $\varepsilon \sim \frac{1}{2}U^{2}\sim m^{2}s^{-2}$
> - - Wave number $k=m^{-1}$;
> - - Kinematic viscosity of seawater **($\kappa_{m}=\nu/\rho$):** $\kappa_{m}=m^{2}s^{-1}$
> - - Energy dissipation rate at per unit mass: $\epsilon =m^{2}s^{-3}$

With the dimensional analysis, we can introduce:
$$ \begin{equation} \lambda_{d}=(\kappa_{m}^{3}/\epsilon)^{1/4} \tag{30}\end{equation} $$

Which is given by dissipation and viscosity and **is called the dissipation length scale or Kolmogorov’s microscale.**

One can show that the dissipation spectrum $k^{2}E(k)$ in Eq. (29) has a maximum near the wave number $1/\lambda_{d}$. Defining the velocity scale in the dissipation range as:
$$ \begin{equation} U_{d}=(\epsilon\kappa_{m})^{1/4} \tag{31}\end{equation} $$

which is the only combination with the correct dimension.

The dissipation time scale for this dissipation velocity scale is:
$$ \begin{equation} T_{d}=(\frac{\kappa_{m}}{\epsilon})^{1/4} \tag{32}\end{equation} $$

The Reynolds number at the dissipation length scale $\lambda_{d}$ follows as $Re_{d}=U_{d}\lambda_{d}/\kappa_{m}=1$. **Hence, at the scale $\lambda_{d}$ inertial forces no longer dominate, and the turbulent transfer of energy ends here.**

Hence, the three length scale are well separated in the normal case where the Reynolds number is large ($Re \gg1$). **The micro scale $\lambda$ is much smaller than the integral scale $L$, but also much larger than the dissipation scale $\lambda_{d}$ at which the viscous and inertial forces become comparable**

### Inertial Subrange
**For wave numbers in the range $1/L \ll k\ll 1/\lambda_{d}$,** dissipation must be small, only the inertial forces are important for the flow in the inertial subrange. According to Eq. (29), the energy flux is constant and equals the dissipation $\epsilon$. Therefore, one can assume that the characteristic features of the turbulence depend only on $\epsilon$ but not on the viscosity $\kappa_{m}$. For energy spectrum, this means that $\phi(k\lambda_{d})=const=c_{k}$, hence:
$$ \begin{equation} E(k)=c_{k}\epsilon^{2/3}k^{-5/3} \tag{33}\end{equation} $$
![[Screenshot 2025-10-20 at 10.29.31.png | center]]^Conceptual sketch of the turbulent energy spectrum in the inertial subrange (logarithmic scaling), which extents from the integral length scale of the largest eddies to the dissipation length scale.

Prerequisite for the existence of an inertial subrange is:
- **a very large Reynolds number** on the large scale $L$, an assumption which often holds in the ocean and the atmosphere.
- This spectral law holds only for **small-scale three-dimensional and isotropic turbulence**, an important restriction


## **Summary of Kolmogorov’s theory of turbulence:**

>[!Attention]
>**Length scale and equilibrium range**
>Kolmogorov viewed turbulence as a **cascade of energy**: large eddies break down into smaller and smaller eddies, transferring kinetic energy through a hierarchy of scales until it finally dissipates by viscosity.
>So there are **three main length scales** in this cascade:

| Name                                                     | Physical meaning                                                                                                     | Dominant forces                                                                                 | Reynolds number |
| -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | --------------- |
| **Integral scale**                                       | size of the largest eddies; where energy is injected into the flow (e.g. from wind shear or convection)              | **Inertial forces dominate**, energy is injected into turbulence (sustained by external energy) | $Re\gg1$        |
| **Inertial range scales**                                | Intermediate scales between integral scale and dissipation scale. Energy cascades through these scales without loss. | **Inertia dominates**, negligible viscosity; energy is transferred but not dissipated.          | $Re\gg1$        |
| **Kolmogorov microscale** <br>(dissipation length scale) | Smallest eddy size — where viscosity **finally** dissipates kinetic energy into heat.                                | **Viscous and inertial forces become comparable**; energy transfer by turbulence ends           | $Re\sim 1$      |

# 11.2 Turbulent Mixing

Consider the concentration of substance or tracer $\psi$ in a turbulent velocity field which in Boussinesq approximation is governed by:
$$ \begin{equation} \frac{\partial \psi}{\partial t} + \mathbf u \cdot \nabla \psi =\nabla \cdot \kappa \nabla \psi \tag{34}\end{equation} $$
This is called **adevection-diffusion equation**, or transport equation which describes the scalar quantity. The three terms are (1) local change term; (2) advection term and (3) diffusion term (spreading of scalar by molecular diffusion)

With Reynolds decomposition:
$$ \begin{equation} \mathbf u=\overline{\mathbf u}+\mathbf u' \quad \quad \psi=\overline{\psi}+\psi' \tag{35}\end{equation} $$
where the $\overline{\psi '}=0$ and $\overline{\overline{\psi}}=\overline{\psi}$ and equivalently for $\mathbf u$. Further useful properties of the statistical mean are:=
- $\overline{(\psi+\lambda \phi)}=\overline{\psi}+\lambda\overline{\phi}$
- $\overline{\phi \overline{\psi}}=\overline{\phi}\:\overline{\psi}$

for any constant $\lambda$ and random functions $\phi,\psi$ and it commute with differential operators.
With these decompositions, **the statistical averaging of the advection-diffusion equation (Eq. (34)) is**:
$$ \begin{equation} \frac{\partial \overline{\psi}}{\partial t}+\overline{\mathbf u} \cdot \nabla \overline{\psi} = \nabla \cdot (\kappa\nabla\overline{\psi}-\overline{\mathbf u' \psi'}) \tag{36}\end{equation} $$

Where the **additional turbulent flux** $\mathbf J_{\psi}^{turb}=\overline{\mathbf u'\psi'}$, which originates from the advective term in Eq. (34) and which is generally unknown but almost always much larger than the (mean) molecular tracer flux $\kappa \nabla \overline{\psi}$

> [!Attention]
> The turbulent flux can be related to the mean gradient of the scalar quantity:
> $$ \begin{equation} \overline{\mathbf u'\psi'}=-K\nabla\overline{\psi} \tag{37}\end{equation} $$
> with the **turbulent diffusivity $K$.**
> 

>[!Tip]
>**Noted that the turbulence $\not=$ diffusion!** But can behave like diffusion statistically. This is the fundamental base for the validity to define the turbulence diffusivity !!
>

Since for $K>0$ relation corresponds to ==_**down-gradient tracer transport,**_== it is also refereed to as down-gradient parameterisation

>[!Tip]
>**Down-gradient transport**: means the turbulent flux acts to reduce the existed gradient, i.e., it moves the quantity from regions of high mean value toward regions of low mean value
>- - e.g., if $\overline{\psi}$ increases with height → $\partial \overline{\psi}/\partial z >0$ → turbulent flux $\overline{w'\psi'} \sim -\partial \overline{\psi}/\partial z<0$ → turbulent flux carry $\psi$ from high to low value region → act to reduce the positive vertical gradient


## 11.2.1 Heuristic Approaches

Two classical approaches:
- based on mixing by energy-containing eddies
- mixing-length concept

### Mixing by Energy-Containing Eddies
A turbulent flow is characterised by fluctuations over a range of different length scales.
A simple from of the turbulent flux can be obtained by **assuming that mixing is mainly caused by eddies with maximum energy, with a length scale $L$ (integral length scale for large eddies)**
Consider:
- a tracer with mean concentration $\overline{\psi}(x)$.
- Let the turbulent flow be characterised by eddies with characteristic swirl velocity $U$ and diameter $L$.
To estimate the transport of by the turbulent flow, we note that the tracer flux from $x_{0}-L/2$towards $x_{0}$ is approximately $U\overline{\psi}(x_{0}-L/2)$, and likewise the flux from $x_{0}+L/2$ towards $x_{0}$ is approximately $-U\overline{\psi}(x_{0}+L/2)$. The total tracer flux $J$ summed over both cases is approximately:
$$ J=U[\overline{\psi}(x_{0}-L/2)-\overline{\psi}(x_{0}+L/2)]=-UL\frac{\overline{\psi}(x_{0}+L/2)-\overline{\psi}(x_{0}-L/2)}{L} $$

Provided that the scale over which the mean tracer varies is larger than $L$, this can be approximated as:
$$ \begin{equation} J\approx-UL\frac{\partial \overline{\psi}}{\partial x} \tag{38}\end{equation} $$

Identifying the turbulent tracer flux $J=\overline{u'\psi'}$, it follows that:
$$ \begin{equation} K=UL\sim\varepsilon^{1/2}L \tag{39}\end{equation} $$

Where the $\varepsilon\sim\frac{1}{2}\overline{u_{i}u_{i}}$ is the TKE per unit mass.

==**Hence, the turbulent diffusivity $K$ can be estimated by the magnitudes of the characteristic velocity and length scale of the eddy==**

### Mixing Length
Introduced by Prandtl. Consider:
- Isotropic flow
- A horizontally homogeneous mean shear flow $\overline{u}(z)$ with $\partial \overline{u}/\partial z>0$
- A turbulence element with a vertical velocity perturbation $w'>0$ will carry a fluid element (and its mean momentum) from the initial level $z_{0}$ upward to a new level $z_{0}+l'$ before it is mixed:
  → Assume that the **parcel retains the mean property it had at its original position towards the new level**
![[Screenshot 2025-10-19 at 22.23.17.png |center]]^*Schematic of the mixing-length concept in which a sheared horizontal mean flow is subject to turbulent vertical exchange of momentum.*

Then, the velocity of the fluid element deviates from the ambient velocity by:
$$ \begin{equation} u'=\overline{u}(z_{0})-\overline{u}(z_{0}+l')<0 \tag{40}\end{equation} $$
as of $\partial \overline{u}/\partial z>0$. While if $w'<0$, then $u'>0$, illustrated in the figure below

In both cases, it is clear that $\overline{u'w'}<0$. Expanding the $\overline{u}(z_{0}+l')$ in a Taylor series (when the vertical deviation $l'$ is small), which leads to:
$$ \overline{u}(z_{0}+l')=\overline{u}(z_{0})+l'\frac{\partial \overline{u}}{\partial z}\bigg|_{z_{0}}+\frac{1}{2}(l'^{2})\frac{\partial^{2} \overline{u}}{\partial^{2} z}\bigg|_{z_{0}} +... $$
Then we’ll have the expression of $u'$, and **ignore the higher order terms** (**because $l'$ is much smaller compared to the scales of the mean flow**)
$$ \begin{equation} u'=-\overline{u}(z_{0}+l')+\overline{u}(z_{0})\approx-l'\frac{\partial \overline{u}}{\partial z}\bigg|_{z_{0}} \tag{41}\end{equation} $$

It follows that
$$ \begin{equation} \overline{u'w'}=-\overline{w'l'}\frac{\partial \overline{u}}{\partial z} =-K_{u}\frac{\partial \overline{u}}{\partial z} \tag{42}\end{equation} $$
One can expect $w'$ and $l'$ to be positively correlated → $\overline{w'l'}>0$

Assuming isotropy so that $\overline{w'^{2}}\approx\overline{u'^{2}}$, this result is equivalent to:
$$ \overline{w'l'}=c(\overline{w'^2}\:\overline{l'^2})^{1/2}=c(\overline{u'^2}\:\overline{l'^2})^{1/2}=c(\overline{l'^2}(\frac{\partial \overline{u}}{\partial z})^{2}\:\overline{l'^2})^{1/2} $$

> [!Attention]
> Thus, the turbulent diffusivity:
> 
> $$ \begin{equation} K_{u}=cl^{2} \bigg|\frac{\partial \overline{u}}{\partial z}\bigg| \tag{43}\end{equation} $$
> 
> where the **length scale $l=(\overline{l'^2})^{1/2}$ is the** _**time-mean mixing-length**_. Under certain conditions, it can be comparable to the scale of the energy-containing eddies with a length scale $L$.

> [!Tip]
Note that termination of the Taylor series in Eq. (41) after the first term **requires that the length scale $l$ is small compared to the mean flow.** Hence, the mixing-length can be valid only if:
$$ \begin{equation} l\ll\ L_{m}=\bigg|\frac{\partial \overline{u}}{\partial z}\bigg|/{\bigg|\frac{\partial^{2} \overline{u}}{\partial z^{2}}\bigg|} \tag{44}\end{equation} $$
>where:
>- - $L_{m}$: characteristic vertical length scale of the mean flow → where the velocity shear term changes significantly
>- - $\big|\partial \overline{u}/\partial z\big|$: how fast the mean velocity changes with height
>- - $\big|\partial^{2} \overline{u}/\partial z^{2}\big|$: how fast that shear changes with height
>
>Or more mathematically speaking, the first order of Taylor’s expansion should be much greater than higher order terms

### Diffusion in the Inertial Subrange
The concept from “mixing by energy-containing eddies” (Eq. (39)) can also be modified for situations where the large eddies are explicitly resolved, and turbulent diffusion is caused by velocity fluctuations on smaller scales.

Specifically, assume that only wave numbers $k\ge k_{*}$ in the inertial subrange contribute to velocity. The energy $\varepsilon_{*}$ of these fluctuations is:
$$ \begin{equation} \varepsilon_{*}=\int_{k_{*}}^{k_{d}}c_{k}\epsilon^{2/3}k^{-5/3}dk\approx \int_{k_{*}}^{\infty}c_{k}\epsilon^{2/3}k^{-5/3}dk=\frac{3c_{k}}{5}\epsilon^{2/3}k_{*}^{-2/3} \tag{45}\end{equation} $$
Assume that the diffusivity $K_{s}$ associated with these fluctuations is given as $K_{s}=\varepsilon_{*}^{1/2}l_{*}$according to Eq. (39), with $l_{*}=1/k_{*}$. One obtains (up to the first order):
$$ \begin{equation} K_{s}\approx\epsilon^{1/3}l_{*}^{4/3}=\varepsilon^{1/2}L(l_{*}/L)^{4/3} \tag{46}\end{equation} $$
where the relation in [Eq. (23)](https://www.notion.so/Ocean-Dynamic-Chapter-11-Small-Scale-Turbulence-28e69691c52b800780bff3677d418d3b?pvs=21) has been used for the last equality.

> [!Attention]
> In numerical circulation models, Eq. (46) is occasionally used to parameterise *subgrid* scale motions. It is possible to relate the scale $l_{*}$ with the resolution, i.e., $l_{*}\approx\Delta x$, which is particularly useful when the resolution is variable.


## 11.2.2 Turbulent Diffusion in the Lagrangian Reference System

In Lagrangian coordinates, it is possible to relate the mean rate of spreading with statistical properties of the flow field. The following description was given by Taylor.
A particle is advected by a **turbulent flow $u(x,t)$ (assumed to be 1D and stationary for simplicity), with $\overline{u}(x,t)=0$. Which means $u(x,t)=u'(x,t)$**
The particle starts from the location $x_{0}$ at time $t_{0}$, and is at the time $t$ at the location $x(t)$.
In the Lagrangian system one has:
$$ \frac{dx(t)}{dt}=v(t)\quad or \quad x(t)=\int_{0}^{t}v(t')dt'
$$
where the $v(t)=u(x(t),t)$ denotes the Lagrangian velocity at position $x(t)$ and time $t$.
**Since $\overline{v}=\overline{u}=0$, $v=v'$ and it follows that $\overline{x}=0$ → on time average, the particle does not move away from its origin.**

The mean quadratic excursion of the particle $\overline{x^{2}}$ (**how far particles have spread due to e.g., turbulent**)is given by:
$$ \frac{d\overline{x^{2}}}{dt}=\overline{2x(t)\frac{dx(t)}{dt}}=\overline{2v(t)\int_{0}^{t}v(t')dt'}=2\int_{0}^{t}\overline{v(t)v(t')}dt' $$
and thus leads to Taylor’s diffusion equation
$$ \begin{equation} \frac{d\overline{x^{2}}}{dt}=2\int_{0}^{t}R^{L}(\tau)d\tau \tag{47}\end{equation} $$
Here the $R^{L}(\tau)=R^{L}(t-t')$ denotes the ==*Lagrangian velocity covariance*== which is characterised by its value at origin, $R^{L}(0)=\overline{v^{2}}\quad or\quad \overline{v'^{2}}$, or the variance of the Lagrangian velocity fluctuation

and the **integral time scale (also introduced in 11.1.3):**

$$ T_{int}=\frac{1}{R^{L}(0)}\int_{0}^{\infty}R^{L}(\tau)d\tau $$

tells how long the Lagrangian velocity “remembers” its past values, **i.e., the average time over which a particle’s velocity remains correlated with itself. Physically, $T_{int}$ is the average lifetime (or persistence time) of turbulence eddies.** A closely related variable is the **integral length scale**, which measures the typical size of energy-containing eddies (largest eddies).

> [!Attention]
> Of particular interest is the limiting case for which the **diffusion time $t$ is much larger than the integral time-scale, $t\gg T_{int}$ as this is when turbulent motion ‘behaves’ like a diffusion process 
> → prerequisite** for using the turbulent diffusivity (Eq. 37) In this case, $\int_{0}^{t}R^{L}(\tau)d\tau\approx\int_{0}^{\infty}R^{L}(\tau)d\tau=\overline{v^{2}}T_{int}$, and it follows that
> 
> $$ \begin{equation} \overline{x^{2}(t)}=2Kt\quad \text{with}\quad K=\overline{v^{2}}T_{int} \tag{48}\end{equation} $$
> 
> $K>0$ is referred to as turbulent diffusivity.
> 
> **Eq. (48) can be used to infer the diffusivity in situations where $\overline{v^{2}}$ and $T_{int}$ are known.**

In 3D, the appropriate generalisation is

$$ K_{ij}\approx \int_{0}^{\infty}\overline{v_{i}(t)v_{j}(t+\tau)}d\tau $$

for $t\gg T_{int}$, where the $K_{ij}$ is symmetric and positive definite.


## 11.2.3 Eulerian Diffusion by Small-Scale Turbulence

In this section, we investigate under which conditions we can expect the analogy between molecular and turbulent diffusion as expressed by Eq. 37

**Consider in a 1D situation with a tracer distribution $\overline{\psi}(x)$**, one can view $\overline{\psi}(x)\Delta x$ as the number of particles in an interval $\Delta x$ around $x$ (up to a constant factor).

> [!Attention]
> Turbulent diffusion corresponds to a random transport process. Specifically, assume that a particle which initially (at time $t'$) is located at $x'$ is found at location $x$ at a later time $t$ with a ==**probability**== $\Phi(x, x',t,t')$. Note that **if the turbulence is homogeneous (statistically uniform in space) and stationary**, then the probability depend only on the **separation length** $r=x-x'$ and $\tau=t-t'$, i.e., $\Phi(x-x',t-t')$.

It follows that the tracer distributions at both times are related by:
$$ \begin{equation} \overline{\psi}(x,t)=\int_{-\infty}^{\infty}\Phi(x-x',t-t')\overline{\psi}(x',t')dx' \tag{49}\end{equation} $$
While the **probability distribution $\Phi$ is not known for turbulent flows,** it has to satisfy the following physical constraints:

$$ \begin{equation} \text{mass conservation}: \quad \int_{-\infty}^{\infty}\Phi(x-x',t-t')dx'=1 \tag{50}
\end{equation} $$
$$ \begin{equation} \text{centre of mass}:\quad \int_{-\infty}^{\infty}(x-x')\Phi(x-x',t-t')dx'=0 \tag{51} \end{equation} $$
Mass conservation requires the integration of probability over all $x$ equals to 1. If the turbulent motion is isotropic, the $\Phi(x-x',t-t')=\Phi(x'-x,t'-t)$ and Eq. (51) follows.

Eq. (51) shows that the probability function $\Phi$ is symmetric about $r=0$, i.e., on average, particles do not drift away from their starting point. This also means that there is no movement of the centre of mass related to the random transport process. If the particle excursion $x-x'$ is small compared to the scales of variation of $\overline{\psi}(x)$, then $\overline{\psi}(x',t')$ under the integral in Eq. (49) can be expanded into a Taylor series.

With $r=x-x'$, one obtains:
$$ \begin{equation} \begin{split} \overline{\psi}(x,t)= \overline{\psi}(x,t')\int_{-\infty}^{\infty}\Phi(r,t-t')dr
\frac{\partial \overline{\psi}(x,t')}{\partial x}\int_{-\infty}^{\infty}r\Phi(r,t-t')dr \\ + \frac{\partial^{2} \overline{\psi}(x,t')}{\partial x^{2}}\int_{-\infty}^{\infty}\frac{1}{2}r^{2}\Phi(r,t-t')dr + \int_{-\infty}^{\infty}r\Phi(r,t-t')O(r^{3})dr \end{split} \tag{52}\end{equation} $$
Using the Eq. (50) and (51), the first two integrals in Eq. (52) can be evaluated.

With $\tau=t-t'$, one finds the tracer distribution can be expressed as:
$$ \begin{equation} \overline{\psi}(x,t)=\overline{\psi}(x,t-\tau)+K\tau\frac{\partial^{2}\overline{\psi}(x,t-\tau)}{\partial x^{2}}+O(r^{3}) \tag{53}\end{equation} $$
with the definition
$$ \begin{equation} \int_{-\infty}^{\infty}r^{2}\Phi(r,\tau)dr=2K\tau \tag{54}\end{equation} $$
Here, the $r$ tells how far a particle has moved over a time interval $\tau$, $\Phi(r,\tau$) gives the probability of finding the particle between $r$ and $r+dr$. **Together, the left-hand side gives the mean-square displacement of particles over time** $\tau$

> [!Attention]
> Eq. (54) relation is in analog to the case of **molecular diffusion, as the turbulence acts like a random kicking of particles, just like molecules.** Note that the second moment $\int_{-\infty}^{\infty}r^{2}\Phi(r,\tau)dr$ becomes also equivalent to the Lagrangian mean quadratic particle excursion which is given by Eq. (48). However, this holds only for time intervals $\tau \gg T_{int}$, as discussed in [11.2.2](https://www.notion.so/Ocean-Dynamic-Chapter-11-Small-Scale-Turbulence-28e69691c52b800780bff3677d418d3b?pvs=21).
> 
> Provided that the tracer concentration change is little over the time $\tau \gg T_{int}$ and the distance $r$, then the Eq. (53) can be approximated by
> 
> $$ \begin{equation} \frac{\partial \overline{\psi}}{\partial t}=K\frac{\partial^{2} \overline{\psi}}{\partial x^{2}} \tag{55}\end{equation} $$
> 
> → standard (molecular) diffusion equation, with a down-gradient diffusivity $K=\overline{v^{2}}T_{int}$ ($v$ denotes the Lagrangian velocity).
> 
> **However, Eq. (55) only holds under restrictive conditions:**
> 
> - $|\partial \psi/\partial t|\ll |\psi/\tau| \quad \text{and} \quad |\partial \psi/\partial x|\ll |\psi/r|$: $\psi$ varies little over the temporal and spatial scales of the eddy field such that the Taylor expansion rapidly converges to the first order term


# 11.3 Inhomogeneous Three-Dimensional Turbulence

In the presence of mean flow, stratification, forcing, and boundary conditions, turbulence in the ocean cannot be expected to be homogeneous. The most important aspect of inhomogeneity is given by stratification.

> However, **numerical simulations confirm that many aspects from Kolmogorov’s theory for isotropic turbulence can be transferred to the inhomogeneous case**, in particular:
> - the predicted shape of the spectrum in horizontal direction
> - the forward energy cascade in the inertial subrange can be found for non-isotropic turbulence in the presence of strong stratification

On the other hand, a closed analytical theory like the one for isotropic turbulence is missing. In the following sections, energetic constraints for inhomogeneous turbulence are derived, and applications to the dynamics of the ocean mixed layer are discussed

Starting point are the equations of motion in the Boussinesq approximation in Eq. (4), with the molecular form for the diabatic terms. In the upper ocean, the state equation (4th on in Eq. (4)) can be approximated by the linear form:
$$ \begin{equation} \rho'=\rho_{0}(\gamma_{0}S'-\alpha_{0}\theta') \tag{56}\end{equation} $$
With these changes, and introducing Reynolds decomposition to velocity $u_{j}=\overline{u_{j}}=u_{j}'$ and buoyancy $b=\overline{b}+b'$, the system can be written (in tensor notation):
$$ 
\begin{align}\frac{\partial \overline{u}_{j}}{\partial t} + \overline{u}_{i}\frac{\partial \overline{u}_{j}}{\partial x_{i}} + 2\Omega_{l}\overline{u}_{m}\epsilon_{lmj} &= -\frac{\partial \overline{p}}{\partial x_{j}} + \overline{b}\delta_{j3} - \frac{\partial}{\partial x_{i}}(\overline{u_{i}'u_{j}'}) - \kappa_m\frac{\partial^{2} \overline{u}_{j}}{\partial x_{i}^2} \tag{57}\\ 
\frac{\partial \overline{u}_{i}}{\partial x_{i}} &= 0 \tag{58}\\ 
\frac{\partial \overline{b}}{\partial t} + \overline{u}_{i}\frac{\partial \overline{b}}{\partial x_{i}} &= - \frac{\partial}{\partial x_{i}} \Big( J_{i}^{b} + \overline{u_{i}'b'} - \kappa \frac{\partial \overline{b}}{\partial x_{i}} \Big) \tag{59}
\end{align} 
$$
Where
- **Eq. 57: Momentum Equation**
    - $\overline{u}_{i}\frac{\partial \overline{u}_{j}}{\partial x_{i}}$: Advection of the mean velocity by the mean flow (nonlinear)
    - $2\Omega_{l}\overline{u}_{m}\epsilon_{lmj}$: Coriolis force ($2\Omega \times\mathbf u$); $\Omega_{l}$ is the rotation vector, $\epsilon_{lmj}$ is the *Levi-Civita* symbol for cross product
    - $\overline{b}\delta_{j3}$ : buoyancy force in the vertical direction ($j=3$)
    - $\frac{\partial}{\partial x_{i}}(\overline{u_{i}'u_{j}'})$: Divergence of Reynolds stress tensor, representing the ==momentum transport by turbulence fluctuations==. Additional dissipation term due to turbulence
    - $\kappa_m\frac{\partial^{2} \overline{u}_{j}}{\partial x_{i}^2}$: ==Molecular or eddy viscosity diffusion term==, small-scale dissipation momentum
- **Eq. 58: Continuity equation** (i.e., incompressibility)
    - States that the mean flow is incompressible (conserving volume)
- **Eq. 59: Buoyancy change equation**
    - Represent mean buoyancy changes due to advection, additional fluxes, turbulence, and molecular diffusion (from left-to-right in sequence)
    - $J_{i}^{b}$ is the buoyancy flux arising from radition

In general, this equation set introduce two additional turbulence fluxes $\overline{u_{i}'u_{j}'}$ and $\overline{u_{i}'b'}$. **Their divergence** introduce the additional changes to the mean velocity and buoyancy field.

## 11.3.1 Energetic Constraints

Parameterisation of the turbulent flux terms involves issues similar to the parameterisation of turbulent tracer fluxes discussed in Section 11.2.

However, **for the turbulent momentum and buoyancy fluxes there are additional constraints arising from consideration of the energy budget.**
### Turbulent Kinetic Energy (TKE) #TKE #Theory 
> [!Attention]
> A conservation equation for TKE is obtained by multiplication of momentum equation (pre-phase of Eq. (57)) with $u_{j}'$ and averaging, summation over all $j$. With the turbulent kinetic energy $\varepsilon=\frac{1}{2}\overline{u_{i}'u_{i}'}=\frac{1}{2}(\overline{u'^2}+\overline{v'^2}+\overline{w'^2})$, it follows that:
> $$ \begin{equation} \begin{split} \frac{\partial \varepsilon}{\partial t}+\overline{u}_{i}\frac{\partial \varepsilon}{\partial x_{i}}=-\frac{\partial}{\partial x_{i}} \biggl(\overline{u'_{i}p'}+\frac{1}{2}\overline{u'_{i}u'_{j}u'_{j}}-\kappa_{m}\frac{\partial \varepsilon}{\partial x_{i}}\biggl) \\ -\overline{u'_{j}u'_{i}}\frac{\partial \overline{u}_{j}}{\partial x_{i}}+\overline{b'w'}-\kappa_{m}\overline{\biggl(\frac{\partial u'_{i}}{\partial x_{j}}\biggl)^{2}} \end{split} \tag{60}\end{equation} $$
> The physical interpretation:
> - **Left hand side**: 
>   material derivative of $\varepsilon$ along the mean flow $\overline{\mathbf u}$; Which describes the local change in time plus the advection by the mean flow
> - **Right hand side**:
>     - $-\frac{\partial}{\partial x_{i}} (\overline{u'_{i}p'}+\frac{1}{2}\overline{u'_{i}u'_{j}u'_{j}}-\kappa_{m}\frac{\partial \varepsilon}{\partial x_{i}})$ : **three terms contribute to a flux divergence which redistributes TKE $\varepsilon$**, they are:
>         
>         - $\overline{u'_{i}p'}$: turbulent flux of mechanical energy
>         - $\overline{u'_{i}u'_{j}u'_{j}}$: turbulent flux of turbulent kinetic energy (triple correlation)
>         - $-\kappa_{m}\frac{\partial \varepsilon}{\partial x_{i}}$: diffusive flux
>         
>         It follows that $\mathbf F=\overline{\mathbf u'p'}+\overline{\mathbf u' \mathbf u'^{2}/2}-\kappa_{m} \nabla \varepsilon$ is the total flux of TKE.
>         
>     - $\overline{u'_{j}u'_{i}}\frac{\partial \overline{u}_{j}}{\partial x_{i}}$: **shear** **production term**; describes the interaction of turbulence with a mean shear (see details in below section “_Exchange with Mean Kinetic Energy_”
>         
>     - $\overline{b'w'}$: **buoyancy production term**; describe the exchanges of energy between the mean potential energy and turbulent kinetic energy
>         
>     - $-\kappa_{m}\overline{(\frac{\partial u'_{i}}{\partial x_{j}})^{2}}$: **viscous dissipation term**; always negative
>         

Note that for isotropic turbulence, i. e. when all mean fields are independent of space, Eq. 60 reduces to the form Eq. 19

> [!Tip]
> **Differences between the (1) molecular diffusion and (2) viscous dissipation terms?**
> 1. **Molecular diffusion**: conduction of energy from high to low regions (spatial redistribution) → $\kappa_m$ multiply spatial gradient
> 2. **Viscous dissipation**: actual conversion of kinetic energy into thermal energy by viscosity (or diffusivity of momentum!) → $\kappa_m$ multiply

### Exchange with Mean kinetic Energy
The shear production term $\overline{u'_{j}u'_{i}}\frac{\partial \overline{u}_{j}}{\partial x_{i}}$ describes the interaction of turbulence with a mean shear. Further insight into its meaning can be gained by considering the conservation equation for mean kinetic energy (MKE) #MKE which is obtained by multiplication of (57) with $\overline{u}_{j}$. With the mean kinetic energy $\frac{1}{2}\overline{u_i}\overline{u_i}=\frac{1}{2} \left( \overline{u}^2 + \overline{v}^2 + \overline{w}^2 \right)$ one finds
$$ \begin{equation} \begin{split} \bigg(\frac{\partial }{\partial t}+\overline{u}_{i}\frac{\partial }{\partial x_{i}}\bigg)\frac{1}{2}\overline{u}_{i}\overline{u}_{i}=-\frac{\partial}{\partial x_{i}} \biggl(\overline{u}_{i}\overline{p}+\frac{1}{2}\overline{u}_{j}\overline{u'_{i}u'_{j}}-\kappa_{m}\frac{\partial}{\partial x_{i}}\frac{1}{2}\overline{u}_{i}\overline{u}_{i} \biggl) \\ +\overline{u'_{i}u'_{j}}\frac{\partial \overline{u}_{j}}{\partial x_{i}}+\overline{b}\overline{w}-\kappa_{m}\biggl(\frac{\partial \overline{u}_{j}}{\partial x_{i}}\biggl)^{2} \end{split} \tag{61}\end{equation} $$
All terms in (61) are analogous to those in (60).
> [!Attention]
> **The term $\overline{u'_{i}u'_{j}}\frac{\partial \overline{u}_{j}}{\partial x_{i}}$ appears in both (60) and (61), but with opposite sign, and hence constitutes an exchange between mean and turbulent kinetic energies.** The direction of this exchange is normally such that energy flows from mean to turbulent kinetic energy. This is consistent with a parameterisation $\overline{u_{i}'u_{j}'}=-K_{u}\partial \overline{u}_{j}/\partial x_{i}$ which for $K_{u}>0$ leads to
> 
> $$ \overline{u'_{i}u'_{j}}\frac{\partial \overline{u}_{j}}{\partial x_{i}}=-K_{u}\bigg( \frac{\partial \overline{u}_{j}}{\partial x_{i}}\bigg)^{2}<0 $$
> 
> As $\overline{u'_{i}u'_{j}}$ and $\frac{\partial\overline{u}_{j}}{\partial x_{i}}$ are always negative correlated

For small-amplitude fluctuations, this energy transfer is related to the process of Kelvin-Helmholtz instability

### Exchange with Mean Potential Energy
A term of the form $bw=-g\frac{\rho w}{\rho_{0}}$ describes work against gravity and can be interpreted as exchange between kinetic and potential energy. In Boussinesq approximation, a useful potential energy is given by
$$ 
v=\frac{gz\rho}{\rho_0}=-zb
$$
With a simplified equation of state Eq. 56, the conservation equation for mean potential energy $\overline{\upsilon}$ follows as:
$$ \begin{equation} \frac{D\overline{\upsilon}}{Dt}=-\overline{b}\overline{w}-\overline{b'w'}-\frac{\partial \overline{u'_i\upsilon'}}{\partial x_{j}}+z\bigg(\frac{\partial J_{i}^{b}}{\partial x_{i}} - \kappa \frac{\partial \overline{b}}{\partial x_{i}} \bigg) \tag{62}\end{equation} $$

> [!Attention]
> The vertical buoyancy fluxes $\overline{b'w'}$ also **occur in both the turbulent and mean kinetic energy equation (60) and (61)**, respectively, and hence **constitute exchanges of mean the turbulent kinetic energy with mean potential energy.**

> [!Tip]
> **If in a stably stratified environment**
> - - $\partial \overline{b}/\partial z>0$ → $\overline{b'w'}<0$ → energy will transfer from TKE to potential energy → $\partial \varepsilon/\partial t<0$
> - - Which means, when turbulent velocity moves a particle upward and downward from its mean position, buoyancy forces will tend to bring the particle back to its mean position under stably stratified condition → _**restoring**_ force
> - - Hence the particle will decelerate, reducing its TKE
> 
>**If in a unstable stratification**
> - - Particle will be further accelerated
> - - the energy transfer: from potential energy → TKE </aside>

![[Screenshot 2025-10-19 at 22.23.17 1.png | center| 512]]^Schematic of the mixing-length concept in which a sheared horizontal mean flow is subject to turbulent vertical exchange of momentum.

In both cases:
$$ \overline{b'w'}=-K_b\frac{\partial \overline{b}}{\partial z} $$
with $K_b>0$ leads to the correct sign for energy transfer.


## 11.3.2 Turbulence Models for the Surface Boundary Layer

### General TKE equation in mixed layer #TKE_MLD
In the **mixed layer**, consider:
- horizontal scales are much larger than the vertical scales.
- The approximation of **horizontal homogeneity** neglects horizontal gradients of all mean quantities.
- molecular TKE flux is small, neglected

**From the mean continuity equation (58), it then follows that $\partial \overline{w}/\partial z=0$ and hence also $\overline{w}=0$ (as $\overline{w}(z=0)=0$ at ocean surface)** and therefore, in this approximation, the advection of mean fields though the mean flow is neglected. Accordingly, the mean field equations (57) and (59) simplify to:
$$ \begin{align} \frac{\partial \overline{\mathbf u}_h}{\partial t}-\mathbf f\times \overline{\mathbf u}_h&=-\frac{\partial}{\partial z}\overline{\mathbf u'_hw'} \tag{63}\\ 
\frac{\partial \overline{b}}{\partial t}&=-\frac{\partial}{\partial z}\bigg(\overline{J^{b}_{3}}+\overline{b'w'}\bigg) \tag{64}\end{align} $$
Closure for $\overline{b'w'}$ and $\overline{\mathbf u'_hw'}$ are needed.

> [!Attention]
> The **TKE equation (60), with horizontal homogeneity in mixed layer**, becomes:
> 
> $$ \begin{equation} \frac{\partial \varepsilon}{\partial t}=\frac{\partial F}{\partial z}-\overline{\mathbf u'_hw}\cdot\frac{\partial \overline{\mathbf u}_h}{\partial z}+\overline{b'w'}-\epsilon \tag{65}\end{equation} $$
> 
> Here on the right-hand side
> - **1st term**: the gradient of vertical transport (flux) $F=\overline{w'(p'+u_{j}'u_{j}'/2})$ (defined as positive into the ocean). Only **redistribute the TKE**. The molecular TKE flux is neglected since it is very small in the mixed layer. Remaining:
>     - pressure transport (flux) $\overline{w'p'}$ and turbulent transport (flux) $\frac{1}{2}\overline{w'u_j'u_j'}$
> - **2nd term**: the $-\overline{\mathbf u'_hw}\cdot\frac{\partial \overline{\mathbf u}_h}{\partial z}$ is the (vertical) **shear production term**, denotes the interaction of turbulence with a mean shear
> - **3rd term**: the **buoyancy production term**; The exchanges of energy between the mean potential energy and turbulent kinetic energy
> - **4th term**: the **viscous dissipation term**
> 
> The vertical transport term $F$ and the dissipation $\epsilon$ are two more variables for which closures are needed

Several approaches for closing the system will be briefly discussed.
Notice for the below sections, only key information for these mixed-layer turbulence model will be collected and noted.

### Bulk Models
Consider:
- the buoyancy has a constant value within a **well-mixed layer of depth $h$,** so that the buoyancy profile is given by $b=b_{0}(t)$ for $0 \ge z \ge -h(t)$
- the buoyancy profile between $z=-h(t)$ and $z=-h_*$ is given by a linear function of depth from $b_0$ to $b_*$, representing a thin pycnocline (a layer where the density increase sharply with depth). The deep buoyancy $b_*$ has to be specified.
- The parameter $\Delta=h_* -h$ characterises the pycnocline thickness and is assumed to be constant and small $\Delta \ll h$So that effectively the pycnocline is modelled as a discontinuity at the mixed layer base
![[Screenshot 2025-10-22 at 13.51.35 1.png | center]]*Sketch of profiles of buoyancy (blue line) and vertical (radiative + turbulent) buoyancy flux (red line) in bulk mixed layer models.*

The physics of the mixed layer model should determine both $b_0(t)$ and $h(t)$ from the specified surface buoyancy flux and the characteristics of the turbulence in that layer.
**It is useful to define $Q=-\overline{(J_3^b+b'w')}$ as the vertical (*radiative (from Eq. 59)* + turbulent) buoyancy flux** (positive downwards into the ocean). As evident from (64), for $b$ to be constant in the mixed layer, $Q(z)$ must be a linear function of depth:
$$ \begin{equation} Q(z)=Q_0+\frac{z}{h}(Q_0-Q_h) \tag{66}\end{equation} $$
Here:

- $Q_0$ is the net surface buoyancy flux which is considered as a **prescribed** forcing
- $Q_h$ is the $Q(z=-h)$ the **flux at the mixed layer base**. The radiative component of the $Q_h$ can normally be neglected, except in certain situations with very shallow mixed layers

Integration of (64) from $z=-h$ to $z=0$ and from $z=-h_*$ to $z=-h$, respectively leads to:
$$ \begin{equation} h\frac{\partial b_0}{\partial t}=Q_0-Q_h \quad and\quad (b_0-b_*)\frac{\partial h}{\partial t}=Q_h-Q_* \tag{67}\end{equation} $$

Usually, $Q_*=Q(z=-h_*)$ is small and neglected though it could easily be retained e.g., as a diffusive flux below the mixed layer.

Since $b_0-b_*>0$ in a stable stratification, the mixed layer will deepen if the buoyancy flux $Q_h$ is positive (i.e., downward). In this case, fluid of buoyancy $b_*$ from below the mixed layer is mixed (or ‘entrained’) into the mixed layer and brought to $b_0$. **This mixing consumes turbulent energy** and hence can only take place if sufficient TKE is available for mixing fluid from below into the mixed layer

> [!Tip]
> The **basic assumption of mixed layer physics is that $Q_h$ originates from turbulent processes within the mixed layer** and, therefore, cannot become negative since there is no ‘un-mixing’ of fluid from the mixed layer. Instead, when $Q_h$ drops to zero, there is insufficient turbulent energy to continue mixing at the mixed layer base. Mixing and deepening of the mixed layer then stops, and **new mixed layer is established at a shallower depth**

Thus the mixed layer depth is determined from the budget of TKE, which usually equilibrates within a few minutes.

Ignoring the shear production term (for simplicity) in (65), integration from $-h$ to 0 with (66) then yields
$$ \begin{equation} 0=F_0-F_h-\frac{h}{2}(Q_0+Q_h)-\int_{-h}^{0}\epsilon dz \tag{68}\end{equation} $$
**A few further assumptions (parameterisations) close the problem.**
- The turbulent energy input $F_0$ is related to the wind stress $\tau_0$ (by exciting surface waves, which by breaking create TKE). From dimensional arguments, one assumes $F_0=c|\tau_0|^{3/2}$ with a dimensionless coefficient $c=O(1)$, while $F_h$ is assumed small and neglected.
- To close the positive dissipation term, simply assume that a fraction $r_1$ of the (positive) wind input $F_0$ is dissipated.
- In situations when buoyancy is lost at the surface, i. e. when $Q_0<0$, which occurs mainly by cooling, the potential energy of the heavier fluid is converted to TKE, and it is likewise assumed that a fraction $r_2$ of this TKE gain is dissipated. Hence the dissipation term is expressed as
    $$ \int_{-h}^{0}\epsilon dz=r_1F_0+r_2\frac{h}{4}(|Q_0|-Q_0) $$
    with $0<r_1,r_2<1$.

This relation, together with (68), finally leads to the required parameterisation of $Q_h$in the form
$$ \begin{equation} \frac{h}{2}Q_h=(1-r_1)c|\tau_0|^{3/2}-\frac{h}{2}\bigg[(1-\frac{r_1}{r_2})Q_0+\frac{r_2}{2}|Q_0|\bigg] \tag{69}\end{equation} $$

With **specified (prescribed)** forcing terms $\tau_0$ and $Q_0$, the recipe to calculate the mixed layer depth $h(t)$ is now as follows:
- If $Q_h>0$ in (69), it is used to compute $h(t)$ from the _**prognostic**_ equation (67) → deepening of mix layer as downward buoyancy flux
- If $Q_h\le0$ in (69), which can only happen when $Q_0>0$, $h$ is determined from the _**diagnostic**_ equation (69) as balance between mechanical forcing and buoyancy gain by setting $Q_h=0$ (see below) → insufficient turbulence energy, determine mixed layer depth by prescribed net surface buoyancy flux
$$ \begin{equation} h=1(1-r_1)c|\tau_0|^{3/2}/Q_0 \tag{70}\end{equation} $$

The algorithm reflects the fundamental difference between the processes of deepening and shallowing of the mixed layer. Two adjustable parameters $(1-r_1)c$ and $r_2$. Typical parameter values are $r_1 \approx0.1$, $r_2\approx0.9$ and $c\approx 1$.

> [!Tip]
> Bulk mixed layer models:
> - **pros**: 
>   conceptual and computational simplicity; Particularly the wind-induced deepening of the mixed layer is usually well described.
> - **cons**:
> 	- performs less well in convective situations;
> 	- are unable to resolve any structures within the boundary layer;
> 	- ‘discontinuity’ at the mixed layer base, which is not generally observed


### Models based on Parameterisation of Vertical Buoyancy and Momentum Fluxes
The standard closure for the turbulent buoyancy and momentum fluxes in (63) and (64) is the down-gradient parameterisation
$$ \begin{equation} \overline{w'\psi'}=-K_\psi\frac{\partial \overline\psi}{\partial z} \tag{71}\end{equation} $$

Where $\psi$ stands for $\mathbf u$ and $b$, and coefficient $K_\psi$ can be different for buoyancy and momentum.

If the $K_\psi$ is constant or slowly varying, (64) will completely fail to reproduce the observed property structure in the mixed layer.

While Pacanowski and Philander have proposed functional relations to express $K_\psi$ as function of local Richardson number
$$ Ri=\frac{\text{buoyant production of turbulence}}{\text{production of turbulence}} $$

Thus there are $K_u(Ri)$ and $K_b(Ri)$, which permits variation of the coefficients over two or event three order of magnitude, with the highest values obtained for $Ri\rightarrow0$.

> [!Tip]
> The parameterisation works well in regions of strong shear, such as in the tropical oceans. However:
> - - **in a situation where the buoyancy is vertically well mixed so that everywhere in the mixed layer $Ri=0$, these methods are less suited** since effectively they imply constant coefficients.
> - - fails in **situations where an unstable stratification induces convective transport** independent of the local gradient. </aside>

The generalisation:
$$ \begin{equation} \overline{w'\psi'}=-K_\psi\bigg(\frac{\partial \overline\psi}{\partial z}-\gamma_\psi\bigg) \tag{72}\end{equation} $$
includes a nonlocal contribution which is modelled as an empirical function of stratification and forcing, applies only in the convective situations.

In the **KPP (K-profile parameterisation)** formulation of ocean mixed layer turbulence, (72) is used together with a specification of mixed layer depth $h$ by a bulk Richardson number criterion and with specified profiles of both the coefficients $K_u$; $K_b$ and the nonlocal parameters $\gamma_u$, $\gamma_b$ based on empirical results

### Models based on the TKE Equation #TKE_scheme 
TKE models are based on the parameterisation (71) and link the diffusivity with TKE and a characteristic length scale with the heuristic relation (39) as:
$$ \begin{equation} K_\psi=c_\psi\varepsilon^{1/2}L \tag{73}\end{equation} $$

Here $c_\psi$ is an empirical dimensionless coefficient (or “stability function”), either chosen as constant or as function of other variables such as stability, normally with $c_\psi=O(1)$. The TKE $\varepsilon$ can in principle be determined from the TKE equation [(60)](https://www.notion.so/Ocean-Dynamic-Chapter-11-Small-Scale-Turbulence-28e69691c52b800780bff3677d418d3b?pvs=21). The dissipation is expressed as $\epsilon=c_\epsilon \varepsilon^{3/2}/L$ according to [(23)](https://www.notion.so/Ocean-Dynamic-Chapter-11-Small-Scale-Turbulence-28e69691c52b800780bff3677d418d3b?pvs=21)., again with $c_\epsilon=O(1)$.

Now consider the vertical transport (flux) of TKE $\overline{w'\varepsilon'}=-K_E(\partial \overline{\varepsilon}/\partial z)\approx-c_E\varepsilon^{1/2}L(\partial \overline{\varepsilon}/\partial z)$ ==**The vertical transport term in TKE equation (60) (i.e., the combination of all three fluxes) are modelled as a diffusive flux of TKE**==, i.e.,
$$ \begin{equation} F=c_E\varepsilon^{1/2}L\frac{\partial \varepsilon}{\partial z} \tag{74}\end{equation} $$
With $c_E=O(1)$ but not that the flux of TKE can take complicated forms in particular for convective situations.

> [!Attention]
> With (71), (73),(74) and (23), the **TKE equation (65) (in mixed layer) can be written as:**
> $$ \begin{equation} \frac{\partial \varepsilon}{\partial t}=\frac{\partial}{\partial z}\bigg( c_E\varepsilon^{1/2}L\frac{\partial \varepsilon}{\partial z}\bigg)+ c_u\varepsilon^{1/2}L\bigg(\frac{\partial \overline{\mathbf u}}{\partial z} \bigg)^2-c_b\varepsilon^{1/2}L\frac{\partial \overline{b}}{\partial z}-c_\epsilon\frac{\varepsilon^{3/2}}{L} \tag{75}\end{equation} $$

For a qualitative discussion of (75), the dimensionless coefficient will be ignored (i.e. $c_E,c_u,c_b,c_\epsilon \rightarrow 1$). Note, however, that modifications may occur in practical applications, due to different choices for the dimensionless coefficients.

As discussed earlier in (24), the time-scale of adjustment of TKE to changes in the forcing $T=L/\sqrt{\varepsilon}$, which typically is much shorter than 1h. **On longer time-scales, the energy equation, therefore, is effectively in equilibrium so that the terms on the right-hand side of (75) must balance**. Since dissipation is a central aspect of turbulence, any term balances not involving dissipation are not very plausible.

Note that a principal difficulty remains, namely the parameterisation of the length scale $L$, which often can be interpreted as scale of the large eddies (see Section 11.2.1). Several plausible limits for $L$ can, however, be derived from considerations of stratification, shear, and geometrical factors:

**On long time-scales → $\partial \varepsilon /\partial t=0$ (local steady state)**
- ==**Stable mean stratification== ($\partial \overline{b}/\partial z=N^2>0$):** Consider a particle this is vertically displaced from its mean position by a turbulent eddy with the length scale $L$. In this way, the particle gains potential energy (per mass) by $E_p=L^2N^2/2$. The potential energy which is gained cannot be larger than the eddy TKE $E_p\le\varepsilon$. It follows:
    $$ \begin{equation} L\lesssim L_b \approx\sqrt{2\varepsilon/N^2} \tag{76}\end{equation} $$
- ==**Unstable mean stratification:**== the buoyancy term in (75) is positive, and TKE is produced. However, the convective TKE production cannot be larger than dissipation term. i.e., $\varepsilon^{1/2}L|\partial \overline{b}/\partial z|\lesssim\varepsilon^{3/2}L$, if one assumes for simplicity that the first two terms on the right-hand side of (75) are also positive (note that the flux of TKE takes complicated form in convective situations). It follows for that case that:

$$ \begin{equation} L\lesssim L_b^* \approx \sqrt{\varepsilon/|\partial \overline{b}/\partial z|} \tag{77}\end{equation} $$
- ==**Presence of mean shear:**== Consider variations of the mean flow $\overline{\mathbf u}$ over the eddy length scale $L$. For the concept of the parameterisation (71) with (73) to be valid, $\overline{\mathbf u}$ should not change over the eddy length scale by more than $u_{rms}=\varepsilon^{1/2}$, i.e., $|\overline{\mathbf u}(z+L)-\overline{\mathbf u}(z)|\approx|L\partial \overline{\mathbf u}/\partial z|\lesssim \varepsilon^{1/2}$, and hence
    $$ \begin{equation} L\lesssim L_u \approx \varepsilon^{1/2}/|\partial \overline{\mathbf u}/\partial z| \tag{78}\end{equation} $$
    another interpretation of the condition (78) is that the shear term in (75) cannot be much larger than the dissipation term (same, $|L\partial \overline{\mathbf u}/\partial z|\lesssim \varepsilon^{1/2}$). Note that for $L=L_u$, with (73) it follows that $K=|\partial \overline{\mathbf u}/\partial z|L^2$, a relation which was derived in Section 11.2.1 based on the _mixing length concept_. The $L$ here is the time-mean mixing length.
    
- ==**Absence of stratification and mean shear:**== Observations suggest that turbulent mixing rarely extends deeper than $L_{max}\approx 50-100m$. This situation is described by a balance between the diffusion of energy and dissipation in (75), which is only valid as local steady state, otherwise the vertical gradient of TKE flux only **redistributes** the TKE i.e.,
    $$
    
    \frac{\partial}{\partial z}\bigg(\varepsilon^{1/2}L\frac{\partial\varepsilon}{\partial z} \bigg)=\frac{\varepsilon^{3/2}}{L} 
    
    $$
    Stands for the “the downward diffusive flux of TKE equals the dissipation rate” Which can be solved analytically with the approximation of constant $L$. The solution is given as $\varepsilon=\varepsilon_0e^{z/d}$ with $=\sqrt{3/2}L\sim L$. Hence the TKE decreases exponentially with z in the oceanic mixed layer, which is to be expected since only dissipation is acting in the interior. Since $L$ is not very different from the mixed layer depth scale $d$ in this case, one finds the further condition $L\lesssim L_{max}$ Note that the magnitude of TKE is determined by the surface flux $F_0$ of TKE into the ocean which appeared in (68), as $\varepsilon_0=((3/2)F_0)^{2/3}$.
    
- ==**For geometrical reasons, eddies cannot be larger than their distance from a boundary:**== In particular, near the surface $z=0$ it follows that
    $$ L \lesssim L_r=|z| $$

In summary, from the preceding considerations it follows that:

$$ L \lesssim L_{min}=\text{min}(L_b, L_b^*, L_{max}, L_r) $$

A very simple choice would be to assume $L=L_{min}$. Other models have used $L\approx L_b$ or an integral variant thereof as a closure for $L$.

> [!Attention]
> #TKE_scheme #TKE_scheme_cons
> Turbulence models based on the TKE equation (75) and a diagnostic specification of $L$ succeed in general in simulating the large variations in diffusivity/ viscosity required for a simulation of a well-mixed surface layer. 
> _**However**_, the parameterisation of the combined TKE flux and pressure correlation as TKE diffusion in TKE redistribution term $F=c_E\varepsilon^{1/2}L\frac{\partial \varepsilon}{\partial z}$ , which is central for TKE models, is not supported by numerical simulations of boundary layer turbulence; 
> → **it appears instead that buoyancy effects** (comes from the buoyancy-induced pressure fluctuations that appears in $\overline{w'p'}$ term in $F$) **dominate that flux**

> [!Tip]
> **The TKE-scheme in ICON:** #TKE_scheme #ICON_parameterisation 
> Using the diagnostic length scale to be:
> $$
> \begin{equation}
> L_{mix}=\sqrt{\frac{2 \varepsilon}{N^2}}
> \end{equation}
> $$
> Which is using $L=L_b$ in Eq. 76

### K-$\epsilon$ and Second order Turbulence Models #kepsilon_scheme
To avoid the diagnostic specification of the length scale $L$, prognostic equations have been used for the dissipation $\epsilon$. In (57), an equation for $\epsilon=\kappa_m \overline{(\partial u_i/\partial x_j)(\partial u_i/\partial x_j)}$ can be derived in a straightforward way. The terms in the resulting equation somewhat resemble those in the energy equation (60), but are of higher order of differentiation and have a less direct physical interpretation. These terms have been modelled in analogy to the TKE equation, and the resulting equation is often of the form
$$ \begin{equation} \frac{\partial \epsilon }{\partial t}=\frac{\partial}{\partial z}c_4\varepsilon^{1/2}L\frac{\partial \epsilon}{\partial z}+\frac{\epsilon}{\varepsilon}\bigg[c_1\varepsilon^{1/2}L(\frac{\partial \overline{\mathbf u}}{\partial z})^2-c_3\varepsilon^{1/2}L]\frac{\partial \overline{b}}{\partial z}-c_2\frac{\varepsilon^{3/2}}{L}\bigg] \tag{79}\end{equation} $$
Which can in principle be solved together with the TKE equation (75) by expressing the length scale $L$ through (23). Here $c_1,...,c_4$ denote additional dimensionless stability functions which need to be specified.

It can be shown that:
- $c_2$ determines the free turbulence decay time scale
- $c_3$ determines the steady-state Richardson number and mixing efficient for homogeneous shear layers

Using (79) together with the TKE equation is often referred to as $k-\epsilon$-model (k stands for TKE)

Another possibility for the construction of a turbulence model is to consider equations for all second order moments. These equations can be obtained in a straightforward way, by multiplication of (57) and (59) with $u_i'$ and respectively $b'$ adding and averaging. With further assumptions (assuming that the turbulent length scale is smaller than a mean length scale and that isotropy prevails at high wave numbers, neglecting the radiative component in (59) and assuming horizontal homogeneity), the equation set is given by

![[Screenshot 2025-10-24 at 10.08.20.png | center]]

where $\epsilon=\kappa\overline{(\partial b/\partial x_i)^2}$. Note that the summation of 11.82 over the diagonal terms i=j yields the TKE equation (65).

A main advantage of 11.82-84 is that parameterisation of the (71) type are no longer needed since the fluxes are now modelled explicitly. However, parameterisations are not needed for the pressure correlation terms $\overline{p'\partial b'/\partial x_i}$ and $\overline{p'(\partial u_j'/\partial x_i+\partial u_i'/\partial x_j)}$ and, as before, for the dissipation and the moments of third order. Equations (11.82)–(11.84) are often used in their equilibrium version (equating right hand sides to zero).

Both $k-\epsilon$ and second order turbulence models are frequently used in practical engineering and have also successfully been applied to the ocean boundary layer modelling.