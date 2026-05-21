---
tags:
  - wave/surface_wave
  - project/surfwaves
  - ICON/experiment
  - stokes_drift
Last Eddited: 2026-05-21
---
# Total-Sea Stokes Profile Reconstruction Method
## Method Framing
This experiment evaluates the climate-scale impact of adding a reconstructed Stokes drift profile to ICON. The target experiment is named `STOKES_TOTAL`, and it is compared against a control simulation, `CTRL`, in which no Stokes drift profile forcing is applied. The purpose of `STOKES_TOTAL` is to test whether a globally complete and physically defensible Stokes profile forcing modifies the simulated ocean state at climate-relevant scales.

The vertical Stokes drift profile is reconstructed from ERA5 wave reanalysis using the Phillips-spectrum approximation of (Breivik et al., 2016). This profile was developed as a practical alternative to simpler monochromatic wave profiles and earlier exponential-integral approximations. Its main advantage is that it provides a more realistic broadband spectral shape while requiring only two bulk quantities: the surface Stokes drift vector and the Stokes transport. This makes it suitable for large-scale ocean-model applications where the full two-dimensional wave spectrum is not used explicitly.

The Stokes transport is defined as the depth integral of the Stokes drift profile,

$$
\mathbf{V}_s=\int_{-\infty}^{0}\mathbf{u}_s(z)\,dz ,
$$

where $\mathbf{u}_s(z)$ is the Stokes drift velocity at depth $z$. ERA5 provides the total surface Stokes drift vector, $\mathbf{u}_{s0}$, directly. The corresponding Stokes transport is not used as a direct ERA5 output here, and is instead estimated from bulk total-sea wave parameters as

$$
\mathbf{V}_s \approx \frac{2\pi}{16}\bar{f}H_{m0}^{2}\hat{\mathbf{k}},
$$

where $H_{m0}=4\sqrt{m_0}$ is the significant wave height, $\bar{f}=m_1/m_0$ is the first-moment mean frequency, and $\hat{\mathbf{k}}$ is the wave propagation direction. In `STOKES_TOTAL`, the transport magnitude is estimated from total-sea wave parameters, while the horizontal direction of the reconstructed profile is aligned with the ERA5 surface Stokes drift vector. The reconstructed profile therefore satisfies the ERA5 surface Stokes drift constraint and uses the bulk wave state to set the vertical integral.

The vertical shape is given by the Phillips-spectrum profile approximation,

$$

\mathbf{v}_s(z)\approx \mathbf{v}_0

\left\{

e^{-2\overline{k}|z|}

-

\beta\sqrt{2\pi \overline{k}|z|}

\left[

\operatorname{erfc}\left(\sqrt{2\overline{k}|z|}\right)

\right]

\right\},

$$

where $\mathbf{v}_0$ is the surface Stokes drift vector, $\beta$ is a profile parameter, and $\overline{k}$ is an inverse depth scale. For a Phillips spectrum, $\beta=1$. Breivik et al., 2016 showed that $\beta=1$ gives good agreement between modelled and parameterised Stokes drift profiles. The inverse depth scale is determined from the surface Stokes drift magnitude, $v_0=|\mathbf{v}_0|$, and the Stokes transport magnitude, $V_s=|\mathbf{V}_s|$, as $$\overline{k}=\frac{v_0}{2V_s}\left(1-\frac{2\beta}{3}\right).$$
In `STOKES_TOTAL`, $\mathbf{v}_0$ is the ERA5 total surface Stokes drift vector, and $V_s$ is estimated from total-sea bulk wave parameters. This formulation gives a vertically decaying Stokes profile that is consistent with the prescribed surface value and bulk transport estimate.

## Rationale For The Total-Sea Closure
The total-sea reconstruction is selected as the operational method because it provides a single, globally applicable closure. For a climate-scale ICON experiment, this consistency is important: every ocean grid point is forced using the same reconstruction logic, and the main experiment can be interpreted as the response to a globally complete Stokes profile forcing. The method also keeps the physical assumption transparent. It assumes that the reconstructed profile follows the direction of the ERA5 surface Stokes drift, while the profile depth scale and integrated strength are constrained by the estimated total-sea Stokes transport.

This choice is scientifically defensible for the present purpose because the study focuses on the climate-scale impact of Stokes forcing. The Phillips-spectrum approximation is more appropriate than a monochromatic profile for a broadband sea state, but it remains simple enough to apply globally with ERA5 bulk diagnostics. In this sense, `STOKES_TOTAL` represents a median-complexity method: it is more physically informed than a single monochromatic wave assumption, while retaining a transparent and uniform global closure.

## Limitations
The total-sea method is still an approximation. It assumes that the entire reconstructed Stokes profile has the direction of the total surface Stokes drift. This can be imperfect in mixed sea states, where short wind waves may dominate the surface Stokes drift while longer swell contributes more strongly to the Stokes transport and deeper profile. The method therefore cannot represent vertical rotation of the Stokes drift vector with depth, nor can it separate wind-sea and swell contributions to the ocean response.

These limitations are acceptable for the primary climate-scale experiment because the method is applied consistently everywhere and avoids spatially mixed reconstruction regimes. The main research question is whether adding a physically plausible Stokes profile forcing changes the simulated climate-scale ocean state. For this question, a transparent and globally complete total-sea closure provides a clean operational forcing. The unresolved directional complexity should be acknowledged in the discussion as a limitation of bulk Stokes profile reconstruction and, if needed, assessed separately with complementary sensitivity or diagnostic tests.

## Target Experiment
The primary experiment is:
- `STOKES_TOTAL`: ICON simulation forced with the globally reconstructed total-sea Stokes drift profile.
The reference experiment is:
- `CTRL`: ICON simulation without Stokes drift profile forcing.
The main modeled response is therefore evaluated as
$$
STOKES\_TOTAL - CTRL .
$$
This comparison isolates the effect of adding the total-sea reconstructed Stokes profile to ICON under a globally consistent forcing strategy.
