---
tags:
  - wave/surface_wave
  - project/surfwaves
  - stokes_drift
  - ICON/experiment
Last Eddited: 2026-05-21
---

# Total-Sea Stokes Profile Reconstruction Method
## Method Framing
This experiment evaluates the climate-scale impact of adding a reconstructed Stokes drift profile to ICON. The target experiment is named `STOKES_TOTAL`, and it is compared with a control simulation, `CTRL`, in which no Stokes drift profile forcing is applied. The purpose of `STOKES_TOTAL` is to test whether a physically plausible bulk Stokes profile modifies the simulated ocean state at climate-relevant scales.

The vertical Stokes drift profile is reconstructed from ERA5 wave reanalysis using the Phillips-spectrum approximation of Breivik et al., 2016. This profile is used because it improves on the monochromatic profile and on the earlier exponential-integral approximation of Breivik et al., 2014 especially in representing the near-surface shear of the Stokes drift profile. A practical advantage of the Phillips profile is that it requires the same two bulk quantities as the monochromatic approximation: the surface Stokes drift velocity and the Stokes transport. It is therefore no more expensive to implement in an ocean model, while providing a more realistic broadband profile shape for a wide range of sea states, including swell-dominated cases.

The Stokes transport is defined as the depth integral of the Stokes drift profile,
$$
\mathbf{V}_s=\int_{-\infty}^{0}\mathbf{u}_s(z)\,dz ,
$$
where $\mathbf{u}_s(z)$ is the Stokes drift velocity at depth $z$. ERA5 provides the total surface Stokes drift vector, $\mathbf{u}_{s0}$, directly. The Stokes transport is not used as a direct ERA5 output here, and is instead estimated from bulk total-sea wave parameters as
$$
\mathbf{V}_s^{bulk} \approx \frac{2\pi}{16}\bar{f}H_{m0}^{2}\hat{\mathbf{k}},
$$
where $H_{m0}=4\sqrt{m_0}$ is the significant wave height, $\bar{f}=m_1/m_0$ is the first-moment mean frequency, and $\hat{\mathbf{k}}$ is the mean wave propagation direction. In the one-dimensional profile reconstruction, the magnitude $V_s=|\mathbf{V}_s^{bulk}|$ is used to set the depth scale of the profile, while the horizontal direction of the reconstructed profile is prescribed by the ERA5 surface Stokes drift vector. This defines a collinear bulk closure: the profile matches the ERA5 surface Stokes drift and uses the estimated total-sea transport magnitude to constrain its vertical integral.

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
where $\mathbf{v}_0$ is the surface Stokes drift vector, $\beta$ is a profile parameter, and $\overline{k}$ is an inverse depth scale. For a Phillips spectrum, $\beta=1$. Breivik et al. \citep{Breivik2016} showed that $\beta=1$ gives good agreement between modeled and parameterized Stokes drift profiles. The inverse depth scale is determined from the surface Stokes drift magnitude, $v_0=|\mathbf{v}_0|$, and the Stokes transport magnitude, $V_s=|\mathbf{V}_s^{bulk}|$, as

$$
\overline{k}=
\frac{v_0}{2V_s}
\left(1-\frac{2\beta}{3}\right).
$$

In `STOKES_TOTAL`, $\mathbf{v}_0=\mathbf{u}_{s0}$ is the ERA5 total surface Stokes drift vector, and $V_s$ is estimated from total-sea bulk wave parameters. The resulting profile is a one-dimensional approximation that is consistent with the prescribed surface Stokes drift and the estimated bulk transport magnitude.

## Rationale For The Total-Sea Closure
The total-sea reconstruction is selected because it gives a physically motivated bulk profile with a clear and limited set of assumptions. The Phillips-spectrum profile is better suited to broadband sea states than a monochromatic profile, and previous tests showed that it performs well for modeled open-ocean spectra and measured spectra. This makes it a defensible choice when the aim is to include Stokes drift effects in an ocean model without explicitly integrating the full two-dimensional wave spectrum at every grid point and time step.

The method should be understood as a one-dimensional reconstruction of an intrinsically directional wave effect. It uses the ERA5 surface Stokes drift to set the direction and surface value of the profile, and it uses the estimated total-sea Stokes transport magnitude to set the profile depth scale. This closure does not claim to resolve the full directional structure of the wave spectrum. Instead, it provides a compact approximation that preserves the two bulk constraints most directly needed by the (Breivik et al., 2016) profile.

This choice is appropriate for the present study because the research question concerns the climate-scale influence of adding a plausible Stokes profile forcing to ICON. The objective is not to attribute the modelled response to individual wave systems, but to test whether Stokes-profile forcing produces a detectable and interpretable ocean response. For this purpose, a total-sea reconstruction provides a method that is more physically informed than a monochromatic profile while remaining simple, transparent, and feasible for long climate-scale simulations.

