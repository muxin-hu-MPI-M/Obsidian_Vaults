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

# TKE Output Table

|**Name**|**Long name**|**Physical Meaning**|**Unit**|
|---|---|---|---|
|**tke**|turbulent kinetic energy|Total turbulent energy per unit mass.|m2 s-2|
|**vmix_dummy_1**|vmix_dummy_1|-|fixme|
|**vmix_dummy_2**|vmix_dummy_2|-|fixme|
|**vmix_dummy_3**|vmix_dummy_3|-|fixme|
|**tke_Tbpr**|TKE tend bpr|TKE tendency due to buoyancy production|m2 s-3|
|**tke_Tspr**|TKE tend spr|TKE tendency due to shear production|m2 s-3|
|**tke_Tdif**|TKE tend dif|TKE tendency due to diffusion|m2 s-3|
|**tke_Tdis**|TKE tend dis|TKE tendency due to dissipation|m2 s-3|
|**tke_Twin**|TKE tend win|TKE tendency due to wind forcing|m2 s-3|
|**tke_Tiwf**|TKE tend iwf|TKE tendency due to internal wave forcing|m2 s-3|
|**tke_Tbck**|TKE tend bck|TKE tendency due to background mixing|m2 s-3|
|**tke_Ttot**|TKE tend tot|Total TKE tendency|m2 s-3|
|**tke_Lmix**|TKE mixing length|(Kolmogorov’s) Mixing length|m|
|**tke_Pr**|TKE Prandtl number|Turbulent Prandtl number|-|

**For the TKE-Scheme in ICON, please refers to [ICON TKE Parameterisation](https://www.notion.so/ICON-TKE-Parameterisation-29a69691c52b80e7888ce8d81a77edb5?pvs=21)**