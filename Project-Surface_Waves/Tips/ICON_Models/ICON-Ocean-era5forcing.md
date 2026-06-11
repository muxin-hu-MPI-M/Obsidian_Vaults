---
tags:
  - project/surfwaves
  - wave/surface_wave
  - ICON
  - ICON/experiment
Last Eddited: 2026-06-01
---
# Provider: `era5g_omip_runoff_provider.py`
## Parses runtime options
such as dataPath, era5Path, component/grid names, log level, and --dryrun.
the core function:
```Python
def parse_args():
    parser = argparse.ArgumentParser(description="ERA5 OMIP Runoff Provider")
    parser.add_argument(
        "--loglevel",
        default="INFO",
        choices=["DEBUG", "INFO", "WARNING", "ERROR", "CRITICAL"],
        help="Set logging level (default: INFO)",
    )
    parser.add_argument(
        "--dryrun",
        action="store_true",
        help="Enable DRYRUN mode (default: False)",
    )
    parser.add_argument(
        "--dataPath",
        default="/pool/data/ICON/oes/",
        help="Base data path  (default: /pool/data/ICON/oes/)",
    )
    parser.add_argument(
        "--era5Path",
        default=None,
        help="ERA5 forcing path (default: <dataPath>/ERA5_forcing/)",
    )
    parser.add_argument(
        "--componentName",
        default="era5_provider",
        help="Component name (default: era5_provider)",
    )
    parser.add_argument(
        "--era5GridName",
        default="era5_grid",
        help="ERA5 grid name (default: era5_grid)",
    )
    parser.add_argument(
        "--omipGridName",
        default="omip_grid",
        help="OMIP grid name (default: omip_grid)",
    )
    return parser.parse_args()
    
```
- dataPath: default="/pool/data/ICON/oes/"
- era5Path: default="/pool/data/ICON/oes/ERA5_forcing/"

### Dry run?
`--dryrun` means: run the provider in a test/read-only mode without doing the live YAC coupling.
In this file, dry-run changes a few things:
```Python
DRYRUN = args.dryrun
if not DRYRUN:
    from yac import *
```
So with `--dryrun`, it does **not** import yac.

It also skips:
```Python
yac = YAC() 
comp = yac.def_comp(...) 
Field.create(...) 
yac.enddef() 
field.put(...)
```
So it does **not** connect to the coupled ICON/YAC run and does **not** send fields.

Instead, it uses a hardcoded short test interval:
```Python
start_date = '1970-12-30T00:00:00'
end_date = '1971-01-02T00:00:00'
```
Then it still tries to open/read the ERA5 and OMIP runoff input files for that period.

## Provider rank
```Python
if not DRYRUN:
    """
    Initialize YAC and define the ERA5 provider component.
    """
    yac = YAC()

    def_calendar(Calendar.PROLEPTIC_GREGORIAN)
    comp = yac.def_comp(args.componentName)

    # Get the number of provider ranks and the rank of the local process
    num_provider_rank = comp.size
    provider_rank = comp.rank

    if provider_rank == 0:
        logger.info(
            f"""
            Initialized YAC component '{args.componentName}' with
            {num_provider_rank} ranks using {num_threads} threads each
            """
        )
else:
    num_provider_rank = 1
    provider_rank = 0
```

Both `num_provider_rank` and `provider_rank` are YAC/MPI concepts, in both providers, this line creates a YAC component:
```Python
comp = yac.def_comp(args.componentName)
```
Then:
```Python
num_provider_rank = comp.size
provider_rank = comp.rank
```
Meaning:
- num_provider_rank: how many MPI ranks/processes belong to this provider component.
- provider_rank: the local rank number inside this provider component, starting from 0.
So if: `num_provider_rank == 1 provider_rank == 0`, that means: there is only one provider process, and it is rank 0. It handles everything.

For the atmosphere/runoff provider, default component supports multiple provider ranks, The ERA5 atmosphere variables are distributed like this (line 64):
```Python
if i % num_provider_rank == provider_rank:
```
So if num_provider_rank = 2, rank 0 handles variables 0, 2, 4, ..., rank 1 handles 1, 3, 5, .... Runoff is special: only provider_rank == 0 handles it.

