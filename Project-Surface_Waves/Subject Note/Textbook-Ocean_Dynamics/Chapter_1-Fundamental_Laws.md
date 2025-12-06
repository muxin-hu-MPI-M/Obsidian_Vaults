---
tags:
  - project/surfwaves
  - Theory
  - Subject-Note
  - "#Ocean_Dynamics"
Last Eddited: 2025-11-29
Type:
---

# 1.1 Flow Kinematics

## 1.1.1 Lagrangian & Eulerian Representation

### Introduction
- **Lagrangian**: the motion of fluid is described by following the trajectory (i.e., the location of each single fluid element as a function of time)
- **Eulerian**: describe the fluid properties at a fixed position and their changes in the course of time
    - consider how the velocity and other fluid properties change at each position and over time

### Two perspectives and Connections
Lagrangian velocity: Gives the velocity of a specific fluid parcel, which moves along a **trajectory $X(t)$**:
$$ \begin{equation} u_{L}(t)= \frac{dX(t)}{dt} \end{equation} $$
Eulerian velocity field: Gives the velocity of a fluid at a fixed **spatial position $\mathbf x$** ( $\mathbf x=(x,y,z)=(x_{1}, x_{2}, x_{3})$)and time $t$
$$ u_{E}(\mathbf x,t)

