---
tags:
  - "#Theory"
  - "#approximations"
  - "#climate_dynamics"
Last Eddited: 2026-07-24
---
# Boussinesq approximation
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