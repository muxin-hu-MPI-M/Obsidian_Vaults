---
tags:
  - ICON
  - mkexp
  - ICON/experiment
Last Eddited: 2025-12-03
---
This note contains the basic setup for using the `mkexp` as a tool for generating the `runscript` for ICON model. The information is referring to [mkexp hands-on](https://pad.gwdg.de/s/c01JTHzYS) from #presenter/Karl-Hermann

# Basic
## Introduction
### Declare instead of program
- describe your experiment setup instead of writing a script
- existing experiments setups are ready for use
- full control over input file and `namelist` settings

### Re-use the declaration
- for different HPC environment
- extend your work process beyond shell scripting
- vary by adding options - packaged settings

### One experiment - one config file
- each experiment deserves its own declaration
- low-effort way to document your work
- for ensembles, this may be lifted via command line settings

## mkExp setup
### Experiment type
contains all settings and data needed for a class of experiments
- model `namelists` configuration
- grid data, initial conditions or model state, boundary conditions
- see `run/mkexp/types` for available types
	- for example, under my work directory `/work/mh0033/m301254/proj_surfwave/icon-2025-08-06-XPP/icon-mpim/run/mkexp/types/XPP` you will find `piControl-R2B5_R2B7.config`, with the headings:
```shell
#
# General setup of ICON-XPP target configuration for CMIP7: R2B5 ICON-NWP coupled with R2B7 ICON-O using HDext
#  *** *** *** *** *** ***
#  Notes:
#   - restart from coupled spinup experiment
#   - no output included, chose via options
#   - update to code and parameters of 20250806
#  *** *** *** *** *** ***

PROJECT = ICON-Seamless

# General paths
ECRAD_PTH = $MODEL_DIR/externals/ecrad/data
XPP_POOL_BASE = /pool/data/icon-xpp
HDMODEL_EXE = hd_05.exe                #  default name on $ICON_DIR/externals/hd/bin
```

### Fantastic namelists and Where to Find Them
Please find more detailed description and names for all subsections and sub-subsection in [Namelist_overview](https://owncloud.gwdg.de/index.php/s/A9WWQAZTPp0mvSW)
#### Namelists and other parameter files
- user defined text files read by model
- parameters controlling most aspects of model behaviour
- different formats (Fortran namelist, YAML)
#### Namelists have their own config section
- fixed name `[namelist]`
- subsections for each namelist file `[[NAMELIST_atm]], [[NAMELIST_lnd]], [[NAMELIST_oce]]`
- subsubsections `[[[nh_testcase_nml]]]`
	- for each Fortran namelist group
	- or other structured data
- simplified style for parameter settings
example:
```shell
[namelists]                                             # headings for change namelist (nml)
  [[NAMELIST_atm]]                                      # subsection: change atmosphere nml 
    [[[nh_testcase_nml]]]                               # subsubsection: 
      # comments start with '#'
      ape_sst_val = 30.0
      ape_sst_case = sst_const # quotes are optional
    [[[output_nml atm_2d]]]
      ml_varlist = ps, ts, prw # commas for lists
      output_grid = true # or 'false'
```
#### Namelists settings are provided “as is”
- no restriction -  allowing for model changes
- no check - user is responsible for correctness
- does not replace documentation -see “Namelist Overview”
	- **==Find all subsections and sub-subsections in [Namelist_overview](https://owncloud.gwdg.de/index.php/s/A9WWQAZTPp0mvSW)==**
- example:
	1. get CO2 concentration from a fixed volume mixing ratio
	2. make this ratio be 4 times its default value
```shell
[namelists]
  [[NAMELIST_atm]]
    [[[aes_rad_nml]]]
    # get CO2 concentration from a fixed volume mixing ratio
    aes_rad_config(jg)% irad_co2 = 2          # default 2: volume mixing ratio set by 'vmr_co2'
    # make this ratio be 4 times its default value
    # aes_rad_config(jg)% vmr_co2: default=348.0e-06
    # use the scaling factor
    aes_rad_config(jg)% frad_co2 = 4.0
```

### Input Data
#### Data files go to the model’s working directory
- by default symbolic links, but other methods allowed
- working name may be different from source name
- files may be dependent on model time
#### Files come in functional group
- mapped to the model grid vs. grid-independent files
- ~={red}**organised in directories by grid and topics**=~
- with model time stamp vs. constant/multi-date files
- as-is vs. pre-processing (e.g., with cdo)
#### Data files are also declared in the config
- section name is `[files]`
- subsections are: `[[atmosphere]], [[land]], [[ocean]]`
- subsubsections define groups of files
- format is `working name` = `source name`
- example:
```shell
[files]
  [[atmosphere]]
    .base_dir = $ATMO_INPUT_DIR # path to your data (e.g., XPP_POOl_ATM)
    
    [[[common]]]
      .sub_dir = common
      [[[[solar_irradiance]]]]  # working_name (in runscript) = source_name (i.e., your files)
        bc_solar_irradiance_sw_b14.nc = swflux_14band_cmip6_1850-2299-v3.2.nc
        
    [[[mapped]]]
      .sub_dir = $ATMO_GRID_ID
      [[[[aerosols]]]]
        .offsets = -1, 0, 1
        .set label = 1849, 1849:%year, 2022:2021
        bc_aeropt_kinne_sw_b14_fin_%year.nc = bc_aeropt_kinne_sw_b14_fin_$${label}.nc
        # for the year before 1849, use the aeropt in 1849 level (label=1849)
        # for the year between 1849:2021, use the aeropt in the each year's level (label=%year)
        # for the year after 2021 (e.g., 2022), use the aeropt in 2021 level (label=2021)
        
      [[[[ocean_surface]]]]
        bc_sst.nc = # disable "normal" SST
        [[[[[sst]]]]]
          .method = 'cdo addc, 4'
          bc_sst.nc = bc_sst_$${label}.nc
          
    [[[independent]]]
      .base_dir = $XPP_POOL_COM
      bc_greenhouse_gases.nc = greenhouse_1pctCO2.nc
      # take a forcing of 1% increase of CO2 each year since 1850
```
#### Input data directory: `/pool/data/`
```shell
# General paths
XPP_POOL_BASE = /pool/data/icon-xpp
# Atmosphere setup
ATMO_GRID_ID = 0030        # what is this?
ATMO_GRID_TYPE = R02B05
ATMO_LEVELS = 130
XPP_POOL_ATM = $XPP_POOL_BASE/$ATMO_GRID_ID

# Ocean setup
OCEAN_GRID_ID = 0062       # what is this?
OCEAN_GRID_TYPE = R02B07
XPP_POOL_OCE = $XPP_POOL_BASE/$OCEAN_GRID_ID

# Coupling setup
XPP_POOL_COM = $XPP_POOL_BASE/common
XPP_POOL_AOC = $XPP_POOL_BASE/${ATMO_GRID_ID}-${OCEAN_GRID_ID}
```

# mkExp Evaluation
### Lazy evaluation
- values may reference variables from its own or any ancestor section

```shell
ATMO_INPUT_DIR = /pool/data/ICON/grids/public/mpim
[files]
  [[atmosphere]]
    .base_dir = $ATMO_INPUT_DIR
```
- these references are only resolved when the value is actually used
    - default values may contain variables that are defined in the type
    - you cannot amend a variable value - `PATH = /usr/bin:$PATH` will cause an infinite loop
### Incremental list assignments
- regardless, any _list_ from a base config, eg.
    ```shell
      [[[output_nml atm_2d]]]
        ml_varlist = ps, psl, ts, prw
    ```
    
    may be amended in the user config using `+=`, `-=`, or `>=`
    
```shell
    [[[output_nml atm_2d]]]
      ml_varlist += prls      # ps, psl, ts, prw, prls
      ml_varlist -= prw, psl  # ps, ts, prls
      ml_varlist >= ls>lr     # ps, ts, prlr
```



# The Hierarchy of Configurations
## User, Type, Default
- within the ICON `run` directory
	- common settings for all types go to `mkexp/defaults`
	- the experiment types’ config is in `mkexp/types`
- a type config amends the common settings
- your user config again overrides the amended settings

## Options
- in addition, type independent settings are grouped in *options*
	- output request definitions
	- testing routines
	- additional jobs
- options are read after the type, before the user config

## How environment (machine) specific settings
`SETUP.config`, `MAP.config` `run/mkexp/environments`
- *mkexp* makes use of build information to get the host environments
- each environment has its own config file holding specific settings



# Documentation For mkExp
The note is referring to the note by #presenter/Karl-Hermann, with the [mkExp documentation](https://owncloud.gwdg.de/index.php/s/A9WWQAZTPp0mvSW)

## 2.2 Tools
- `mkexp [-m] [-g] file.config [name=value ..]`
	- main tool for generating an experiment setup
	- when running, `mkexp` creates rhree directories: job scripts, run-time data and output data.
	- when given the ‘`-m`’ or ‘`--no-make-dirs`’ option, only the script directory is created while the creation of the run-time and output directories is skipped
	- `... [name=value ...]`, allows to override the `.config` file settings on he command line by defining or re-defining a variable *name* set to *value*.
	- Instead of '=', you mayuse '+=', '-=', or '>=' to amend or alter *name*'s value.
- `getexp [-v ...] [-R] [-k key] file.config [name=value ...]`
	- `getexp` reads the experiment setup the same way as `mkexp`, but does not generate job scripts. Instead it prints the experiment name and directories to be generated in a shell-readable form. It is intended for debugging or passing setup information to utility scripts.
	- When given the '`-v`' or '`--verbose`' option, all global configuration variables and their values are printed in alphabetical order. When given twice, the whole configuration is dumped to the screen. Save this to a file for use with `mkexp` -g.
	- When given the '-R' or '--readme' option, the header comment text is printed. When given the '-k' or '--key' option, only the configured value for key is printed.
- `diffexp file1.config file2.config`
	- comparison of he whole set of generated scripts for two different experiments
- `cpexp [-n] file.config new_name [name=value ...]`
	- Replicates all data of an experiment to a new experiment name; also updates text files by rewriting references to the old name. With '-n', shows what would be done instead of actually doing it.
- `compconfig [-t] [-c] file1.config file2.config [file.config ...]`
	- Filter tool to select all settings from file1.config that are common to file2.config and every file.config. Useful to extract a default config for a number of experiment type configs
- `diffconfig [-t] [-c] file1.config file2.config`
	- Filter tool to remove all settings from file1.config that are duplicated in file2.config. Useful to check new experiment type configs against the default config.

### 2.3.3 Special Variables and Sections
- **EXP_TYPE**
  Selects one of the standard experiments that are pre-defined in the model setup as basis of the current experiment definition.
- **EXP_OPTIONS** (optional) 
  Subset of the model's standard options that should be applied to the current experiment definition.
- **EXP_ID** (optional)  
  Name of the experiment to be created. If not set, this will be set to the base name of the user's .config file, e.g. 'joe1234' in the introductory example. All job description files will carry this as the first part of their name. For almost all model setups, this will be used in the definitions of SCRIPT_DIR, WORK_DIR, and DATA_DIR.






# [[2025-12-04]] Q&A from Helmuth
#presenter/Helmuth_Haak

- **Q1: Are the namelists different in different ICON configuration?** (e.g., ICON-Sapphire, ICON-XPP), since the ICON-XPP use the ICON-NWP as the atmospheric component. I heard that there is a so-called ‘Sapphire physics’, for example, the `aes_rad_nml` subsections
	- yes. And sometimes the namelists for difference ICON configuration are not the same. if you are confused, go to the documentation or ask #presenter/Helmuth_Haak and #presenter/Karl-Hermann 
	- 

- **Q2: Why there are only limited amount of `EXP_TYPES` in ICON-XPP?** IN the directory: `/run/mkexp/types/XPP`, there’re only two piControl configurations in two grid types

- **Q3: where I can find the configuration file (i.e., reference) for specific forced experiment?** (e.g., omip) 
	- `xpp-b5b7-HDext.config` under `/run/examples/` directory
	- `ocean_omip-R02B04L40.config` is found in `run/mkexp/xpp` in master branch of icon-mpim, but not in the XPP-branch (branch: icon-XPP-20250806). 

- **Q4: what are the *options*, *environment* and *default* under `/run/mkexp/` directory?**

- **Q5: After a test run, how to restart and continue?** I suppose something different from the `isRestartRun.sem`