# Create runscript: example
```Bash
#omip
for EXP in exp.ocean_era5
do
    RES=R2B7
    LEV=L72
    ID=${RES}${LEV}
    
    cp checksuite.ocean_internal/era5/${EXP} ${EXP}_${ID}

    ./make_target_runscript in_script=${EXP}_${ID} in_script=exec.iconrun EXPNAME=${EXP}_${ID} cpu_time=02:00:00 no_of_nodes=30 openmp_threads=4 account_no=mh0033
    cd $WRK

    sed -i "s/RXBY/${RES}/" ${EXP}_${ID}.run
    sed -i "s/ZZZ/${LEV}/" ${EXP}_${ID}.run
    sed -i "s/NNN/1/" ${EXP}_${ID}.run
    sed -i "s/DNM/1/" ${EXP}_${ID}.run
```
- core function: `./maks_target_runscript` under the `/master/run/` directory


# Inside the runscript (e.g., `exp.ocean_era5_R2B7L72.run`)
## DataPath and era5Path

```Fortran
# Define the number of ERA5 provider processes
ERA5_PROCS=1

cat > ${EXPDIR}/mpmd.conf << EOF
0-$((mpi_total_procs - ERA5_PROCS - 1))   $MODEL
$((mpi_total_procs - ERA5_PROCS))-$((mpi_total_procs - 1))    ${PYTHON} ${basedir}/etc/era5g_omip_runoff_provider.py
#$((mpi_total_procs - ERA5_PROCS))-$((mpi_total_procs - 1))    ${PYTHON} ${basedir}/etc/era5g_omip_runoff_provider.py  --loglevel=DEBUG
EOF

export START_MODEL="srun -l --mem=0  --export=ALL --kill-on-bad-exit=1 --hint=nomultithread  --ntasks=${mpi_total_procs} --ntasks-per-node=${mpi_procs_pernode} --cpus-per-task=${OMP_NUM_THREADS} --multi-prog mpmd.conf"
```

