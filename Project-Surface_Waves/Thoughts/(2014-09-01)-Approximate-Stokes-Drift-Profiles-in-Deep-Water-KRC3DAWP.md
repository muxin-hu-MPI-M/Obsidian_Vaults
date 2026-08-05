---
tags: []
parent: 'Approximate Stokes Drift Profiles in Deep Water'
collections:
    - '2025 Ocean Waves'
    - 'Suface waves'
    - Parameterisation
$version: 50352
$libraryID: 1
$itemKey: KRC3DAWP

---
# <span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(238, 249, 253)">(2014-09-01) Approximate Stokes Drift Profiles in Deep Water</span></span>

| <!-- --> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(219, 238, 221)">Author:</span></span>**<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(219, 238, 221)"> Øyvind Breivik; Peter A. E. M. Janssen; Jean-Raymond Bidlot;</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(243, 250, 244)">Journal: (Publication Date: 2014-09-01)</span></span>**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(219, 238, 221)">Journal Tags:</span></span>**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(243, 250, 244)">Local Link: </span></span>**<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(243, 250, 244)"><a href="zotero://open-pdf/0_AJZ5VNYC" rel="noopener noreferrer nofollow">Breivik et al. - 2014 - Approximate Stokes Drift Profiles in Deep Water.pdf</a></span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(219, 238, 221)">DOI: </span></span>**<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(219, 238, 221)"><a href="https://doi.org/10.1175/JPO-D-14-0020.1" rel="noopener noreferrer nofollow">10.1175/JPO-D-14-0020.1</a></span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(243, 250, 244)">Abstract: </span></span>***<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(243, 250, 244)">A deep-water approximation of the Stokes drift velocity profile is explored as an alternative to the monochromatic profile. The alternative profile investigated relies on the same two quantities required for the monochromatic profile, namely, the Stokes transport and the surface Stokes drift velocity. Comparisons with parametric spectra and profiles under wave spectra from the Interim ECMWF Re-Analysis (ERA-Interim) and buoy observations reveal much better agreement than the monochromatic profile even for complex sea states. That the profile gives a closer match and a more correct shear has implications for ocean circulation models since the Coriolis–Stokes force depends on the magnitude and direction of the Stokes drift profile, and Langmuir turbulence parameterizations depend sensitively on the shear of the profile. The alternative profile comes at no added numerical cost compared to the monochromatic profile.</span></span>* |
| **<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(219, 238, 221)">Tags:</span></span>**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(243, 250, 244)">Note Date: </span></span>**<span style="color: rgb(25, 60, 71)"><span style="background-color: rgb(243, 250, 244)">05/07/2025, 14:19:33</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |


## <span style="color: rgb(224, 255, 255)"><span style="background-color: rgb(102, 205, 170)">📜 Research Key Point</span></span>

***

### ⚙️ Summary

*   A alternative to the monochromatic profile (of Stokes drift) in deep water is explored;

*   The alternative profile invesigated relies on the same two quantities required for the monochromatic profile, namely,

    *   Stokes transport
    *   surface Stokes drift velocity

*   The alternative profile gives a closer match and more correct shear

    *   \--> has implications for ocean circulation models, since the <span style="background-color: rgba(255, 212, 0, 0.5)">Coriolis-Stoles force and the Langmuir turbulence parameterizations depends on the Stokes drift profile</span>

*   The alternative profile comes at no added numerical cost compared to monochromatic profile

### 💡 Background

*   Inclusion of Langmuir Turbulence and Coriolis-Stokes force requries to model the magnitude and the shear of the Stokes drift velocity correctly

*   Stokes drift profiles are also needed when estimating the drift of partially or entirely submerged objects

*   Computing Stokes drift is expensive since it involves evaluating an integral with the 2D wave spectrum at every desired vertical level; Impractical when the full 2D wave spectrum not available

    *   Reason why replace te full Stokes drift velocity profile by a monochromatic profile matched to the transport and the surface Stokes velocity