## Limitations
The total-sea method remains an approximation. The main limitation is that it represents a full two-dimensional wave spectrum by a single one-dimensional Stokes drift profile. In reality, the direction of the surface Stokes drift and the direction of the Stokes transport need not be identical. The surface Stokes drift is strongly weighted toward short waves and can therefore be influenced by local wind sea, whereas the transport and the deeper part of the profile can receive a larger contribution from longer swell. Previous studies have noted that mean wave direction can be a better proxy for the transport direction than the surface Stokes drift direction, while multidirectional wave spreading and opposing wind-sea and swell systems can substantially modify individual profiles.

The present reconstruction cannot represent such vertical directional structure. In mixed sea states, especially when wind sea and swell propagate in different or nearly opposing directions, the true Stokes drift vector may rotate with depth. A single parametric profile cannot capture this behaviour, and it also cannot separate wind-sea and swell contributions to the modelled ocean response. These are known limitations of using a one-dimensional bulk Stokes profile rather than a full spectral reconstruction.

We retain this method because the limitation is explicit and tolerable for the primary climate-scale question. The Phillips-spectrum profile gives a substantially better one-dimensional approximation than simpler bulk profiles, requires only surface Stokes drift and transport, and can be applied consistently with ERA5 bulk diagnostics. The resulting experiment should therefore be interpreted as the response to a scientifically defensible bulk Stokes forcing, not as a complete representation of all directional wave effects. The unresolved directional complexity should be discussed as a methodological limitation and, where relevant, assessed with complementary diagnostics or sensitivity tests.

## Target Experiment

The primary experiment is:
- `STOKES_TOTAL`: ICON simulation forced with the globally reconstructed total-sea Stokes drift profile.
The reference experiment is:
- `CTRL`: ICON simulation without Stokes drift profile forcing.
The main modelled response is therefore evaluated as
$$
STOKES\_TOTAL - CTRL .
$$
This comparison isolates the effect of adding the total-sea reconstructed Stokes profile to ICON under a bulk one-dimensional Stokes forcing strategy.


# Manuscript: Methodology-Total-Sea Stokes Drift Profile Reconstruction

We reconstruct the vertical Stokes drift profile from ERA5 wave reanalysis using the Phillips-spectrum approximation of Breivik et al. 2016. The target experiment, STOKES_TOTAL, is compared with a control simulation, CTRL, in which no Stokes drift profile forcing is applied. The aim is to test whether adding a physically plausible bulk Stokes profile affects the simulated ocean state at climate-relevant scales.

The Phillips-spectrum profile is used because it provides a more realistic broadband approximation than a monochromatic profile and improves the representation of near-surface Stokes shear compared with earlier bulk profiles (Breivik et al., 2014, 2016). It also requires only two bulk constraints: the surface Stokes drift velocity and the Stokes transport. This makes it suitable for long ICON simulations in which the full two-dimensional wave spectrum is not explicitly integrated.

The Stokes transport is defined as

$$ \mathbf{V}_s=\int_{-\infty}^{0}\mathbf{u}_s(z)\,dz , $$

where $\mathbf{u}_s(z)$ is the Stokes drift velocity at depth $z$. ERA5 provides the total surface Stokes drift vector, $\mathbf{u}_{s0}$, directly. Since the Stokes transport is not used here as a direct ERA5 output, we estimate it from total-sea mean wave quantities as

$$ \mathbf{V}_s^{bulk} \approx \frac{2\pi}{16}\bar{f}H_{m0}^{2}\hat{\mathbf{k}}, $$

where $H_{m0}=4\sqrt{m_0}$ is the significant wave height, $\bar{f}=m_1/m_0$ is the first-moment mean frequency, and $\hat{\mathbf{k}}$ is the mean wave propagation direction. In the one-dimensional reconstruction, the magnitude $V_s=|\mathbf{V}_s^{bulk}|$ is used to determine the profile depth scale, while the profile direction is prescribed by the ERA5 surface Stokes drift vector.

The reconstructed profile is given by

$$ \mathbf{u}_s(z)\approx \mathbf{u}_{s0} \left\{ e^{-2\overline{k}|z|} - \beta\sqrt{2\pi \overline{k}|z|} \left[ \operatorname{erfc}\left(\sqrt{2\overline{k}|z|}\right) \right] \right\}, $$

where $\beta=1$ for the Phillips-spectrum profile, following Breivik et al. \citep{Breivik2016}. The inverse depth scale is

$$ \overline{k}= \frac{u_{s0}}{2V_s} \left(1-\frac{2\beta}{3}\right), $$

where $u_{s0}=|\mathbf{u}_{s0}|$. This formulation constrains the profile by the observed ERA5 surface Stokes drift and by a bulk estimate of the total-sea Stokes transport.

This reconstruction is a one-dimensional approximation to an intrinsically directional wave field. The direction of the surface Stokes drift and the direction of the Stokes transport may differ, especially in mixed wind-sea and swell conditions. The method therefore cannot represent vertical rotation of the Stokes drift vector or separate wind-sea and swell contributions. We retain this approach because these limitations are explicit and acceptable for the present climate-scale experiment: the Phillips-spectrum profile is a defensible bulk approximation, requires only ERA5-available diagnostics, and allows us to test the modeled response to a physically plausible total-sea Stokes forcing. The unresolved directional complexity is treated as a methodological limitation to be discussed separately.