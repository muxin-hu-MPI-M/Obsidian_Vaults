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