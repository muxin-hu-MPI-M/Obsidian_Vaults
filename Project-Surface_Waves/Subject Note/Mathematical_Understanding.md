---
tags:
  - project/surfwaves
  - wave
  - wave/surface_wave
  - Fourier_Transform
  - math
Last Eddited: 2026-07-14
---
# Fourier Transform
The video link: [https://youtu.be/gTOzmE7_-mU?si=hh7dkCD_TSs3r_HI](https://youtu.be/gTOzmE7_-mU?si=hh7dkCD_TSs3r_HI)
The paper link:

- ==Fourier transform==:
	- ~={red}**It takes a signal in time (or space) and expresses is as a sum of sinusoidal waves with different frequencies, amplitudes and phases**=~
	- It decomposes any signal into its building-block waves
	- If $X(f)$ is the Fourier Transform of $x(t)$, then the Power Spectral Density (PSD) is:
	  $$
	    PSD(f)=\frac{|X(f)|^2}{(\text{normalisation})}
		$$
		Hence, PSD represents the distribution of power (i.e., variance) per unit frequency.
	- If the FT is applied to a *time series*, then the PSD tells you:
		- ~={red}**how the variance (energy) of a signal is distributed across frequencies or wavenumbers**=~.
		- Which time scales dominate variability. It answers what time scales control the **variability** of the scaler quantity
		- Variability is dominated by rapid (high frequency) or slow (low frequency) processes

# Vector product rules
==**Distributivity**==
- cross product:
$$
(\mathbf{a}+\mathbf{b})\times \mathbf{c} = \mathbf{a}\times \mathbf{c} + \mathbf{b}\times \mathbf{c}
$$
$$
\mathbf{a}\times (\mathbf{b} + \mathbf{c}) = \mathbf{a}\times \mathbf{b} + \mathbf{a}\times \mathbf{c}
$$
- dot product:
$$
\mathbf{a}\cdot (\mathbf{b} + \mathbf{c}) = \mathbf{a}\cdot \mathbf{b} + \mathbf{a}\cdot \mathbf{c}
$$
$$
(\mathbf{a} + \mathbf{b}) \cdot \mathbf{c} = \mathbf{a}\cdot \mathbf{c} + \mathbf{b}\cdot \mathbf{c}
$$
**Commutativity/Anti-commutative**
- dot product is commutative
$$\mathbf a \cdot \mathbf b = \mathbf b \cdot \mathbf a$$
- cross product is anti-commutative
$$\mathbf a \times \mathbf b = -\mathbf b \times \mathbf a $$
- Thus:
$$\mathbf a \times \mathbf a = -\mathbf a \times \mathbf a =0$$
**Scalar Multiplication**
- scalar ($\lambda$) can be moved in or out
$$(\lambda\mathbf a) \times \mathbf b = \mathbf a \times (\lambda\mathbf b) = \lambda (\mathbf a \times \mathbf b) $$
$$(\lambda\mathbf a) \cdot \mathbf b = \mathbf a \cdot (\lambda\mathbf b) = \lambda (\mathbf a \cdot \mathbf b) $$
==**Associativity**==
Dot and cross products are not generally associative in the same way multiplication is.
- cross products
$$(\mathbf a \times \mathbf b) \times \mathbf c \ne \mathbf a \times (\mathbf b \times \mathbf c) $$
- dot products ($\mathbf a \cdot \mathbf b=\text{Scalar}$)
$$ (\mathbf a \cdot \mathbf b) \mathbf c \ne \mathbf a(\mathbf b \cdot \mathbf c) $$
**<span style="background:#fff88f">Vector triple product</span>**
- general forms
$$
\begin{align}
\mathbf{a}\times (\mathbf{b} \times \mathbf{c}) &= \mathbf{b}(\mathbf a \cdot \mathbf{c}) - \mathbf{c}(\mathbf a \cdot \mathbf{b})\\
(\mathbf{a}\times \mathbf{b}) \times \mathbf{c} &= \mathbf{b}(\mathbf a \cdot \mathbf{c}) - \mathbf{a}(\mathbf b \cdot \mathbf{c}) \\
\mathbf{a}\cdot(\mathbf{b}\times\mathbf{c}) &= \mathbf{b}\cdot(\mathbf{c}\times\mathbf{a}) = \mathbf{c}\cdot(\mathbf{a}\times\mathbf{b}) \\
\mathbf{a}\cdot(\mathbf{b}\times\mathbf{c}) &= -\mathbf{a}\cdot(\mathbf{c}\times\mathbf{b})
\end{align}
$$

# Useful conversions in wave-averaged momentum framework
- **Einstein conversion**:
$$
\begin{align}
	u_j \nabla u_j &=u_x \nabla u_x + u_y \nabla u_y + u_z \nabla u_z \\
	&=
	\begin{pmatrix}
		u_x \partial_x u_x + u_y \partial_x u_y + u_z \partial_x u_z \\
		u_x \partial_y u_y + u_y \partial_y u_y + u_z \partial_y u_z \\
		u_x \partial_z u_z + u_y \partial_z u_y + u_z \partial_z u_z
	\end{pmatrix}.
\end{align}
$$
- if $\mathbf \alpha=(\alpha_x, \alpha_y, \alpha_z), \mathbf \beta=(\beta_x, \beta_y, \beta_z)$. **See details in (Suzuki & Fox-Kemper, 2016)**
$$
\begin{align}
\mathbf{\alpha}\times (\nabla\times \mathbf{\beta}) &= \alpha_j \nabla \beta_j - (\alpha \cdot \nabla)\times \beta \\
&=\alpha_x \nabla \beta_x + \alpha_y \nabla \beta_y + \alpha_z \nabla \beta_z - (\alpha \cdot \nabla)\times \beta
\end{align}
$$
- **conservative force**
$$ - \frac{\nabla |\mathbf u|^2}{2}=-u_j\nabla u_j $$
- **Convective acceleration identity** (i.e., Lamb form identity); Standard identity used to convert between advective from and vortex-force form of the momentum equation
$$(\mathbf{u}\cdot\nabla)\mathbf{u} = (\nabla \times \mathbf{u}) \times \mathbf{u} + \nabla(\frac{1}{2}|\mathbf{u}|^2)$$