If you want to use a different data location, the right place is the provider launch line in the generated `mpmd.conf` block, for example:
```
$((mpi_total_procs - ERA5_PROCS))-$((mpi_total_procs - 1)) \ ${PYTHON} ${basedir}/etc/era5g_omip_runoff_provider.py \ --dataPath /your/base/path/
```
or more explicitly:
```
`$((mpi_total_procs - ERA5_PROCS))-$((mpi_total_procs - 1)) \ ${PYTHON} ${basedir}/etc/era5g_omip_runoff_provider.py \ --dataPath /your/base/path/ \ --era5Path /your/era5/path/
```

## Executable Python path
```Fortran
# export the python interpreter
export PYTHON="/fastdata/mh0156/buildbot/venv-3.10.10-p24a46/bin/python3"
export PYTHONPATH="/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/externals/mtime/build/python:/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/externals/yac/python::/home/m/m301254/Download/pyicon"
```
it means:
- `PYTHON` is just a shell variable naming **which Python executable to run**.
- `PYTHONPATH` tells Python **where else to look for importable modules**. Python will search the directories in `PYTHONPATH` in addition to its normal package locations. The entries are separated by : on Linux.

For the provider, `PYTHON` chooses the interpreter and package environment, including things like xarray, numpy, isodate, zarr. `PYTHONPATH` adds local builds directories so Python can find ICON-related modules, especially the YAC python bindings, usually through 
- /work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/externals/mtime/build/python
- /work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/externals/yac/python



# Pipeline: introducing 3 wave fields to ICON code
`p_as` is **not ERA5-specific**. It is `TYPE(t_atmos_for_ocean)`, a general “atmosphere/forcing for ocean” container. In this branch, the ERA5 provider fills it when `iforc_oce == era5_provider`.
So for Option A, we use `p_as` as the storage place for your ERA5 wave forcing too. That is practical and consistent.

## Files that contains ‘era5’
```bash
config/mo_ocean_nml.f90:745: INTEGER, PARAMETER :: ERA5_provider             = 13  ! ERA5 from python reader
boundary/mo_ocean_surface_refactor.f90:40:    &  No_Forcing, Analytical_Forcing, OMIP_FluxFromFile, Coupled_FluxFromAtmo,era5_provider,       &
boundary/mo_ocean_surface_refactor.f90:956:    USE mo_ocean_era5_provider_coupling, ONLY : couple_ocean_to_era5_provider
boundary/mo_ocean_surface_refactor.f90:957:    USE mo_coupling_config, only : is_coupled_to_era5
boundary/mo_ocean_surface_refactor.f90:990:    CASE (OMIP_FluxFromFile, era5_provider)         !  12,13      !  Driving the ocean with OMIP fluxes
boundary/mo_ocean_surface_refactor.f90:995:      IF ( is_coupled_to_era5() ) THEN
boundary/mo_ocean_surface_refactor.f90:997:    CALL couple_ocean_to_era5_provider(p_patch_3D, p_as%tafo, &

boundary/mo_ocean_forcing.f90:452:    LOGICAL, DIMENSION(MAX_GROUPS) :: groups_oce_era5
boundary/mo_ocean_forcing.f90:461:    groups_oce_era5 = groups("oce_era5")
boundary/mo_ocean_forcing.f90:470:    CALL add_var(ocean_default_list, 'era5_t2m', p_as%tafo ,&
boundary/mo_ocean_forcing.f90:472:      &          t_cf_var('era5_t2m', 'C', 'era5_t2m', datatype_flt),&
boundary/mo_ocean_forcing.f90:474:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:476:    CALL add_var(ocean_default_list, 'era5_tdew', p_as%ftdew , &
boundary/mo_ocean_forcing.f90:478:      &          t_cf_var('era5_tdew', 'K', 'era5_tdew', datatype_flt),&
boundary/mo_ocean_forcing.f90:480:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:482:    CALL add_var(ocean_default_list, 'era5_wind10', p_as%fu10 , &
boundary/mo_ocean_forcing.f90:484:      &          t_cf_var('era5_wind10', 'm s-1', 'era5_wind10', datatype_flt),&
boundary/mo_ocean_forcing.f90:486:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:488:    CALL add_var(ocean_default_list, 'era5_ustress', p_as%topBoundCond_windStress_u , &
boundary/mo_ocean_forcing.f90:490:      &          t_cf_var('era5_ustress', 'Pa', 'era5_ustress', datatype_flt),&
boundary/mo_ocean_forcing.f90:492:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:494:    CALL add_var(ocean_default_list, 'era5_vstress', p_as%topBoundCond_windStress_v , &
boundary/mo_ocean_forcing.f90:496:      &          t_cf_var('era5_vstress', 'Pa', 'era5_vstress', datatype_flt),&
boundary/mo_ocean_forcing.f90:498:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:500:     CALL add_var(ocean_default_list, 'era5_u10', p_as%u , &
boundary/mo_ocean_forcing.f90:502:      &          t_cf_var('era5_u10', 'm s-1', 'era5_u10', datatype_flt),&
boundary/mo_ocean_forcing.f90:504:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:506:          CALL add_var(ocean_default_list, 'era5_v10', p_as%v , &
boundary/mo_ocean_forcing.f90:508:      &          t_cf_var('era5_v10', 'm s-1', 'era5_v10', datatype_flt),&
boundary/mo_ocean_forcing.f90:510:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:512:          CALL add_var(ocean_default_list, 'era5_ldown', p_as%flwr , &
boundary/mo_ocean_forcing.f90:514:      &          t_cf_var('era5_ldown', 'W m-2', 'era5_ldown', datatype_flt),&
boundary/mo_ocean_forcing.f90:516:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:518:            CALL add_var(ocean_default_list, 'era5_precip', p_as%frshflux_precipitation , &
boundary/mo_ocean_forcing.f90:520:      &          t_cf_var('era5_precip', 'm s-1', 'era5_precip', datatype_flt),&
boundary/mo_ocean_forcing.f90:522:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:524:            CALL add_var(ocean_default_list, 'era5_swdown', p_as%fswr , &
boundary/mo_ocean_forcing.f90:526:      &          t_cf_var('era5_swdown', 'W m-2', 'era5_swdown', datatype_flt),&
boundary/mo_ocean_forcing.f90:528:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:530:           CALL add_var(ocean_default_list, 'era5_runoff', p_as%frshflux_runoff , &
boundary/mo_ocean_forcing.f90:532:      &          t_cf_var('era5_runoff', 'm s-1', 'era5_runoff', datatype_flt),&
boundary/mo_ocean_forcing.f90:534:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:536:            CALL add_var(ocean_default_list, 'era5_slp', p_as%pao , &
boundary/mo_ocean_forcing.f90:538:      &          t_cf_var('era5_slp', 'Pa', 'era5_slp', datatype_flt),&
boundary/mo_ocean_forcing.f90:540:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_forcing.f90:542:            CALL add_var(ocean_default_list, 'era5_tcc', p_as%fclou , &
boundary/mo_ocean_forcing.f90:544:      &          t_cf_var('era5_tcc', '1', 'era5_tcc', datatype_flt),&
boundary/mo_ocean_forcing.f90:546:      &          ldims=(/nproma,alloc_cell_blocks/),in_group=groups_oce_era5)
boundary/mo_ocean_ext_data.f90:272:    ! omip forcing data on cell edge (dont need this with iforc_oce == era5_provider)
boundary/mo_ocean_ext_data.f90:455:    ! dont need this with iforc_oce == era5_provider
```

## Recommended Pipeline
### 0. First adjust Python names
I would make the scalar name ERA5-specific too:
```python
YAC_OUTPUT_FIELDS = [
    ("ust", "era5_zonal_sfc_stokes_drift"),
    ("vst", "era5_meridional_sfc_stokes_drift"),
    ("stokes_transport", "era5_stokes_transport"),
]
```
So all three YAC fields are clearly ERA5 wave forcing fields.

### 1. Add storage in `t_atmos_for_ocean`
File:
```text
/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/src/ocean/boundary/mo_ocean_surface_types.f90
```
Current type is here:w
```fortran
TYPE t_atmos_for_ocean
```
Add three pointer arrays, for example near `u`, `v`, or near wind stress:
```fortran
& era5_zonal_sfc_stokes_drift     (:,:), & ! ERA5 surface Stokes drift, zonal      [m/s]
& era5_meridional_sfc_stokes_drift(:,:), & ! ERA5 surface Stokes drift, meridional [m/s]
& era5_stokes_transport           (:,:), & ! ERA5 Stokes transport magnitude       [m2/s]
```
These are cell-centered 2D fields with shape `(nproma, nblks_c)`, same as other surface forcings.

### 2. Register them with `add_var`
File:
```text
/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/src/ocean/boundary/mo_ocean_forcing.f90
```
Subroutine:
```fortran
construct_atmos_for_ocean
```
Add three `CALL add_var(...)` entries, similar to the existing ERA5 entries around `era5_u10`, `era5_v10`, etc.

Example:
```fortran
CALL add_var(ocean_default_list, 'era5_zonal_sfc_stokes_drift', &
  &          p_as%era5_zonal_sfc_stokes_drift, &
  &          grid_unstructured_cell, za_surface, &
  &          t_cf_var('era5_zonal_sfc_stokes_drift', 'm s-1', &
  &                   'era5_zonal_sfc_stokes_drift', datatype_flt), &
  &          dflt_g2_decl_cell, &
  &          ldims=(/nproma,alloc_cell_blocks/), in_group=groups_oce_era5)
```
Similarly:
```fortran
era5_meridional_sfc_stokes_drift  ! unit: m s-1
era5_stokes_transport             ! unit: m2 s-1
```
Then initialize them to zero in the same subroutine:
```fortran
p_as%era5_zonal_sfc_stokes_drift(:,:)      = 0.0_wp
p_as%era5_meridional_sfc_stokes_drift(:,:) = 0.0_wp
p_as%era5_stokes_transport(:,:)            = 0.0_wp
```
Also add them to the OpenACC `COPYIN` block if this run may use GPU/OpenACC:
```fortran
!$ACC   COPYIN(p_as%era5_zonal_sfc_stokes_drift, p_as%era5_meridional_sfc_stokes_drift) &
!$ACC   COPYIN(p_as%era5_stokes_transport)
```
I would **not** manually add explicit `ALLOCATE/DEALLOCATE` yet unless compile/runtime shows it is needed. The existing `add_var` pattern appears to manage most of these `p_as` arrays.

### 3. Add a YAC receiver
Here I recommend a slightly cleaner version than your step 3.
**Instead of mixing the wave fields into the existing 12-field atmospheric ERA5 receiver, create a separate wave receiver, either:**
```text
src/coupling/mo_ocean_era5_wave_provider_coupling.f90
```
or add separate subroutines inside:
```text
src/coupling/mo_ocean_era5_provider_coupling.f90
```
Recommended new routines:
```fortran
construct_ocean_era5_wave_provider_coupling_post_sync(...)
couple_ocean_to_era5_wave_provider(...)
```
Why separate? Because the wave provider is a separate Python/YAC component:
```text
era5_wave_provider
era5_wave_grid
```
and it should be possible to debug it independently from the existing atmosphere/runoff provider.

This receiver should define and receive exactly:
```text
era5_zonal_sfc_stokes_drift
era5_meridional_sfc_stokes_drift
era5_stokes_transport
```
It should follow the same pattern as:
```text
src/coupling/mo_ocean_era5_provider_coupling.f90
```
but with:
```fortran
INTEGER, PARAMETER :: no_of_fields = 3
```
and three field objects.

### 4. Register the wave coupling fields during YAC setup
File:
```text
/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/src/coupling/mo_ocean_coupling_frame.f90
```
Current ERA5 construct call is around line 131:
```fortran
CALL construct_ocean_era5_provider_coupling_post_sync(...)
```
If you create a separate wave receiver, add a similar call for:
```fortran
CALL construct_ocean_era5_wave_provider_coupling_post_sync(...)
```
This is the ICON-side declaration of “the ocean component has target fields with these names”.

### 5. Call the receiver during the ocean forcing update
File:
```text
/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/src/ocean/boundary/mo_ocean_surface_refactor.f90
```
Existing ERA5 receive call is around:
```fortran
CALL couple_ocean_to_era5_provider(...)
```
Add a second call nearby:
```fortran
CALL couple_ocean_to_era5_wave_provider( &
  p_patch_3D, &
  p_as%era5_zonal_sfc_stokes_drift, &
  p_as%era5_meridional_sfc_stokes_drift, &
  p_as%era5_stokes_transport, &
  lacc=lzacc)
```

Yes, an `IF` statement is a good idea, for example only call it when the ERA5 wave provider is enabled. At first, you can key it off the same ERA5 coupling condition for testing, but eventually it would be nicer to have a separate switch like `is_coupled_to_era5_wave()`.

### 6. Modify the runscript YAC YAML
File:

```text
/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/master/run/exp.ocean_era5_R2B7L72_test.run
```

Add component config:

```yaml
era5wave2ocn: &era5wave2ocn
  src_component: era5_wave_provider
  src_grid: era5_wave_grid
  src_lag: 0
  tgt_component: oce
  tgt_grid: icon_ocean_grid
  tgt_lag: 0
  mapping_side: target
  <<: [ *time_config ]
```

Add coupling entry:

```yaml
- <<: [ *era5wave2ocn, *nnn_fixed ]
  coupling_period: PT1H
  field:
    - era5_zonal_sfc_stokes_drift
    - era5_meridional_sfc_stokes_drift
    - era5_stokes_transport
```

You may later choose a better interpolation stack, but `nnn_fixed` is a reasonable first test.

### 7. Modify `mpmd.conf` launch
In the same runscript, add one process for:

```bash
${PYTHON} ${basedir}/etc/era5g_wave_provider.py
```

and reduce the model rank range accordingly. Your runscript already has commented hints for this.

### 8. Build and test
Test order:
- Python dry run:
   ```bash
   python era5g_wave_provider.py --dryrun ...
   ```
- Compile ICON after Fortran changes.
- Run a very short coupled test, maybe 1-6 hours.
- Confirm fields exist in output:
   ```bash
   cdo sinfo output_file.nc
   ncdump -h output_file.nc | grep era5.*stokes
   ```

**Main correction to your list**
Your step 3 says “modify `mo_ocean_era5_provider_coupling.f90` to add the 3 more fields”. That can work, but I would instead add a separate wave-provider receiver. Storage can still be in `p_as`; the coupling receive logic should be separate because the source component is separate. This makes the system easier to test and avoids making the atmosphere/runoff provider and wave provider artificially inseparable.