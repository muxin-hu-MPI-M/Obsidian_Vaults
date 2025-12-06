---
tags:
  - ICON
  - "#code/tips"
Last Eddited: 2025-12-01
---
The below is the important tip for running long simulation under high resolution configuration (e.g., R2b7), suggested from Helmuth Haak

# Normal runscript
this method is suitable for the experiment using the standard method `make_runscript` to generate the runscript.
## Running Long Simulation

1. **Run the first few months/years to check the validity of the simulation**
    
    1. Go to `/path/to/your/icon/run/` and check:
        1. `LOG.xxx` → check for the `sbatch` simulation time
        2. `LOG.ERROR.xxx` → check for the `error`
    2. using visualisations of key global variables
2. To continue run the same experiment but with an extended simulation time, **a file “`isRestartRun.sem`” has to existed in work directory**
    
    1. Go to the work directory: `/path/to/your/icon/experiments/exp-ID/`
        
    2. Then type: `touch isRestartRun.sem` to ensure the existence of this file under work directory
        
        Notice: “if it finds this `isRestartRun.sem` file it picks the restart, otherwise is it thinks its the first job in the chain and ignores any existing restart but start from the initial state”
        
3. **Extend the simulation time to your desired time in the same experiment running script**
    
    1. Notice: if the start date is 2010 and the end date is 2040, the model will run a chain of jobs each 5 years
4. **Resubmit the same job in the Levante using:** `sbatch same-exp-ID.run`


# Using mkExp
this method is suitable for the experiment using the method `mkexp` to generate the runscript.

## Running Long Simulatino
1. Run the first few months/years to check the validity of the simulation

	1. Generate your run script under `/icon-mpim/build/run` by `../../utils/mkexp/mkexp/ mux1234.config`
	2. In the `/icon-mpim/experiments/mux1234/script` directory, `sbatch` your experiment ID: `sbatch mux1234.run_start`

2. To continue run the same experiment but with an extended simulation time

	1. First go back to the `/icon-mpim/build/run` to modify your configuration file with extended simulation time
	2. Then, go to the `/icon-mpim/experiments/mux1234/script` using the update function `./update`
		1. This will update your `mux1234.run` and `mux1234.run_start`, the older versions are stored in the `/backup` directory. You can check the difference between any backup scrips with your current version using `diff`
	3. Last, sbatch your job, this time, `sbatch mux1234.run` instead of the `mux1234.run_start`. Otherwise, the simulation will restart from the beginning