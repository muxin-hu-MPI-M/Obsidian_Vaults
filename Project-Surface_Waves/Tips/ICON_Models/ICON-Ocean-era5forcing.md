---
tags:
  - project/surfwaves
  - wave/surface_wave
  - ICON
  - ICON/experiment
Last Eddited: 2026-06-01
---
# `era5g_omip_runoff_provider.py`
the core function:
```python
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

# Inside the runscript (e.g., `exp.ocean_era5_R2B7L72.run`)

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

If you want to use a different data location, the right place is the provider launch line in the generated mpmd.conf block, for example: