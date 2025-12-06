---
tags:
  - project/surfwaves
  - wave/surface_wave
  - stokes_drift
  - Theory
  - Subject-Note
Last Eddited: 2025-11-30
---
The note is referring to the journal paper: **Breivik et al., 2014 (**[10.1175/JPO-D-14-0020](https://doi.org/10.1175/JPO-D-14-0020.1)**). Please find the original paper in Zotero: 2025 Ocean Waves**

# Stoke Drift Profile

The Stokes drift profile in water of arbitrary depth was shown by Kenyon (1969) in the case of linear waves to relate to the **wave variance spectrum** (Breivik et al. (2014)):

$$ \begin{equation}\mathbf{v}_s(z) = g \int_{-\infty}^{\infty} F(\mathbf{k}) \frac{\mathbf{k}}{\omega} \left[ \frac{2k \cosh 2k(z + h)}{\sinh 2kh} \right] \mathrm{d} \mathbf{k} \tag{1}\end{equation} $$

where the $k=|\mathbf{k}|=|(k_x, k_y)|$ is the magnitude of the wavenumber vector, $h$ is the bottom depth (positive), $g$ is the gravitational acceleration, $\omega =2 \pi f$ is the circular frequency, and $z$ is the vertical coordinate (positive up).

> [!Attention]
> **To avoid confusion, we use $v$ for Stokes drift velocities and $u$ for Eulerian currents**

> [!Tip]
> ### **Wave variance spectrum:**
> In the real ocean, the free surface elevation $\eta(\mathbf{x},t)$ is not a single sinusoid but a _random superposition_ of many waves with different wavenumber, frequencies, directions and amplitudes. Because it is random, we describe it by its variance, rather than by individual amplitudes.
> Then, the total surface-elevation variance is: $\langle \eta^2 \rangle$. Kenyon (1969) decomposed this variance into contributions from each wavenumber: $\langle \eta^2 \rangle = \int F(\mathbf{k})\, d\mathbf{k}$, which is the wave variance spectrum in wavenumber space.
> Hence, the $F(\mathbf{k})$ (unit: $m^4$) indicates surface variance density per unit wavenumber area. It tells you how much of the sea-surface variance (energy) ****is contained in waves with wavenumber vector $\mathbf{k}$.

> [!Tip]
> ### **Deep water limit:**
> Considered when the water depth $h$ is much greater than the wave wavelength.
> Hence, the linear surface gravity wave dispersion relation becomes:
> $$ \omega^2=gk\tanh{(kh)}\approx gk $$
> as when water depth $h$ is much greater than the wavelength, it has: $h \gg \lambda/2 \quad \text{or}\quad kh \gg 1$
> $$ \tanh{(kh)}=\frac{\sinh{(kh)}}{\cosh{(kh)}}\approx \frac{\frac{1}{2}e^{kh}}{\frac{1}{2}e^{kh}} \approx 1 $$

In the following, we will only consider the **deep-water limit** of the dispersion relation:
$$ \begin{equation} \omega^2=gk \tag{2}\end{equation} $$
Hence, we will have the dispersion relationship in terms of frequency:
$$ k=\frac{(2 \pi f)^2}{g} $$
With the Eq (2), the ~={red}**Stokes drift profile**=~ Eq (1) simplified to:
$$ \begin{equation} \mathbf{v_s}(z)=\frac{2}{g}\int\int_{-\infty}^{\infty}\omega^3 \hat{\mathbf{k}} e^{2kz}F(\mathbf{k})\;d\mathbf{k} \tag{3}\end{equation} $$
where the $\hat{\mathbf{k}}=\mathbf k/k$ is the unit vector in the direction of the wave component. The double integrals refer to the 2-D wavenumber space $(k_x, k_y)$

We now recast the east and north components of the ~={red}**Stokes drift profile** in frequency direction=~ $(f, \theta)$ coordinates as:
$$ \begin{equation} \mathbf{v_s}(z)=\frac{16\pi^3}{g}\int_{0}^{2\pi} \int_{0}^{\infty}f^3 \hat{\mathbf{k}} e^{2kz}F(f, \theta)\;df\:d\theta \tag{4} \end{equation} $$
Where $\theta$ is measured clockwise from north (going to).

The ~={red}**Stokes transport**=~ $\mathbf{V_s}=\int_{0}^{\infty}\mathbf{v_s}(z)dz$ becomes in the deep water limit:
$$ \begin{equation} \mathbf{V_s}=2\pi\int_{0}^{2\pi} \int_{0}^{\infty}f\hat{\mathbf{k}}F(f,\theta)dfd\theta \tag{5}\end{equation} $$
The integrand (Eq (5)) here is the first-order moment of the wave spectrum $m_1$ weighted by the unit vector $\hat{\mathbf{k}}$ of the wave component, with the $n$th-order moment of the 2D spectrum defined as:
$$ \begin{equation} m_n=\int_{0}^{2\pi}\int_{0}^{\infty}f^nF(f,\theta)dfd\theta \tag{6}\end{equation} $$

Estimating the full profile from Eq (4) can be a costly operation even when a modelled or observed wave spectrum is available.

> **(2)→(3) Simplification process:**
> since large $h$, we will have both $2kh$ and $2k(z+h)$ are large. Since:
> $$ \sinh{x}\sim\frac{1}{2}e^x,\quad \cosh{x}\sim\frac{1}{2}e^x\quad\quad(x\gg1) $$
> Thus, we will have the below simplification in Eq (1):
> $$ \sinh{(2kh)}\approx\frac{1}{2}e^{2kh},\quad \cosh{[2k(h+z)]}\approx\frac{1}{2}e^{2k(z+h)} $$

> **(3)→(4) Transform process:**
> given: (1) $\mathbf{k}=(k_x,k_y)$ and $k=|\mathbf{k}|$; (2) $\omega=2\pi f$ (angular frequency ←→ ordinary frequency);(3) keep $k$ in the exponent as $k(f)$ via the dispersion relationship $k=(2\pi f)^2/g=k(f)$ as in deep water limit
> (4) $d\mathbf{k}=dk_xdk_y=d^2k$ denotes the 2-D element in wavenumber space; in polar form $d^2k=k\:dk\:d\theta$; Hence, the frequency-direction spectrum $F(f,\theta) \;df\;d\theta=F(\mathbf{k})d^2k=F(\mathbf{k})k\;dk\;d\theta$
> Start from Eq (3)
> $$ \begin{align} \mathbf{v_s}(z)&=\frac{2}{g}\int\int_{-\infty}^{\infty}\omega^3 \hat{\mathbf{k}} e^{2kz}F(\mathbf{k})\;d\mathbf{k} \nonumber \\ &= \frac{2}{g}\int\int_{-\infty}^{\infty}\omega^3 \hat{\mathbf{k}} e^{2kz}F(\mathbf{k})\;d^2k \nonumber \\ &= \frac{16\pi^3}{g}\int\int_{-\infty}^{\infty}f^3 \hat{\mathbf{k}} e^{2kz}F(\mathbf{k})\;d^2k \nonumber \\ &= \frac{16\pi^3}{g}\int_{0}^{2\pi}\int_{0}^{\infty}f^3 \hat{\mathbf{k}} e^{2kz}F(\mathbf{k})\;kdkd\theta \nonumber \\ &= \frac{16\pi^3}{g}\int_{0}^{2\pi} \int_{0}^{\infty}f^3 \hat{\mathbf{k}} e^{2kz}F(f, \theta)\;df\:d\theta \nonumber \end{align} $$


> [!Attention]
> When a wave spectrum is not available, **the Stokes drift profile must be approximated from the transport Eq (5) and the surface Strokes drift velocity.**


# Approximate Stokes Drift Profile: Monochromatic Wave

It is therefore common to **approximate Eq (4) by the exponential profile of a monochromatic wave:**
$$ \begin{equation} v_m=v_0e^{2k_mz} \tag{7}\end{equation} $$

where $v_0$ is the surface Stokes drift velocity.

To ensure that the surface Stokes drift and the total transport of the monochromatic wave in Eq. (7) agree with the values for the full spectrum, Eqs (4)-(5), the wavenumber must be determined by:
$$ \begin{equation} k_m=\frac{v_0}{2V_s} \tag{8}\end{equation} $$

> (7)→(8) derivation:
> Given that:
> $$ V_s=\int_{-\infty}^{0} v_m\;dz $$
> we will have
> $$ V_s=\frac{v_0}{2k_m}\int_{-\infty}^{0} e^{2k_mz}\;d(2k_mz)=\frac{v_0}{2k_m} $$

> [!Attention]
> ==**However, this monochromatic wave profile is prone to problem:**==
> - Will have a weaker vertical gradient than the profile under a full spectrum near the surface
> - Will tends too quickly to zero deeper down </aside>

# Approximate Stokes Drift Profile: Alternative

The behaviour of the profile under a full spectrum is most readily investigated by considering the Phillips spectrum, applicable to the equilibrium range of the spectrum of wind generated waves above the spectral peak:
$$ \begin{equation} F_P= \begin{cases} \alpha_Pg^2\omega^{-5}, &\omega \gt \omega_p \\ 0, &\omega \le\omega_p \end{cases} \tag{9}\end{equation} $$
Set Phillips’ parameter $\alpha_P=0.0083$ (there is a disagreement, other works preferring $\alpha_P=0.0081$). The **peak circular frequency is denoted $\omega_p$.** the Stokes drift profile (Eq (4)) under Eq (9) is:
$$ \begin{equation} v_P(z)=2\int_{\omega_P}^{\infty}\alpha_Pg\omega^{-2}e^{2\omega^2z/g}\;d\omega \tag{10}\end{equation} $$
Which can be found analytically:
$$ \begin{equation} v_P(z)=2\alpha_Pg \biggl\{ \mathbf {exp}(2\omega_P^2z/g)/\omega_p -\sqrt{\frac{-2\pi z}{g}}\biggr[1-\mathbf{erf}(\frac{\omega_p}{\sqrt{-2z/g}})\biggr]\biggl\} \tag{11}\end{equation} $$
The transport can also be found analytically:
$$ \begin{equation} V_P=\frac{\alpha_Pg^2}{2\omega_P^3} \tag{12}\end{equation} $$

- **Near the surface (when $|z|$ is small)**, the term in Stoke drift profile Eq (11) involving the error function becomes vanishingly small compared with the first term, and it is clear that:
    $$ \begin{equation} v_P(z)\approx(2\alpha_Pg/\omega_p)e^{2k_pz} \tag{13}\end{equation} $$
    
    Here we have introduced the peak wavenumber $k_p=\omega_p^2/g$.
    
- **In deeper water, (when large $|z|$)**, we substitute the following asymptotic expansion for the error function in Eq (11), valid for large $x$:
    $$ \begin{equation} \mathbf{erf}(x)\approx 1-\frac{e^{-x^2}}{x\sqrt{\pi}}\bigg(1-\frac{1}{2x^2}\bigg) \tag{14}\end{equation} $$
    Hence, for the large $|z|$ profile, Eq (10) becomes:
    $$ \begin{equation} v_P(z)\approx\alpha_Pg^2(e^{2k_pz}/2\omega_p^3|z|) \tag{15}\end{equation} $$

Motivated by this, ==**Breivik et al. (2014) have explored a profile that approaches the exponential shape [Eq (13)] near the surface and goes like the asymptotic solution [Eq (15)] in the deep**==:
$$ \begin{equation} v_e=v_0\frac{e^{2k_ez}}{1-Ck_ez} \tag{16}\end{equation} $$

The coefficient that was found to minimise the MSE for the Phillips spectrum over the entire profile is $C\approx8$. Obviously the MSE takes into account discrepancies over the entire water column and will be more sensitive to deviations in the upper part where the drift is stronger.

The transport under such a profile involved the exponential integral $E_1$ and can be solved analytically to yield:

$$ \begin{equation} V_S=\frac{\frac{1}{4}(v_0e^{1/4}E_1)}{8k_e} \tag{17}\end{equation} $$

It will in the following be referred to as the exponential integral profile. This imposes the following constraint on the inverse depth scale:

$$ \begin{equation} k_e=\frac{\frac{1}{4}(v_0e^{1/4}E_1)}{8V_s} \tag{18}\end{equation} $$

Here $e^{1/4}E_1(1/4)\approx 1.34$; thus,

$$ \begin{equation} k_e\approx \frac{v_0}{5.97V_s}\approx\frac{k_m}{3} \tag{19}\end{equation} $$

Where the $k_m$ is the wavenumber for the monochromatic wave [Eq (8)].

> [!Attention]
> Thus, **this alternative profile relies on the same two quantities required for the monochromatic profile**, namely
> - Stokes transport $V_s$
> - surface Stokes drift velocity $v_0$
> 
> These two quantities is used to calculate the wavenumber $k_e$, then the profile $v_e$


**So how do model calculate the above two quantities?**

# The Shear of the Stokes Drift Profile

The ==production of **Langmuir turbulence arises from a vortex force term==: $\mathbf v_s \times \nabla\times\mathbf u$** (where the $\mathbf u=(u_x,u_y, u_z)=(u_1, u_2, u_3)$ is the vectorised Eulerian currents) **in the momentum equation** ([Leibovich 1983](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=The+form+and+dynamics+of+Langmuir+circulations.&btnG=)).

It is assumed that the vortex force gives rise to a term involving the shear of the Stokes drift velocity profile in the turbulence kinetic energy, although it is somewhat unclear whether this effect will be strong enough to explain the observed Langmuir circulation.

The turbulent kinetic energy (TKE) equation with a Stokes drift shear term can be written as:

$$ \begin{equation} \frac{De}{Dt} = \frac{g}{\rho_w}\overline{u_3'\rho'}-\overline{u_i'u_j'}\frac{\partial \overline u_i}{\partial x_j}-\overline{u_i'u_j'}\frac{\partial v_i}{\partial x_j} - \frac{\partial}{\partial x_j}(\overline{u_j'e})-\frac{1}{\rho_w}\frac{\partial}{\partial x_i}(\overline{u_i'p'})-\epsilon \tag{20}\end{equation} $$

Given that the $u$ stands for current velocity, $v$ stands for Stokes drift velocity

Here:
- $e=q^2/2=\overline{u_i'u_i'}/2$ is the TKE per unit mass (with $q$ the turbulence velocity)
- $\rho_w$ is the water density
- $\epsilon$ is the dissipation.
- Each term in the right-hand side:
    - $\frac{g}{\rho_w}\overline{u_3'\rho'}=\overline{b'w'}$: buoyancy production term
    - $\overline{u_i'u_j'}\frac{\partial \overline u_i}{\partial x_j}$: shear production term(s)
    - $-\overline{u_i'u_j'}\frac{\partial v_i}{\partial x_j}$: ~={red}**production of Langmuir turbulence; Involving the Reynolds stresses multiplied by the gradient in Stoles drift velocity $v_i$.**=~
    - $-\frac{\partial}{\partial x_j}(\overline{u_j'e})-\frac{1}{\rho_w}\frac{\partial}{\partial x_i}(\overline{u_i'p'})$: TKE Flux (redistribution) term


> [!Attention]
> ==**Langmuir Turbulence Production**== 
> By making the gradient transport closure approximation, ignoring advective terms and horizontal gradients, and rewriting in vectorial form, we arrive at:
> $$ \begin{equation} \frac{\partial e}{\partial t}=-\nu_hN^2+\nu_mS^2+\nu_m\mathbf S\cdot \frac{\partial \mathbf v_s}{\partial z}-\frac{\partial}{\partial z}(\overline{w'e})-\frac{1}{\rho_w}\frac{\partial}{\partial z}(\overline{w'p'})-\epsilon \tag{21}\end{equation} $$
> Using $z$ for the vertical axis and $w$ for vertical velocities.
> 
> Here:
> - $\nu_h,\nu_m$: diffusion coefficients and turbulent viscosity, respectively
> - $\nu_mS^2=\nu_m(\partial \overline{\mathbf{u}}/\partial z)^2$: shear production
> - $-\nu_hN^2=-\nu_h(-\frac{g}{\rho_w}\frac{d\rho}{dz})$: buoyancy production
> - ==**$\nu_m\mathbf S\cdot \frac{\partial \mathbf v_s}{\partial z}$: Langmuir turbulence production**==, wave-current interaction term (couple the shear of Eulerian current with the vertical shear of stokes drift)
>   - $\nu_m\mathbf S\cdot \frac{\partial \mathbf v_s}{\partial z}=\nu_m(\partial \overline{u}/\partial z)\frac{\partial \mathbf v_s}{\partial z}+ \nu_m(\partial\overline{v}/\partial z)\frac{\partial \mathbf v_s}{\partial z}$
> - Divergences of the pressure correlation term $\overline{w'p'}$ and turbulent transport $\overline{w'e'}$ (Redistribution term)

Notice that **the diffusion coefficient and turbulent viscosity are used when assume that the turbulent flux is related to the mean gradient (e.g., $\overline{u'w'}=-\nu\nabla \overline{u}$)**

The shear of the Phillips spectrum, an analytical solution can be found, but the shear goes to infinity on the surface, which is in contrast to the shear under a monochromatic wave, which remain bounded near the surface.

$$ \begin{equation} \frac{\partial v_m(z=0)}{\partial z}=2k_mv_0 \tag{22}\end{equation} $$

The shear of the exponential integral profile (Eq (16)) also remains bounded, but reaches a value approximately 67% higher than the monochromatic profile at the surface:

$$ \begin{equation} \frac{\partial v_e(z=0)}{\partial z}=10k_ev_0\approx \frac{10}{3}k_mv_0 \tag{23}\end{equation} $$

In practice, though, it may **be necessary to cap the Stokes shear near the surface** when estimating the Langmuir turbulence when assuming a tail proportional to f 25 (see next section).

# High-Frequency Contribution to the Profile

In wave prediction model (e.g., WAM, WaveWatch-III and ECMWF version of WAM), the above procedure is used to compute the Stokes profiles and transports from discretised wave spectra with ==**a high-frequency cut-off $f_c$.**==

> [!Attention]
> However, as the **stokes drift is weighted toward the high-frequency (HF)** part of the spectrum (since $\mathbf v_s\sim f^3$, Eq (4)), the tail beyond the cutoff frequency $f_c$ is significant both for the Stokes drift profile and the transport.
> In general, the operational wave spectra stop beyond the finite HF cutoff $f_c$, but waves with $f>f_c$ still exist and are significant.


One can follow Komen et al. (1994) and assume a tail of the form

$$ \begin{equation} F_{HF}=F(f_c,\theta)(\frac{f_c}{f})^5 \tag{24}\end{equation} $$

which is consistent with the Phillips spectrum (Eq (9)).

The two dimensional spectrum below the cutoff frequency $f_c$ is here assumed to come from observations or from a numerical wave prediction model. This is the procedure used for adding the diagnostic high-frequency contribution to the spectrum in the Wave model (Wave Model (WAM; see Wamdi Group 1988; Komen et al. 1994; Janssen 2004) as well as the WaveWatch-III model (Tolman 1991; Tolman et al. 2002). In the ECMWF version of the WAM model [ECWAM; see ECMWF (2013)] (Breivik et al., 2014, p. 2437). A **lower diagnostic cutoff** is set at:

$$ \begin{equation} f_d=min(f_{max}, 2.5\overline{f}_{windsea}) \tag{25}\end{equation} $$

here:
- $\overline{f}_{windsea}$ is the mean frequency of the wind sea based on the first moment
- $f_{max}$ is the highest resolved frequency of the modelled spectrum

Above the $f_d$ the spectrum is treated diagnostically, that is , a tail of the form Eq (24) overwrites the prognostic tail.


> [!Tip]
> $f_c$ and $f_d$:
> - $f_c$: **Theoretical cutoff used in Stokes drift equations**
>   - Indicates the frequency where the physical spectrum must be extended using a tail $F_{HF}$ when $f>f_c$
>- $f_d$: **Diagnostic cutoff in wave models (WAM, ESWAM)**
>   - Indicates where the model stops using prognostic spectrum (i.e., resolved $(f,\theta)$ space.
>- In practice:
>   - $f<f_c$: model describe prognostic spectrum
>   - $f_c<f<f_d$: model describe prognostic spectrum, but use HF tail ($F_{HF}$)
>   - $f>f_d$: model treat the spectrum diagnostically, HF tail overwrites the tail

The high-frequency tail adds the following contribution:

$$ \begin{equation} \mathbf v_{HF}(z)=\frac{16 \pi^3}{g}f_c^5\int_{0}^{2\pi}F(f_c,\theta)\hat{\mathbf k}\:d\theta\int_{f_c}^{\infty}\frac{\text{exp}(8\pi^2zf^2/g)}{f^2}\:df \tag{26}\end{equation} $$

The later integral is similar to Eq (10) and can be solved in a similar manner to Eq (11), yield:

$$ \begin{equation} \mathbf v_{HF}(z)=\frac{16 \pi^3}{g}f_c^5\int_{0}^{2\pi}F(f_c,\theta)\hat{\mathbf k}\:d\theta \times \biggl\{\frac{\text{exp}(-\mu f_c^2)}{f_c}-\sqrt{\mu \pi}[1-\text{erf}(f_c\sqrt{\mu})] \biggl\} \tag{27}\end{equation} $$

where $\mu=-8\pi^2z/g$. Thus, $\mu(z=0)=0$.

The **high-frequency addition to the surface Stokes drift** in **deep water** is:

$$ \begin{equation} \mathbf v_{HF}(0)=\frac{16 \pi^3}{g}f_c^4\int_{0}^{2\pi}F(f_c,\theta)\hat{\mathbf k}\:d\theta \tag{28}\end{equation} $$

ECWAM (ECMWF 2013) computes and outputs the surface Stokes drift velocity vector corrected for the high-frequency contribution. The tail contribution to the Stokes transport is:

$$ \begin{equation} \mathbf V_{HF}=\frac{2\pi}{3}f_c^2\int_{0}^{2\pi}F(f_c,\theta)\hat{\mathbf k}\:d\theta \tag{29}\end{equation} $$

# Modelled profiles in the NA

- The MSE of the exponential integral profile from the full Stokes profile is on average 35% of that of the monochromatic profile for our chosen location and model period
    - The improvement is consistent for a range of different sea states.
    - But poor performance is expected in cases when a one-dimensional fit is made to wave spectra with two diametrically opposite wave systems (e.g., a swell system travels in the opposite direction of the wind sea)

## Tail sensitivity of modelled Stokes drift profiles

Adding the contribution from the high-frequency tail is important, and indeed it is standard practice to include it in the computation of the **surface Stokes drift velocity** (see, e.g., the ECMWF model documentation; ECMWF 2013, p. 52) (Breivik et al., 2014)

> [!Attention]
> Breivik et al (2014) found that:
> - ~={red}**Substantial high-frequency tail contribution to surface stokes drift velocity**=~: the contribution from the spectral tail to the surface Stokes drift velocity found in Eq (27) on average is about a third and sometimes exceeding 75%!
> - **In contrast, small contribution to the Stokes transport**: the spectral tail contribution to the transport is generally small
> 
> However, the high frequency contribution decays rapidly with depth, below 0.5 meters, the Low-frequency match perfectly with the full profile. 
> → Which means, in model operation, **if the upper-most layer has a depth > 0.5 m, it cannot capture the high-frequency contribution**

When add HF contribution to both profiles (monochromatic, exponential integral), the match improves, especially for the exponential integral.

In principle it is straightforward to add this contribution to the approximate profile by way of Eq (27), but it requires knowledge of the two-dimensional wave spectrum $(f,\theta)$ at the cutoff frequency $f_c$.

## Discrepancy between the Stokes transport and $m_1$

It is clear that:

$$ \begin{equation} |\mathbf V_s|\le2\pi m_1 \tag{30}\end{equation} $$

However, it is not clear how large this deviation is on average for typical wave spectra in the open ocean. Assessing the overestimation is of practical value since the first spectral moment is often archived or indirectly measured.

since:
- $\overline{f}=m_1/m_0$ is defined as the mean frequency
- $H_{m_{0}}=4\sqrt{m_0}$ is the significant wave height

we can derive the first momentum from the integrated parameters (see Eq (6)) of a wave model or from wave observations and find an estimate for the Stokes transport:

$$ \begin{equation} \mathbf V_s\approx\frac{2\pi}{16}\overline f H_{m_{0}}^2 \hat{\mathbf k_s} \tag{31}\end{equation} $$

Here, $\hat{\mathbf k_s}=(\sin{\theta_s}, \cos{\theta_s})$ is the unit vector in the direction $\theta_s$ of the Stokes transport. $\theta_s$ is not normally archived by wave prediction models but it can be approximated by the mean wave direction (MWD) $\overline{\theta}$:

Estimating the Stokes transport (Eq (5)) from the first moment $m_1$ (Eq (6)) is attractive since it involves only integrated parameters readily available from wave models. However, using $m_1$ and Eq (31) will **overestimate** the Stokes transport on average by 17%.

Similarly, the surface Stokes drift velocity:

$$ \begin{equation} |\mathbf v_0| \le16\pi^3m_3/g \tag{32}\end{equation} $$

The estimate from $m_3$ will be on average about 19%.

## Deviation between the Stokes transport direction and the mean wave direction

mean wave direction (MWD) $\overline{\theta}$:

$$ \begin{equation} \overline{\theta}=\arctan \Biggr[ \frac{\int_{0}^{2\pi}\int_{0}^{\infty}\sin{\theta}F(f,\theta)\:df\:d\theta}{\int_{0}^{2\pi}\int_{0}^{\infty}\cos{\theta}F(f,\theta)\:df\:d\theta} \Biggr] \tag{33}\end{equation} $$

How well it approximates the direction of the Stokes transport since it is a standard output parameter of many wave models?

The average deviation is about 28 and 75% of the time the difference is less than 108. In contrast, Fig. 8b shows a much larger deviation between the direction of the Stokes transport and the surface Stokes drift velocity (Breivik et al., 2014, p. 2441). This is due to the sensitivity to high-frequency wave components arising from the 3rd power of the frequency $f$ under the integral in Eq (4) of Stokes drift profile.

> [!Attention]
> It will therefore in general be **better to estimate the Stokes transport direction from the mean wave direction** rather than from the surface Stokes direction.


# Summary: Recommendation from the Authors:

- The alternative profile proposed here has been shown to be a better approximation than the monochromatic approximation for both theoretical spectra, modelled 2D spectra in the open ocean, and 1D observed spectra.
- Utilising this alternative profile comes at no added cost since the computation relies on the same two parameters
- In the open ocean the mean wave direction (MWD) serves as a good proxy for the Stokes transport direction, it is a significantly better substitute than the surface Stokes drift direction.
- Adding the contribution from the tail gives an important contribution to the Stokes drift velocity in the upper half meter in the open ocean. Its impact rapidly decays, and below 0.5 m, the difference is marginal. However, treat the high-frequency contribution relies on the full 2D spectrum makes this approach impractical.