*   <span style="background-color: rgba(255, 212, 0, 0.5)">The monochromatic profile have problems:</span>

    *   **drift velocity shear under a broad spectrum is much stronger** than that of a monochromatic wave of intermediate wavenumber due to the presence of short waves whose associated Stokes drift quickly vanishes with depth.
    *   **deep Stokes drift profile will be stronge**r than that of a monochromatic wave since the low-wavenumber components penetrate much deeper
    *   **Thus, it is of interest to investigate profiles that exhibit stronger shear at the surface and stronger deep drift**

*   <span style="background-color: rgba(255, 212, 0, 0.5)">The new proposed Stokes drift profile has advantages:</span>

    *   The computation follows the same procedure as estimating a monochromatic profile

    *   The proposed Stokes drift profile mimics the effect of a broader spectrum where the long-wave components penetrate deeper than the mean wavenumber component while the short waves only affect the upper part of the water column.

        *   robust, easy to implement, computationally inexpensive

        *   relies on the same two parameters required for monochromatic profile:

            *   surface Stokes drift velocity  
            *   the Stokes transport

    *   Is recently implemented in ECMWF’s implementation of the Nucleus for European Modelling of the Ocean (NEMO)

### 🧩 Main Content

*   The shear of the Stokes drift profile:

    *   The production of Langmuir turbulence arises from a vortex force term in the momentum equation
    *   The term involving the Reynolds stresses multiplied by the gradient in Stokes drift velocity yi represents the production of Langmuir turbulence
    *   The shear of the Phillips spectrum goes to infinity on the surface, in contrast to spectrum under both monochromatic wave and exponential wave, which remains bounded near the surface. In practice, it may be necessary to cap the Stokes drift shear near the surface when estimating the Langmuir turbulence when assuming a tail proportional to f^-5

*   High-frequency contribution to the profile

    *   high-frequency (short waves) cutoff  $f_c$

    *   Both the Stokes drift profile and the transports is significant beyond the high frequency cutoff, a cutoff used when computing profiles and transport from discretised wave spectra

        *   above the  $f_c$ , one should assume a tail  $F_{HF}$

    *   diagnostic cutoff  $f_d$ , once above  $f_d$ , wave model treats the spectrum diagnostically, replacing it to the prognostic tail  $F_{HF}$

    *   Additional high-frequency contribution to the Surface Stokes drift velocity the the transport are significant, summarized in formula

*   Modelled profiles in the North Atlantic:

    *   Comparing the two profiles from (1) monochromatic wave (2) exponential integral to the ERA-Interim reanalyis (using wave model) on wave field on NA.
    *   for a choosing location, the exponential integral profile match so well to the full profile, but poor performance in the case where a strong swell system is superimposed on the locally generated wind sea, but exponential profile still performs better than the monochromatic

### 📜 Conclusion

## <span style="color: rgb(32, 178, 170)"><span style="background-color: rgb(175, 238, 238)">🔁 Research Details</span></span>

***

### 💧 Data

### 👩🏻‍💻 Method

*   Investigate the Stokes drift velocity in deep water limit (i.e., when the water depth is much greater than the wave wavelength:)

    $$
    \omega^2=gktanh(kh)\approx gk
    $$

    as when water depth (h) is much greater than the wavelength, we will have

    $$
    h \gg \frac{\lambda}{2}\quad or\quad kh \gg 1 \quad \rightarrow tanh(kh)\approx 1
    $$

    Given that the dispersion has the relationship:

    $$
    k=(2\pi f)^2/g
    $$

    With this we can have the vertical **profile of stokes drift** (vs(z)). And if integrated from minus infinity to the surface (z=0), we can have the **Stokes Transport** Vs

*   It is possible to estimate the full profile of Stokes Drift when modelled or observed wave spectrum is available, but costly

*   When a wave spectrum is not available, **<span style="background-color: rgba(255, 212, 0, 0.5)">the profile must be approximated from the Stokes transport and the surface Stokes drift velocity</span>**:

    *   It is common to approximate the profile by the exponential profile of a monochromatic wave (Eq.(7)); With that, the wavenumber must be determined by Eq.(8)
    *   <span style="background-color: rgba(255, 212, 0, 0.5)">But the monochromatic profile have a weaker vertical gradient near the surface, whereas it tends too quickly to zero deeper down</span>

### 🔬 Innovation

*   The behavior of the profile under a full spectrum is most readily investigated by considering the Phillips spectrum, applicable to the equilibrium range of the spectrum of wind-generated waves above the spectral peak.

    *   The Stokes drift profile and the Stokes transport under the Phillips’s spectrum of wind generated waves can be found analytically;

    *   **Near the surface** the term involving the error function becomes vanishinigly small, hence we will have:

        $$
        v_p(z)=(2\alpha_p g / \omega_p)e^{2k_pz}
        $$

    *   **For large z**, the erroe function is substituted by the asymptotic expansion, get:

        $$
        v_p(z)\approx \alpha_pg^2(e^{2k-pz}/2\omega_p^3|z|)
        $$

    *   <span style="background-color: rgba(255, 102, 102, 0.5)">Motivated by this, the author explore a </span>**<span style="background-color: rgba(255, 102, 102, 0.5)">profile that approaches the expoential shape near the surface and goes like the asymptotic soilution in the deep</span>**

        *   The estimated profile is (Eq. (16)), the coefficients was found to minimize the MSE for the Phillips spectrum
        *   The Stokes transport can be solved analytically
        *   the corresponding wavenumber is approximately of 1/3 of the wavenumber of monochromatic profle

## <span style="color: rgb(0, 77, 153)"><span style="background-color: rgb(135, 206, 250)">🤔 Personal Summary</span></span>

***

### 🙋‍♀️ Key Points

*   **Stokes drift:**\
    is the <span style="background-color: rgba(255, 212, 0, 0.5)">net movement of fluid particles caused by the </span>**<span style="background-color: rgba(255, 212, 0, 0.5)">presence of surface gravity waves</span>**, typically in the ocean.

    *   <span style="color: inherit"><span style="background-color: rgba(0, 0, 0, 0)">resulting in a difference between the wave-averaged velocity of a particle (Lagrangian) and the velocity in a fixed reference frame (Eulerian)</span></span>.<span style="color: rgb(0, 29, 53)"><span style="background-color: rgb(255, 255, 255)"> </span></span>

    *   **Important for the generation of Langmuir turbulence**, as it is resulted from interactions between the Stokes drift (from surface waves) and wind-driven shear currents in the ocean’s surface boundary layer.

        *   Thus, with the inclusion of Langmuir turbulence in Eulerian ocean model, it is important to model the magnitude and the shear of the *Stokes drift velocity*

*   Parameterisation of Coriolis-Stokes force and Langmuir turbulence depend sensitively on the shear of the Stokes drift velocity profile

*   The use of monochromatic profile is problematic:

    *   The profile has a weaker vertical gradient (than the profile under a full spectrum) near the surface\
        -> i.e., weaker shear near the surface
    *   Vanishes too quickly to zero deeper down\
        -> i.e., ‘shallower’ Stokes drift into deeper layer

*   <span style="background-color: rgba(255, 102, 102, 0.5)">the author explore a </span>**<span style="background-color: rgba(255, 102, 102, 0.5)">profile that approaches the expoential shape near the surface and goes like the asymptotic soilution in the deep</span>**

    *   The estimated profile is (Eq. (16)), the coefficients was found to minimize the MSE for the Phillips spectrum
    *   The Stokes transport can be solved analytically
    *   the corresponding wavenumber is approximately of 1/3 of the wavenumber of monochromatic profle

*   The production of Langmuir turbulence arises from a vortex force term in the momentum equation:

    $$
    v_s \times \nabla \times \bold u
    $$

    It is assumed that the vortex force gives rise to a term involving the shear of the Stokes drift velocity profile in the TKE (although it is still unclear wheather this effect will be strong enought to explain the observed Langmuri circulation). The TKE equation with a Stokes dirft sheaar term can be written as (Eq. (23))

### 📌 Expectation

### 💭 Your Thinking

<a href="./Note-on-Surface-Wave-DJN2YIN5.md" rel="noopener noreferrer nofollow" zhref="zotero://note/u/DJN2YIN5/" ztype="znotelink" class="internal-link">Note on Surface Wave</a>
