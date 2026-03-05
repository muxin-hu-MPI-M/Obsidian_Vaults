---
tags:
  - ICON
Last Eddited: 2025-11-20
---
The general information about the ICON-output

# ICON Horizontal Grids: R2 family

check the table (Table 1 in [Giorgetta et al. (2018)](https://10.1029/2017MS001242))

![[Screenshot 2025-11-14 at 10.21.29.png]]

- Both Atmosphere and ocean use the same grid setting name
    - e.g.,ICON-XPP (coupled): atmosphere: r2b5; ocean:r2b7

# ICON Vertical Grid

Four options: L40, L64, L72, L128

*Notice: `dzlev_m` is the layer thickness

```shell
#--- dzlev_m: layer thickness ---#
if [ ${LEV} == L40 ] ; then
    n_zlev=40
    dzlev_m="12.,10.,10.,10.,10.,10.,13.,15.,20.,25.,30.,35.,40.,45.,50.,55.,60.,70.,80.,90.,
         100.,110.,120.,130.,140.,150.,170.,180.,190.,200.,220.,250.,270.,300.,350.,400.,
         450.,500.,500.,600."
    levidx_100m=9
    levidx_200m=12
    levidx_2000m=30
    minVerticalLevels=2

elif [ ${LEV} == L64 ] ; then
    n_zlev=64
    dzlev_m="12.,10.,10.,10.,10.,10.,10.,10.,10.,10.,11.,12.,13.,14.,15.,16.,17.,18.,20.,22.,
         24.,26.,28.,30.,32.,35.,38.,41.,45.,49.,53.,58.,62.,66.,71.,75.,80.,85.,91.,97.,
         104.,111.,118.,125.,132.,138.,145.,152.,160.,167.,175.,182.,188.,195.,201.,208.,
         213.,219.,224.,230.,235.,241.,250.,260."
    levidx_100m=10
    levidx_200m=17
    levidx_2000m=46
    minVerticalLevels=2

elif [ ${LEV} == L72 ] ; then
    n_zlev=72
    dzlev_m="2.0,2.2,2.5,2.8,3.1,3.5,3.9,4.4,4.9,5.4,5.9,6.4,7.1,7.7,8.4,9.2,10.1,11.0,
           12.0,13.2,14.4,15.7,17.1,18.7,20.4,22.3,24.3,26.5,28.9,31.5,34.3,37.3,40.6,
           43.1,45.3,46.8,48.4,50.0,51.7,53.4,55.2,57.0,58.9,60.8,62.9,66.6,72.6,80.6,
           90.6,100.2,110.0,120.3,128.7,137.4,146.4,155.7,165.2,174.8,184.4,194.1,203.6,
           212.9,221.9,230.5,238.5,245.9,252.4,258.1,262.8,266.4,268.9,270.1"
    levidx_100m=15
    levidx_200m=25
    levidx_2000m=55
    minVerticalLevels=10

elif [ ${LEV} == L128 ] ; then
    n_zlev=128
    dzlev_m="2.0, 2.1, 2.2, 2.3, 2.4, 2.5, 2.6, 2.7, 2.8, 3.0, 3.1, 3.2,\\
                 3.4, 3.5, 3.7, 3.9, 4.0, 4.2, 4.4, 4.6, 4.8, 5.0, 5.3, 5.5,\\
                 5.8, 6.0, 6.3, 6.6, 6.9, 7.2, 7.5, 7.8, 8.2, 8.5, 8.9, 9.3,\\
                 9.8, 10.2, 10.7, 11.1, 11.5, 11.9, 12.3, 12.7, 13.1, 13.5,\\
                14.0, 14.5, 14.9, 15.4, 15.9, 16.5, 17.0, 17.6, 18.2, 18.8,\\
                19.4, 20.0, 20.7, 21.4, 22.1, 22.8, 23.6, 24.4, 25.2, 26.0,\\
                26.9, 27.8, 28.7, 29.7, 30.6, 31.7, 32.7, 33.8, 34.9, 36.1,\\
                37.3, 38.5, 39.8, 41.1, 42.5, 43.9, 45.3, 46.8, 48.4, 50.0,\\
                51.7, 53.4, 55.2, 57.0, 58.9, 60.8, 62.9, 64.9, 67.1, 69.3,\\
                71.6, 74.0, 76.5, 79.0, 81.6, 84.3, 87.1, 90.0, 93.0, 96.1,\\
                99.3, 102.6, 106.0, 109.5, 113.2, 116.9, 120.8, 124.8, 128.9,\\
               133.2, 137.6, 142.2, 146.9, 151.8, 156.9, 162.1, 167.4, 173.0,\\
               178.7, 184.7, 190.8, 197.1"
    levidx_100m=27
    levidx_200m=37
    levidx_2000m=96
    minVerticalLevels=12
```

# TKE Output

| **Name**         | **Long name**                      | **Physical Meaning**                      | **Unit** |
| ---------------- | ---------------------------------- | ----------------------------------------- | -------- |
| **tke**          | turbulent kinetic energy           | Total turbulent energy per unit mass.     | m2 s-2   |
| **vmix_dummy_1** | vmix_dummy_1                       | -                                         | fixme    |
| **vmix_dummy_2** | vmix_dummy_2                       | -                                         | fixme    |
| **vmix_dummy_3** | vmix_dummy_3                       | -                                         | fixme    |
| **tke_Tbpr**     | TKE tend bpr                       | TKE tendency due to buoyancy production   | m2 s-3   |
| **tke_Tspr**     | TKE tend spr                       | TKE tendency due to shear production      | m2 s-3   |
| **tke_Tdif**     | TKE tend dif                       | TKE tendency due to diffusion             | m2 s-3   |
| **tke_Tdis**     | TKE tend dis                       | TKE tendency due to dissipation           | m2 s-3   |
| **tke_Twin**     | TKE tend win                       | TKE tendency due to wind forcing          | m2 s-3   |
| **tke_Tiwf**     | TKE tend iwf                       | TKE tendency due to internal wave forcing | m2 s-3   |
| **tke_Tbck**     | TKE tend bck                       | TKE tendency due to background mixing     | m2 s-3   |
| **tke_Ttot**     | TKE tend tot                       | Total TKE tendency                        | m2 s-3   |
| **tke_Lmix**     | TKE mixing length                  | (Kolmogorov’s) Mixing length              | m        |
| **tke_Pr**       | TKE Prandtl number                 | Turbulent Prandtl number                  | -        |
| tke_plc          | TKE Langmuir turbulence            | TKE Langmuir turbulence                   | m2 s-3   |
| wlc              | Langmuir turbulence velocity scale | Langmuir turbulence velocity scale        | m s-1    |
| hlc              | Depth of Langmuir cell             | Depth of Langmuir cell                    | m        |

## TKE Scheme
For the TKE-Scheme in ICON, please refers to [[ICON_Parameterisation#TKE-Scheme (ICON-o)]]
In general, the tendency term referring to the change relative to time:
$$ \begin{equation} \frac{\partial \varepsilon}{\partial t}=\frac{\partial}{\partial z}\bigg( c_E\varepsilon^{1/2}L\frac{\partial \varepsilon}{\partial z}\bigg)+ c_u\varepsilon^{1/2}L\bigg(\frac{\partial \overline{\mathbf u_h}}{\partial z} \bigg)^2-c_b\varepsilon^{1/2}L\frac{\partial \overline{b}}{\partial z}-c_\epsilon\frac{\varepsilon^{3/2}}{L} \end{equation} $$

# ICON-XPP: standard output

## Lists in the config file
```
# Ocean
	[[[output_nml oce_mon]]]
      .hide             = false
      output_grid       = true
      ml_varlist        += group:ocean_monitor

    [[[output_nml oce_def]]]
      .hide             = false
      output_grid       = true
      output_interval   = P1Y                          # production: annual data only
      ml_varlist        += group:oce_default, group:oce_essentials, to, so, u, v

    [[[output_nml oce_upp]]]
      output_grid       = true
      m_levels          = "1...10, 18, 25, 55"         # upper 1 km, in additon 2, 3, 4 km
      ml_varlist += draftave, hi, hs, conc, to, so, ice_u, ice_v, Qtop, Qbot, u, v, mld, mlotst, mlotst10, zos, condep, stretch_c, verticallyTotal_mass_flux_e, HeatFlux_Total, FrshFlux_Runoff, FrshFlux_Precipitation, FrshFlux_Evaporation, FrshFlux_SnowFall, FrshFlux_TotalOcean, FrshFlux_VolumeIce, totalsnowfall, Wind_Speed_10m

    [[[output_nml oce_moc]]]
      output_grid       = true
      ml_varlist        += group:ocean_moc
      
      
[output]
  oce_mon = true
  oce_def = true
  oce_upp = true
  oce_moc = true

  [[fixed]]
    oce_fx = true 
    
# Atmosphere
	[[[output_nml atm_mon]]]
      .hide = false
      ml_varlist  = tas_gmean, rsdt_gmean, rsut_gmean, rlut_gmean, radtop_gmean, prec_gmean, evap_gmean, pme_gmean

    [[[output_nml atm_2d]]]
      .hide = false
      output_grid = true
      ml_varlist  = pres_msl, pres_sfc, t_s, clct, tot_prec_rate, tqv, tqv_dia, tqc_dia, tqi_dia, umfl_s, vmfl_s, sp_10m, t_2m, sob_t, sod_t, sou_t, thb_t, sob_s, sou_s, thb_s, shfl_s, lhfl_s, snow_con_rate, snow_gsp_rate, ice_gsp_rate, qifl_s, qhfl_s, t_seasfc, fr_land, fr_seaice, condhf_ice, meltpot_ice, t_ice, h_ice, albvisdif, albvisdir, albnirdif, albnirdir

    [[[output_nml atm_3d]]]
      .hide = false
      output_grid = true
      ml_varlist  = pres_sfc, pres, geopot, temp, u, v, qv, rh, clc, tot_qc_dia, tot_qi_dia, runoff_s, runoff_g
      ## h_levels = raw('10, 500, 5000') 

    [[[output_nml lnd_mon]]]
      .hide = false

    [[[output_nml lnd_bas]]]
      # ml_varlist = group:jsb_all_basic  #  full basic output
      # - reduced basic output incl. alcc
      ml_varlist += group:jsb_seb_basic,group:jsb_hydro_basic,group:jsb_sse_basic, group:jsb_pheno_basic,group:jsb_rad_basic,group:jsb_alcc_basic


[output]
  atm_mon = true
  atm_3d  = true
  atm_2d  = true
  lnd_mon = true
  lnd_bas = true
```
## Ocean
### `oce_def`
Standard output for ocean default variables

![[Screenshot 2025-12-16 at 09.59.08.png | center]]


### `oce_fx`
This is the file for grid cell information, and masks (e.g., sea_land_mask)

![[Screenshot 2025-12-16 at 10.05.55.png]]


### `oce_moc`
The standard ocean output for meridional overturning circulation, heat/freshwater/salt transports

![[Screenshot 2025-12-16 at 10.11.03.png]]


### `oce_mon`
The output for global variables in (time,) dimension. For example, the AMOC strength at 26 N; the global potential energy, global mean sea surface temperature.

![[Screenshot 2025-12-16 at 10.22.34.png]]


### `oce_upp`
Variables that are important in the upper ocean
![[Screenshot 2025-12-16 at 10.28.31.png]]


## Atmosphere
### `atm_2d`
Two-dimensional output for surface data or column-integrated data, including sea level pressure, total column integrated water vapour.

![[Screenshot 2025-12-16 at 10.31.53.png]]




# Potential output namelist
This section will discuss the potential output variables needed for future simulation when the ICON-Wave is ready
## Start from Research Questions
There are 3 main research questions involved in the 1st phase of my PhD:
1. Which wave-induced processes, including 
	   **(1) Wave-mediated momentum pathways**
		   - Waves mediate how much and through which pathways does wind momentum reach to ocean
	   **(2) Stokes-drift-driven processes**
		   - Waves generate Lagrangian drift that reorganises momentum, tracers and momentum
	   **(3) Wave breaking and associated irreversible processes**
		   - Waves breaking transfers energy and momentum into turbulence, bubbles (i.e., dissipation) that change air-sea exchange efficiency
	
	play the dominant role in the modulation of ~={red}air-sea exchanges=~ in the Peruvian coastal region.
	
> [!Attention]
> - This version is **different from the version recorded in the first panel report**!
> - The main difference lies in the classification of wave-related processes.
>   - Reason of change: Overlap between previous classes and unclear classification standard
> - We should keep this open! Nothing has determined yet!!
	
1. How do these wave-induced modifications affect the Peruvian coastal upwelling system in terms of its ~={red}structure and variability=~
	1. **more specific questions??**
2. What are the relative contributions of locally generated wind waves and remotely generated swells to air-sea exchanges in the Peruvian upwelling system?

## “Subjects” and Required Variables
The subjects (or the “targets” we will mostly focuses) are:
- air-sea exchanges
- structure and variability of upwelling system

Needed **ICON output namelist** (see the full list in [[https://icon-o.gitlab-pages.dkrz.de/icon-o-documentation/0206_output.html](https://icon-o.gitlab-pages.dkrz.de/icon-o-documentation/0206_output.html)]) will be given as below, classified based on different subjects
### Air-sea exchanges
#### Surface Fluxes
- Heat:
	- `HeatFlux_Total/Sensible/Latent` → **uncoupled**??
	- `HeatFlux_LongWave/Shortwave` → for analysis cloud cover
	- `atmos_fluxes_HeatFlux_Total/Sensible/Latent` → should we use this?? → **coupled**
- Momentum (wind stress):
	- `stress_xw/yw` → uncoupled?
	- `atmos_fluxes_stress_xw/yw` → wind stress → **coupled**
	- `bc_top_WindStress_u/v` → what is bc → boundary condition

find the coupling namelist (information) in ``
#### TKE
make sure all related variables are selected; 
- TKE equation: see details in [[ICON_Output_Namelist#TKE Output Table]]
- Stokes-drift: `u_stokes, u3d_stokes, v3d_stokes` 
	- 3d is the profile → Langmuir turbulence (see details in [[Note_Stokes-Drift-Profile#The Shear of the Stokes Drift Profile]]) → Langmuir turbulence term hasn’t been added to the TKE parameterised equation

### Structure and variability of Upwelling System
#### Heat content budget analysis
- Heat content: `heat_content_total/300m/700m`, temp * capacity
- !!!`Tt_had, Tt_vad, Ts_had...`
	- related to the time
	- vertically integrated over one depth (multiplied by a layer thickness) 
	- in a output group 
	- **tendency**: divergence of the flux
	- → ask andrea #presenter/Andrea_Mosso 
	- need to add it `use_Ts_budget = True` in the namelist and write them out
- advection (flux): `uT, vT, wT`

## Air-sea coupled output list
from #presenter/Helmuth_Haak 
```
if [[ "$output_oce_flx" == "yes" ]]; 

then cat >> ${oce_namelist} <<EOF

&output_nml 
	filetype = 5                                      ! output format: 2=GRIB2, 4=NETCDFv2, 5=NETCDFv4 
	output_filename = "${EXPNAME}_oce_flx" 
	filename_format = "<output_filename>_<datetime2>" 
	output_start = "${start_date}"                    ! start date in ISO-format 
	output_end = "${end_date}"                        ! end date in ISO-format 
	output_interval = "P1D"                           ! interval in ISO-format 
	file_interval = "${oce_file_interval}"            ! interval in ISO-format 
	output_grid = .TRUE. 
	mode = 1                                          ! 1: forecast mode (relative t-axis); 2: climate mode 
	operation = 'mean'                                ! mean over output interval 
	include_last = .FALSE. 
	m_levels = "1"                                    ! surface and subsurface level only 
	ml_varlist = 'Qtop', 'Qbot','HeatFlux_Total','HeatFlux_ShortWave','HeatFlux_LongWave','HeatFlux_Sensible','HeatFlux_Latent', 'FrshFlux_Runoff','FrshFlux_Precipitation','FrshFlux_Evaporation','FrshFlux_SnowFall','FrshFlux_TotalOcean','FrshFlux_VolumeIce','totalsnowfall','atmos_fluxes_stress_x','atmos_fluxes_stress_y','atmos_fluxes_stress_xw','atmos_fluxes_stress_yw', 'sea_level_pressure'

/
```


cloud cover, SST, wind stress, LH/SH surface, wind profiles

## Strategy
==30 years of spin up with simplified output (default output), then 20 years of detailed output==
the potential list of “additional” detailed outputs:
1. `oce_tke`: TKE related output (already implemented in the `mux0001_b5b7` configuration)
2. `oce_flx`: air-sea fluxes; Check the above namelist; should I keep on only the surface and subsurface only?
3. `oce_htd`: heat tendency related variables. all 3d variables?
4. `oce_std`: salt tendency related variables
5. `oce_upo`: update the upper ocean output with additional tke, tendency parameters ??

### Namelist for the additional outputs:
```
# additional output
    # TKE related 
    [[[output_nml oce_tke]]]
      .hide             = false
      output_grid       = true
      output_interval   = P1M                          
      ml_varlist        += tke, vmix_dummy_1, vmix_dummy_2, vmix_dummy_3, tke_Tbpr, tke_Tspr, tke_Tdif, tke_Tdis, tke_Twin, tke_Tiwf, tke_Tbck, tke_Ttot, tke_Lmix, tke_Pr, tke_plc, wlc, hlc, u_stokes
    
    # air-sea coupled fluxes
    [[[output_nml oce_flx]]]
      .hide             = false
      output_grid       = true
      output_interval   = P1M                          
      m_levels          = "1"                          # surface and subsurface level only
      ml_varlist        += Qtop, Qbot, HeatFlux_Total, HeatFlux_ShortWave, HeatFlux_LongWave, HeatFlux_Sensible, HeatFlux_Latent, FrshFlux_Runoff, FrshFlux_Precipitation, FrshFlux_Evaporation, FrshFlux_SnowFall, FrshFlux_TotalOcean, FrshFlux_VolumeIce, totalsnowfall, atmos_fluxes_stress_x, atmos_fluxes_stress_y, atmos_fluxes_stress_xw, atmos_fluxes_stress_yw, sea_level_pressure

    # heat/salinity tendency (i.e., divergence of flux to diff. components)
    [[[output_nml oce_bgt]]]
      .hide             = false
      output_grid       = true
      output_interval   = P1M
      ml_varlist        += Tt_had, Tt_vad, Tt_hdf, Tt_vdf, Tt_idf, Tt_sur, Tt_srf, Tt_tot, Ts_had, Ts_vad, Ts_hdf, Ts_vdf, Ts_idf, Ts_sur, Ts_srf, Ts_tot

    # heat/salinity advection
    # this will not be turned on since r2b7 has not resolve eddy
    [[[output_nml oce_adv]]]
      .hide             = false
      output_grid       = true
      output_interval   = P1M
      ml_varlist        += uT, uS, vT, vS, wT, wS

    # updated upper ocean variables
    [[[output_nml oce_upo]]]
      .hide             = false
      output_grid       = true
      output_interval   = P1M
      m_levels          = "1...10"         # upper 1 km
      ml_varlist        += draftave, tos, sos, mld, mlotst, mlotst10, zos, condep, stretch_c, verticallyTotal_mass_flux_e, heat_content_total, heat_content_300m, heat_content_700m, to, so, rho, rhopot, mass_flux, u, v, w

```


# Heat/salt budget Output Table

| Name   | Long name                     | physical meaning                                  | Unit    |
| ------ | ----------------------------- | ------------------------------------------------- | ------- |
| Tt_had | temp. tend. hor. adv.         | horizontal advection term                         | K s-1 m |
| Tt_vad | temp. tend. vert. adv.        | vertical advection term                           | K s-1 m |
| Tt_hdf | temp. tend. hor. diff.        | horizontal diffusion term                         | K s-1 m |
| Tt_vdf | temp. tend. vert diff.        | vertical diffusion term                           | K s-1 m |
| Tt_idf | temp. tend. impl. diff.       | Implicit diffusion term                           | K s-1 m |
| Tt_sur | temp. tend. surface           | surface tendency (net change from surface fluxes) | K s-1 m |
| Tt_srf | temp. tend. surface refractor | surface refractor                                 | K s-1 m |
| Tt_tot | temp. tend. total             | total tendency                                    | K s-1 m |
Same for the salt tendency output

## Function: conservation equation:
Take the heat as an example:
$$
\frac{\partial T}{\partial t}=-\nabla \cdot (\mathbf{u}T)+ \nabla \cdot (K\nabla T)+Q_{\text{surface}}
$$
where:
- $T$: temperature
- $\mathbf{u}=(u,v,w)$: velocity field
- $K$: diffusion tensor (mixing)
- $Q_{\text{surface}}$: surface heat source (converted into temperature tendency)

In general, locally:
$$
\frac{\partial T}{\partial t}=-\nabla \cdot \mathbf{F}
$$
where $\mathbf{F}$ is a heat/temperature flux

> [!Important]
> A temperature tendency is mathematically the convergence of a temperature/heat flux

### Decompositions
The model store **layer-integrated tendencies**, in the unit of $K\;s^{-1}\;m$
In general:
$$
Tt_{tot} = Tt_{had} + Tt_{vad} + Tt_{hdf} + Tt_{vdf} + Tt_{idf} + Tt_{sur} + Tt_{srf}
$$
if the term is <font color="#ff0000">positive</font>, then it tend to <font color="#ff0000">increase</font> the heat inside the cell
#### Horizontal advection
Tendency:
$$
Tt_{had}=-(\frac{\partial (uT)}{\partial x}+\frac{\partial (vT)}{\partial y}) \times \Delta z
$$ 
#### Vertical advection
Tendency:
$$
Tt_{vad}=-(\frac{\partial (wT)}{\partial z})\times \Delta z
$$
#### horizontal diffusion
represents the lateral mixing:
the flux:
$$
\mathbf F_{hdf} = -K_h \nabla_h T
$$
Tendency:
$$
Tt_{hdf} = \nabla_h \cdot (K_h\nabla_hT)\times \Delta z
$$

#### Vertical diffusion
tendency (vertical gradient of vertical turbulent mixing):
$$
Tt_{vdf}=\frac{\partial}{\partial z}(K_v\frac{\partial T}{\partial z})\times \Delta z
$$

#### Implicit diffusion
Numerical/implicit mixing from solver stability schemes. Mathematically similar to diffusion:
$$
Tt_{idf}=\nabla \cdot (K_{impli}\nabla T)\times \Delta z
$$
but arises from:
- implicit time stepping
- numerical stabilisation

#### Surface tendency
heating/cooling from surface fluxes:
$$
Tt_{sur}=\frac{Q_{net}}{\rho c_p \Delta z}\times \Delta z = \frac{Q_{net}}{\rho c_p}
$$
where:
- $Q_{net}$: net surface heat flux

#### Surface refractor
model-specific correction term