---
tags:
  - code/tips
Last Eddited: 2025-12-01
---
# Introduction

Speed comes form parallel computing. In general, there are two types of parallel computing that we can implement:

1. If the issue of slow speed is because of the very **big size** of a single file, you should consider [**dask**](https://docs.dask.org/en/stable/array-best-practices.html) (4.1). This also applies to the case where you read multiple files into a single dataset with `xr.open_mfdataset`. Dask do two things:
    - enable to process data whose size is larger than the memory.
    - parallel apply calculations on chunks thus faster.
2. If the issue of slow speed is because of a **loop** on many files, or just a loop with many steps on a single file, consider [**mpi4py**](https://mpi4py.readthedocs.io/en/stable/)(4.2) or [**parallel**](https://www.gnu.org/software/parallel/)(4.3).

This note will consider the first category, to use DASK for parallel working on large size of a single data frame

## Reference

- A blog to start([https://blog.dask.org/2021/11/02/choosing-dask-chunk-sizes](https://blog.dask.org/2021/11/02/choosing-dask-chunk-sizes))
- xarray dask doc([https://docs.xarray.dev/en/stable/user-guide/dask.html#chunking-and-performance](https://docs.xarray.dev/en/stable/user-guide/dask.html#chunking-and-performance))
- easygen dask doc([https://easy.gems.dkrz.de/Processing/dask/index.html](https://easy.gems.dkrz.de/Processing/dask/index.html))

# Scenario: Calculate the surface-forced WMT from multiple large dataset

- You have NetCDF files with monthly data for variables like `SST`, `SSS`, `areacello`, `freshwater_flux`, `net_heat_flux`.
- Each variable is stored in **chunks internally** in the NetCDF files, e.g., `SST` might be stored with **chunks of size `(time=12, lat=255, lon=255)`**. This is how the file physically stores data on disk.

In this case, when reading data you should align your chunks with your storage format (e.g., “time”, “nlat”, “nlon”). This will automatically not store all your data into one big dataset that is larger than the RAM. The data is represented as DASK arrays which are essentially lazy objects pointing to the data on disk. Operations are applied chunk by chunk.

## Example work flow

```python
import xarray as xr
import dask

# ------------------------
# 1. Open multiple datasets with chunks
# ------------------------
# Define your chunk size, e.g., time chunks of 12 months, spatial 255x255
chunk_dict = {"time": 12, "lat": 255, "lon": 255}

# List of variable files (SST, SSS, areacello, freshwater flux, net heat flux)
file_paths = {
    "SST": "/path/to/sst.nc",
    "SSS": "/path/to/sss.nc",
    "areacello": "/path/to/areacello.nc",
    "fwflux": "/path/to/freshwater_flux.nc",
    "netheat": "/path/to/net_heat_flux.nc"
}

# Open datasets with chunks
ds_dict = {var: xr.open_dataset(fp, 
						chunks=chunk_dict)[var] for var, fp in file_paths.items()}
# This will read data into lazy object

# ------------------------
# 2. Perform computation chunk-wise with Dask
# ------------------------
# Example: weighted mean temperature (just an illustration)
# WMT = sum(SST * area) / sum(area)

sst = ds_dict["SST"]
area = ds_dict["areacello"]

# Compute weighted mean
wmt = (sst * area).sum(dim=("lat", "lon")) / area.sum(dim=("lat", "lon"))

# ------------------------
# 3. Trigger computation
# ------------------------
# This is when Dask actually reads chunks from disk
wmt_computed = wmt.compute()

# ------------------------
# 4. Optionally save output
# ------------------------
wmt_computed.to_netcdf("/path/to/wmt_output.nc")

print("WMT calculation done!")

```