$$
The **Lagrangian velocity at any time** is obtained by evaluating the **Eulerian velocity field along the parcel’s trajectory**:
$$ \begin{equation} u_{L}(t)=\frac{dX(t)}{dt}=u_{E}(X(t),t) \end{equation} $$
where the parcel’s path (i.e., Lagrangian trajectory) $X(t)$ is the integration of Eulerian velocity in time:
$$ \begin{equation} X(t)=X_{0}+\int_{t_{0}}^{t} u_{E}(X(t^{'}), t^{'}) \ dt^{'} \end{equation} $$
where the $X_{0}$ is the initial point of parcel at time $t_{0}$

**In property field**: **for any scalar property $\phi(\mathbf x,t)$ at a fixed position (i.e., Eulerian field)**, the rate of change of some property is a partial derivative
$$ \frac{\partial \phi(\mathbf x,t)}{\partial t} $$
While the rate of change following a parcel (the **material derivative, Lagrangian view**) is the total derivative, because the parcel’s position $X(t)$ depends on time:
$$

\frac{D\phi(\mathbf x,t)}{Dt}

$$
They are connected through the **Euler’s relation**:
$$ \begin{equation} \frac{D\phi}{Dt}=\frac{\partial \phi}{\partial t}+u_{E}\cdot \nabla\phi \end{equation} $$
- at the left hand side:
	- Change following parcel
- at right hand side:
	- **Local change term:** Local rate of change
	- **Advection term**: “advection of fluid through the point of observation” → how much change the parcel experiences because it moves into regions where $\phi$ has spatial gradients → Advection: transport of a property by the flow

> [!Tip]
> **In the field of fluid dynamics**
> 
> - **If the material rate of change vanishes for a specific property for all times, this property is conservative**; While the property observed at any fixed position (Eulerian) may change in the course of time
> - **The Eulerian formulation is generally more convenient.** Especially when the dynamical evolution equations are written completely in Lagrangian coordinates (i.e., position is described in x, y, z → Cartesian coordinate)

## 1.1.2 Deformation and Rotation

### Velocity field
The velocity field (vector) is now described in another form,
$$

u_{E}=\mathbf u(\mathbf x,t) $$
It completely quantifies the macroscopic flow of the fluid. At any fixed time, **the velocity vector define a family of streamlines which are everywhere tangential to the velocity $\mathbf u(\mathbf x,t)$.**
- If the flow is steady, streamlines that taken at a particular time, and parcel trajectory are congruent
- **These families of lines are used to present a global view of the fluid motion**

When zooming in locally, a fluid parcel at position $x$ is translated by the motion an infinitesimal distance:
$$

\mathbf u(\mathbf x,t)\delta t

$$
But it can be displaced in a slightly different way. The distortion of a pattern of infinitesimal neighbouring parcels is contained in the gradient $\nabla \mathbf u$ of the velocity field.

### Deformation and Rotation tensor
> [!Attention]
> The velocity gradient can be decomposed into a symmetric and an anti-symmetric part:
> $$ \begin{equation} \nabla \mathbf u=\frac{\partial u_{i}}{\partial x_{j}}=\frac{1}{2}(\frac{\partial u_{i}}{\partial x_{j}}+\frac{\partial u_{j}}{\partial x_{i}})+\frac{1}{2}(\frac{\partial u_{i}}{\partial x_{j}}-\frac{\partial u_{j}}{\partial x_{i}})=D_{ij}+R_{ij} \end{equation} $$
where $\mathbf u=(u,v,w)=(u_{1}, u_{2}, u_{3})$; $\mathbf x=(x,y,z)=(x_{1}, x_{2}, x_{3})$

In a more general form, the deformation and rotation tensor can be described as:
$$ D_{ij} =\frac{1}{2}(\frac{\partial u_{i}}{\partial x_{j}}+\frac{\partial u_{j}}{\partial x_{i}})=\frac{1}{2}(\nabla \mathbf u + (\nabla \mathbf u)^{T})=D_{ji}\quad \text{symmetric part} $$
$$ R_{ij}=\frac{1}{2}(\frac{\partial u_{i}}{\partial x_{j}}-\frac{\partial u_{j}}{\partial x_{i}})=\frac{1}{2}(\nabla \mathbf u - (\nabla \mathbf u)^{T})=-R_{ji}\quad \text{asymmetric part} $$
Where the T (transpose) is simply row → column. Given the gradient of velocity field:
$$ \nabla \mathbf u=\begin{bmatrix} \frac{\partial u_1}{\partial x_1} & \frac{\partial u_1}{\partial x_2} & \frac{\partial u_1}{\partial x_3} \\[6pt] \frac{\partial u_2}{\partial x_1} & \frac{\partial u_2}{\partial x_2} & \frac{\partial u_2}{\partial x_3} \\[6pt] \frac{\partial u_3}{\partial x_1} & \frac{\partial u_3}{\partial x_2} & \frac{\partial u_3}{\partial x_3} \end{bmatrix}= \begin{bmatrix}\frac{\partial u}{\partial x} & \frac{\partial u}{\partial y} & \frac{\partial u}{\partial z} \\[6pt] \frac{\partial v}{\partial x} & \frac{\partial v}{\partial y} & \frac{\partial v}{\partial z} \\[6pt] \frac{\partial w}{\partial x} & \frac{\partial w}{\partial y} & \frac{\partial w}{\partial z} \end{bmatrix} $$

In a more intuitively way, the deformation and rotation tensor could be described as:
- For x-y shear rate (or x1-x2 shear rate):
$$ D_{12}=D{xy}=\frac{1}{2}(\frac{\partial u_{1}}{\partial x_{2}} + \frac{\partial u_{2}}{\partial x_{1}}) = \frac{1}{2}(\frac{\partial u}{\partial y} + \frac{\partial v}{\partial x}) $$
- For x-y rotation rate (or x1-x2 rotation rate):
$$ R_{12}=R{xy}=\frac{1}{2}(\frac{\partial u_{1}}{\partial x_{2}} - \frac{\partial u_{2}}{\partial x_{1}}) = \frac{1}{2}(\frac{\partial u}{\partial y} - \frac{\partial v}{\partial x}) $$

### Distance vector
The distance vector $l(t)$ is the position separation between two parcels:

$$ \begin{equation} l(t+\delta t)=l(t)+\delta \mathbf u\delta t \end{equation} $$
where the change in velocity $\delta \mathbf u$ is:
$$ \begin{equation} \begin{split} \delta \mathbf u &= \delta \mathbf u(\mathbf x_{0},t) \\ &=\mathbf u(\mathbf x_{0}+l(t),t)-\mathbf u(\mathbf x_{0},t) \end{split} \end{equation} $$
When the displacement (or distance $l$) is small, we can use **Taylor expansion to the $\mathbf u(\mathbf x_{0}+l(t),t)$** term in Eq. (7):
$$ \begin{equation} \begin{split} \delta \mathbf u &= \mathbf u(\mathbf x_{0},t)+(l(t)\cdot \nabla)\mathbf u(\mathbf x_{0},t)+O(l^{2})-\mathbf u(\mathbf x_{0},t) \\ &= l(t)\cdot \nabla\mathbf u(\mathbf x_{0}, t) + O(l^{2})
\end{split} \end{equation} $$
Hence, the distance vector $l(t)$ is governed by:
$$ \begin{equation} \frac{dl}{dt}=\frac{Dl}{Dt}=l(t)\cdot \nabla \mathbf u=(D+R)\cdot l \end{equation} $$
The later expression is according to the Euler’s relation, where the local change of distance vector is zero;

**The distance vector is affected by both tensor contributions to the velocity gradient**


### Deformation
**The evolution of distance vector $\sqrt{l^{2}}$ between the parcels is governed by the deformation tensor $D$ only:**
$$ \begin{equation} \frac{Dl^{2}}{dt}=2l\cdot (D\cdot l+R\cdot l)=2l\cdot D\cdot l \end{equation} $$
This second term related to Rotation tensor $R$ vanishes.

**Mathematically**, according to the matrix-vector multiplication:
$$ \begin{equation} \begin{split} l\cdot (R\cdot l) &= l\cdot (Rl) \\ &= l_{i}R_{ij}l_{j} \\ &= l_{j}R_{ji}l_{i} \\ &= -l_{j}R_{ij}l_{i} \\ &= 0 \end{split} \end{equation} $$
This is because when swap dummy indices, we have the $R_{ij}=-R_{ji}$, and then:
$$ l_{i}R_{ij}l_{j}=-l_{i}R_{ij}l_{j}=0 $$

**Physically**, the rotation velocity vector points always perpendicular to both position vector and angular velocity vector, hence the rotation velocity never along the position vector. Therefore, rotation does not change the length, only the symmetric deformation tensor $D$ (stretching/shearing) can change distance

Hence, the rate of change of the volume $V=(a\times b)\cdot c$ of a infinitesimal material volume element spanned by three vectors $a,b,c$ is given by
$$ \begin{equation} \begin{split} \frac{DV}{Dt} &= (\frac{Da}{Dt}\times b)\cdot c+(a\times \frac{Db}{Dt})\cdot c+(a\times b)\cdot \frac{Dc}{Dt} \\ &=((\nabla u)a\times b)\cdot c+ (a\times (\nabla u)b)\cdot c +(a\times b)\cdot (\nabla u)c \\ &= (D\cdot a\times b)\cdot c + (a\times D\cdot b)\cdot c+ (a\times b)\cdot D \cdot c \end{split} \end{equation} $$
The transition of the Eq. 12 is given by the expression in Eq. 10, as $\frac{Dl}{dt}=D\cdot l$ and since the rotation tensor R is vanished in the deformation, the gradient of the velocity $\nabla u=D$. Which means **the contributions from the asymmetric part of the velocity gradient are canceled, since the rotation does not change its shape or volume**.

After tedious calculation, from the symmetric part only the diagonal tensor elements $D_{ii}$ remain:
$$ \begin{equation} \begin{split} \frac{DV}{Dt}&=VD_{ii} \\ &=V(D_{11}+D_{22}+D_{33}) \\ &=V(\nabla \cdot u) \end{split} \end{equation} $$
> [!Attention]
> This means:
> 
> - **The asymmetric part $R$ describes pure rotation of the rigid body** → no volume change and no distance change
> - **Only the symmetric diagonal elements ($D_{ii}$) describe expansion (i.e., stretching) or compression along coordinate axes** → volume change!
>     - The diagonal elements are also called principal strain rates, or the three principal eigenvectors
> - **The symmetric off-diagonal elements cause shear deformation** → shape change, but not volume change!


### Rotation
The rotation tensor $R$ is anti-symmetric. The contribution to the rate of change of the distance vector can thus be expressed as the vectorial product of vorticity vector $\omega$ and distance vector $l$:
$$ \begin{equation} R\cdot l=\frac{1}{2} \omega \times l \end{equation} $$
which describing the pure rotation of $l$ with a angular velocity $\frac{1}{2}\omega$. Written in terms of the velocity gradient, the **vorticity vector** is therefore expressed as:
$$ \begin{equation} \omega= (\frac{\partial u_{3}}{\partial x_{2}}-\frac{\partial u_{2}}{\partial x_{3}}) + (\frac{\partial u_{1}}{\partial x_{3}}-\frac{\partial u_{3}}{\partial x_{2}}) + (\frac{\partial u_{2}}{\partial x_{1}}-\frac{\partial u_{1}}{\partial x_{2}})=\nabla \times \mathbf u \end{equation} $$
It is the curl of the velocity vector. While the effect of the rotation tensor on the local relative motion is elucidated by considering the fluid in an infinitesimal disk with radius $r$ and normal vector $n$, centred around the reference point. The fluid generates average angular velocity $u_{tangential}/r$ which we estimated as $u_{tangential}\approx \frac{r}{2}\omega \cdot n$

# 1.2 Thermodynamics of Sea Water

## 1.2.1 Salt concentration and salinity

some basic terms in the salt concentration:
- $s$: salt concentration; in kg salt/kg seawater
- $\rho_{s}$: density of salt; in kg salt/m^3
- $w$: content of freshwater; in kg freshwater/kg seawater
- $\rho_{w}$: density of freshwater; in kg freshwater/m^3

**Where both $s$ and $w$ are concentrations in the conventional sense → dimensionless!!**
$$ \rho_{s}=s\rho,\quad \rho_{w}=w\rho=(1-s)\rho\quad and\quad \rho=\rho_{s} +\rho_{w} $$

where $\rho$ refers to the density of total mass of the system (seawater)

The above relationship will change if more than 2 partial masses contributes to the total masses.

In more practical use, the salinity is more often been expressed as:
$$ S=1000s \quad g \;(salt)/kg\;(seawater) $$

## 1.2.3 First Law of Thermodynamics

### Closed system → no mass exchange
> [!Attention]
> The first law of thermodynamics introduces the concept of **internal energy $E$ (energy per mass, in $J/kg^{-1}=m^{2}s^{-2}$).** In seawater, the internal energy per mass contains the kinetic energy of water molecules and the binding/solution energy of freshwater and salt ions (more often refer to as chemical energy).
> 
> In a closed system, internal energy change rate can be expressed as:
> 
> $$ \begin{equation} dE=-pdv+\delta Q \end{equation} $$
> 
> **The change in internal energy $E$ in a closed system corresponds to:**
> 
> - work on the volume: $-pdv$
> - total heating per mass which is added to the system: $\delta Q$, resulting from external fluxes, internal conversions

The internal energy per mass is an additive variable, which satisfied the additivity relation:
$$ E=\sum_{2} m_{i}E_{i} \quad with \quad E_{i}=\frac{\partial E}{\partial m_{i}} $$

The first law of thermodynamics may also be described in terms of enthalpy (per mass):
$$ H=E+pv $$

If written in additive from, the:
$$ H=\sum_{2} m_{i}H_{i} \quad with \quad H_{i}=\frac{\partial E}{\partial m_{i}}=E_{i}+pv_{i} $$
and $H_{i}(m_{1},...,m_{n},T,p)$ is the specific enthalpy of constituent

hence the change in enthalpy can be described:
$$ \begin{equation} \begin{split} dH &=dE+pdv+vdp \\ &=vdp+\delta Q \end{split} \end{equation} $$

### Open system → allow mass exchange
In a open system, we much account for difference enthalpies of the constitutes:
$$ \begin{equation} dH=\delta Q+vdp+\sum_{i} H_{i}dm_{i} \end{equation} $$
$$ \begin{equation} dE=\delta Q-pdv+\sum_{i} H_{i}dm_{i} \end{equation} $$
For seawater, the rate of change of enthalpy:
$$ dE=\delta Q-pdv+H_{s}dm_{s}+H_{w}dm_{w} $$
> [!Tip]
> Written in additive form and change the independent variable from salt concentration $s$ to salinity $S$, one obtain:
> 
> $$ \begin{equation} dE=\delta Q -pdv+\frac{\partial H}{\partial S}dS \quad or\quad dH=\delta Q +vdp+\frac{\partial H}{\partial S}dS \end{equation} $$
> 
> As the convenient form of the first law for seawater

Note that:
- $\delta Q$ contains all internal irreversible and all external energy exchanges, except those connected to the diffusive exchange of salinity
    - if melting/freezing happens near the ocean’s surface, it is commonly included in the formulation of energy and mass exchange at the boundary rather as a source/sink in the interior ocean
- $dS$ includes all diffusive exchange of salinity

## 1.2.4 Second Law of Thermodynamics

If energy and mass are exchanged in such a way that the system can be brought back into its initial state without leaving changes in the environment, the process is called _reversible_, otherwise _irreversible_.
→ Natural processes are generally irreversible because dissipative changes occur during the process.
→ Reversibility is an idealisation

### Closed System → no mass exchange
The second law of thermodynamics states that when a small amount of energy $\delta Q$ is added to a closed system at absolute temperature $T$, the quantity $\delta Q/T$ is equivalent to the change of a state variable, which is the **entropy** (entropy per mass, thus in $m^{2}s^{-2}K^{-1}$) given by:
$$ \begin{equation} d\eta=\frac{\delta Q}{T} \quad (closed\;system) \end{equation} $$
The entropy is also an additive variable:
$$ \eta_{i}(m_{1},...,m_{n},T,p) =\sum_{i} m_{i}\eta_{i} \quad with \quad \eta_{i}=\frac{\partial \eta}{\partial m_{i}} $$
### Open system → allow mass exchange
$$ \begin{equation} d\eta=\frac{\delta Q}{T} +\sum_{i} \eta_{i}dm_{i}\quad (open\;system) \end{equation} $$
Processes that conserve entropy $\eta$, which satisfy $d\eta=0$, are called isentropic.
- Processes in an adiabatically closed system with $\delta Q=0$ and $dm_{i}=0$ are always isentropic.
- Reversible processes (which have $\delta Q^{irr}=0$ and $dm_{i}^{irr}=0$) are not necessarily isentropic. Likewise, isentropic processes need not be reversible;
- The two concepts, Reversible and isentropic, are only equivalent if there are no external (irreversible) sources for energy and mass
    - In a reversible process, entropy can still change if there is reversible heat exchange (e.g., phase change of a gas)
    - In a isentropic process, entropy could stay constant due to compensation of irreversible sources and sinks (e.g., air expands, but condensation releases heat)
    - Reversible = isentropic is largely the case in interior ocean, but not in the atmosphere (due to radiation, and phase change of water vapour)

Applying to seawater, gives:
$$ \begin{equation} \begin{split} d\eta=\frac{\delta Q}{T}+\frac{\partial \eta}{\partial S}dS \end{split} \end{equation} $$
Note the the salinity exchanges in the interior ocean are normally diffusive and hence irreversible


## 1.2.5 Thermodynamics Potential

Helmholtz free energy (or work function) $F(m_{1},...,m_{n},T,p)$ and the free enthalpy (or Gibbs function) $G(m_{1},...,m_{n},T,p)$ are defined by:
$$ \begin{equation} F=E-T\eta=\sum m_{i}F_{i} \end{equation} $$
$$ \begin{equation} G=E+pv-T\eta=H-T\eta=\sum m_{i}\mu_{i} \end{equation}

$$
where $\mu_{i}=E_{i}+pv_{i}-T\eta_{i}$

> [!Attention]
> The four potentials $E,H,F,G$ all have the dimension energy per mass but a rather different physical interpretation.
> 
> - **The internal energy $E$ describes the sum of all energies participating in energy transformations**, in particular energy of molecular motions and intermolecular attraction potential, as well as the ‘chemical’ energy of the dissolution of salt in seawater.
> - **The enthalpy $H$ describes the heat content at constant pressure.**
> - **The Gibbs function $G$ is the chemical potential of the mixture** (in other texts $G$ is frequently denoted by $\mu$). From $dG=-\eta dT+vdp$ we learn that G is constant for isothermal-isobaric processes, it is actually the only thermodynamic potential with this property.
>     - $dF=-\eta dT-pdv$, hence the **free energy $F$ is the energy that is available for conversion into work under isothermal conditions.**

The total change of the any potential can be expressed in the following form:
$$ \begin{equation} dE(m_{1},...,m_{n},\eta,v)=\sum \mu_{i}dm_{i}+Td\eta -pdv \end{equation} $$
$$ \begin{equation} dH(m_{1},...,m_{n},\eta,p)=\sum \mu_{i}dm_{i}+Td\eta +vdp \end{equation} $$
$$ \begin{equation} dF(m_{1},...,m_{n},T,v)=\sum \mu_{i}dm_{i}-\eta dT -pdv \end{equation} $$
$$ \begin{equation} dG(m_{1},...,m_{n},T,p)=\sum \mu_{i}dm_{i}-\eta dT +vdp \end{equation} $$
while for seawater specifically, if consider salinity $S$ with the chemical potential $\mu$, we obtain free energy and Gibbs function in the form:
