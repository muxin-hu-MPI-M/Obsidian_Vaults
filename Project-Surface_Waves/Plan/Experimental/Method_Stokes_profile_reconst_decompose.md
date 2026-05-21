---
tags:
  - project/surfwaves
  - ICON/experiment
  - stokes_drift
Last Eddited: 2026-05-21
---
# Concise Method Framing
We reconstruct the vertical Stokes drift profile from ERA5 wave reanalysis using the Phillips-spectrum approximation of (Breivik et al., 2016) This method requires the surface Stokes drift vector, $\mathbf{u}_{s0}$, and the depth-integrated Stokes transport,
$$
\mathbf{V}_s=\int_{-\infty}^{0}\mathbf{u}_s(z)\,dz .
$$
ERA5 provides $\mathbf{u}_{s0}$ directly, while $\mathbf{V}_s$ is estimated from bulk wave parameters as
$$
\mathbf{V}_s \approx \frac{2\pi}{16}\bar{f}H_{m0}^{2}\hat{\mathbf{k}},
$$
where $H_{m0}=4\sqrt{m_0}$, $\bar{f}=m_1/m_0$, and $\hat{\mathbf{k}}$ is the mean wave propagation direction. A total-sea reconstruction can therefore be obtained by combining the ERA5 total surface Stokes drift with the transport estimated from total wave parameters. However, this assumes that the whole Stokes profile follows the surface Stokes drift direction, which may be problematic in mixed sea states where short wind waves dominate the surface drift but longer swell contributes more strongly to the transport.

For both the total-sea and decomposed reconstructions, the same Phillips-spectrum vertical profile is used:
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
where $\mathbf{v}_0$ is the surface Stokes drift vector, $\beta$ is a profile parameter, and $\overline{k}$ is an inverse depth scale. For a Phillips spectrum, $\beta=1$, and Breivik et al. \citep{Breivik2016} showed that this value gives good agreement between modeled and parameterized Stokes drift profiles. The inverse depth scale is determined from the surface Stokes drift magnitude, $v_0=|\mathbf{v}_0|$, and the Stokes transport magnitude, $V_s=|\mathbf{V}_s|$, as
$$

\overline{k}=

\frac{v_0}{2V_s}

\left(1-\frac{2\beta}{3}\right).

$$
Thus, the total-sea and decomposed experiments differ in how $\mathbf{v}_0$ and $V_s$ are defined, not in the vertical profile function itself.

To account for this, we separate the reconstruction into wind-sea and total-swell components where possible. Partition-specific Stokes transports are estimated from the corresponding significant wave height, first-moment mean frequency, and mean wave direction. The unknown surface Stokes drift magnitudes of the wind-sea and swell components are obtained from
$$
u_{s0}\hat{\boldsymbol{\theta}}
=
u_{s,ww}\hat{\boldsymbol{\theta}}_{ww}
+
u_{s,ts}\hat{\boldsymbol{\theta}}_{ts},
$$
with the physical constraint

$$
u_{s,ww}\ge 0, \qquad u_{s,ts}\ge 0 .
$$

Here, $\hat{\boldsymbol{\theta}}$, $\hat{\boldsymbol{\theta}}_{ww}$, and $\hat{\boldsymbol{\theta}}_{ts}$ denote the directions of the total surface Stokes drift, wind sea, and total swell, respectively. The decomposition is accepted only when the total surface Stokes vector lies inside the positive cone spanned by the wind-sea and swell directions.

Each grid point and time step is then assigned to one of three cases. First, if the wind-sea and swell directions are non-collinear and the solved component magnitudes are both positive, separate wind-sea and swell profiles are reconstructed and summed. Second, if the directions are non-collinear but one solved magnitude is negative, the decomposition is rejected because the available bulk directions are inconsistent with a two-component non-negative closure. This does not imply that one wave component is physically absent. Rather, it indicates that the bulk ERA5 partition directions and the total ERA5 surface Stokes drift vector are mutually inconsistent under a two-direction, non-negative closure. Such inconsistency can arise because total swell may combine several swell systems, because partition-mean directions are bulk energy-weighted summaries, or because the surface Stokes drift is computed from the full spectrum rather than from only two idealised directional components. Third, if the two directions are nearly collinear, defined here by an angular separation smaller than $5^\circ$, the decomposition is not attempted because the linear system is ill-conditioned.

For the rejected and collinear cases, we apply a total-sea fallback reconstruction using the ERA5 total surface Stokes drift and the bulk total-sea transport estimate. This fallback is introduced to avoid spatially intermittent missing forcing in ICON, not to imply that the total-sea profile is physically equivalent to a successful wind-sea/swell decomposition. It is most defensible when wind sea and swell propagate in similar directions. It is less informative for opposing or strongly mixed systems, where the true Stokes profile may rotate with depth and cannot be represented by a single-direction Breivik-type profile.

We therefore distinguish high-confidence decomposed profiles from low-confidence fallback profiles. Their influence should be evaluated explicitly by mapping the frequency of successful decomposition, collinear fallback, and non-collinear rejected fallback, and by performing sensitivity experiments.

In addition to a control run without Stokes forcing and a default run using decomposed profiles plus fallback where needed, a high-confidence-only experiment can exclude or damp the fallback regions. If the difference between the default and high-confidence-only experiments is small compared with the full Stokes-forcing response, then the main modelled conclusions are not controlled by the fallback treatment. If the difference is large, the fallback must be reported as a leading methodological uncertainty.

# Sensitivity Experiment Design

| Experiment | Stokes-profile treatment | Main purpose | Key comparison |
|---|---|---|---|
| `CTRL` | No Stokes drift profile forcing | Baseline ICON simulation without Stokes forcing | Reference state |
| `EXP_DEFAULT` | Use decomposed wind-sea + total-swell profiles where admissible; use total-sea fallback where decomposition is rejected or collinear | Main practical experiment with spatially complete Stokes forcing | `EXP_DEFAULT - CTRL` gives the full modeled response to the proposed Stokes forcing |
| `EXP_HICONF` | Use decomposed profiles only where the non-negative wind-sea/swell decomposition is admissible; exclude or damp forcing in low-confidence fallback regions | Tests whether the fallback regions control the modeled response | `EXP_DEFAULT - EXP_HICONF` isolates the influence of fallback treatment |
| `EXP_TOTAL` | Use total-sea Breivik-type reconstruction everywhere, without wind-sea/swell decomposition | Tests whether the proposed decomposition changes the result relative to a conventional bulk reconstruction | `EXP_DEFAULT - EXP_TOTAL` isolates the added effect of directional decomposition |

The sensitivity experiments are summarized in Table X. The comparison between `EXP_DEFAULT` and `CTRL` quantifies the total impact of the proposed Stokes forcing, while `EXP_DEFAULT - EXP_HICONF` isolates the contribution from low-confidence fallback regions. If the latter is small relative to the full Stokes-forcing response, the main conclusions are not controlled by the fallback treatment. The `EXP_TOTAL` experiment further tests whether the wind-sea/swell decomposition produces a materially different response from a conventional total-sea reconstruction.
