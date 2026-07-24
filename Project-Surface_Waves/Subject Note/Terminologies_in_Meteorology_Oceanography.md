---
tags:
  - "#Theory"
  - "#approximations"
  - "#climate_dynamics"
Last Eddited: 2026-07-24
---
# Boussinesq Approximation
## Concept
> [!Attention] The **Boussinesq approximation** usually means:
> $$ \boxed{\rho=\rho_0(z)+\rho',\qquad |\rho'|\ll\rho_0} $$
> - Density variations are small compared with a constant reference density $\rho_0$.
> - Density variations are neglected except in the buoyancy term of the momentum equation

Because of that, density is replaced by $\rho_0$ in inertia and pressure-gradient denominators:

$$ \boxed{\rho\frac{D\mathbf u}{Dt}\approx\rho_0\frac{D\mathbf u}{Dt},\qquad \frac{1}{\rho}\nabla p\approx\frac{1}{\rho_0}\nabla p.} $$
But density variations are **kept in gravity/buoyancy**:
$$ \boxed{-\rho g\hat{\mathbf z}= -\rho_0g\hat{\mathbf z}-\rho' g\hat{\mathbf z}.} $$
After removing the large background hydrostatic balance, the remaining buoyancy is
$$ \boxed{b=-\frac{g\rho'}{\rho_0}.} $$
It also implies incompressible/volume-conserving flow:
$$ \boxed{\nabla\cdot\mathbf u=0.} $$
or, in primitive-equation form,
$$ \boxed{\nabla_h\cdot\mathbf v+\partial_z w=0.} $$
The pressure part should be stated carefully:
$$ \boxed{\text{Boussinesq does not require pressure variations to be small in the same way as density variations.}} $$
Pressure is still dynamically important. What changes is that pressure acceleration is divided by constant $\rho_0$, not by variable $\rho$:
$$ \boxed{\frac{1}{\rho}\nabla p\rightarrow\frac{1}{\rho_0}\nabla p.} $$
So your list should be:
- volume conservation / incompressibility,
- density variation is small compared with $\rho_0$,
- density variation is neglected everywhere except buoyancy/gravity,
- pressure remains dynamically active, but pressure-gradient acceleration uses $\rho_0$.

## Boussinesq Ertel PV



# Hydrostatic Approximation
## Concept


## Hydrostatic Boussinesq Ertel PV
from the full Boussinesq Ertel PV:
$$ q_B=(\nabla\times\mathbf u+f\hat{\mathbf z})\cdot\nabla b. $$
With $\mathbf u=(u,v,w)$,
$$ \nabla\times\mathbf u=(\partial_yw-\partial_zv,\partial_zu-\partial_xw,\partial_xv-\partial_yu). $$
Define
$$ \zeta=\partial_xv-\partial_yu. $$
Then
$$ q_B=(f+\zeta)\partial_zb+(\partial_yw-\partial_zv)\partial_xb+(\partial_zu-\partial_xw)\partial_yb. $$
Under the hydrostatic primitive-equation scaling,
$$ x,y\sim L,\qquad z\sim H,\qquad u,v\sim U,\qquad w\sim \epsilon U,\qquad \epsilon=\frac{H}{L}\ll1. $$
Then
$$ \partial_z u,\partial_z v\sim\frac{U}{H},\qquad \partial_xw,\partial_yw\sim\frac{\epsilon U}{L}=\epsilon^2\frac{U}{H}. $$
So $\partial_xw$ and $\partial_yw$ are smaller than $\partial_zu,\partial_zv$ by $O(\epsilon^2)$. Therefore the hydrostatic primitive-equation vorticity is approximated by
$$ \boldsymbol{\omega}_a^H=(-\partial_zv,\partial_zu,f+\zeta). $$
Thus the hydrostatic Boussinesq Ertel PV becomes
$$ \boxed{q_H=(f+\zeta)\partial_zb+\partial_zu\,\partial_yb-\partial_zv\,\partial_xb.} $$
In compact horizontal-vector form $\mathbf{v} = (u,v)$,
$$ \boxed{q_H=(f+\zeta)\partial_zb+\hat{\mathbf z}\cdot(\partial_z\mathbf v\times\nabla_h b).} $$
> [!Important] **Horizontal gradient of buoyancy**
> Where the horizontal gradient of buoyancy is not neglected under hydrostatic approximation. They enter the horizontal momentum equation indirectly through the vertical structure of the pressure gradient. This is exactly the baroclinic pressure-gradient effect. It can be shown as: $$\partial_z\left(\frac{1}{\rho_0}\nabla_h p'\right)=\nabla_h b. $$

For inviscid, adiabatic, unforced hydrostatic primitive equations, the PV conservation goes like:
$$ \boxed{(\partial_t+\mathbf v\cdot\nabla_h+w\partial_z)q_H=0.} $$
Using hydrostatic pressure anomaly, since $\partial_zp'=\rho_0b$, you could also write
$$ q_H=\frac{1}{\rho_0}\left[(f+\zeta)\partial_{zz}p'+\partial_zu\,\partial_{zy}p'-\partial_zv\,\partial_{zx}p'\right]. $$
But physically it is much clearer to keep the PV in buoyancy form. The hydrostatic approximation does **not** reduce PV to only $(f+\zeta)\partial_zb$; the vertical-shear terms $\partial_zu\,\partial_yb-\partial_zv\,\partial_xb$ are still part of hydrostatic Ertel PV.