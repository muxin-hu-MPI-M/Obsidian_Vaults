---
tags:
  - code
  - ICON-O
  - ICON/experiment
Last Eddited: 2026-08-11
---
# Status: 

## [[2026-11-10]]
Once the `l_stokes_eulerian_velocity_diffusion = .TRUE.`, the TKE shear production term uses the reconstructed Eulerian velocity, not the prognostic Lagrangian velocity
The chain is:
1. Reconstruct source-time Eulerian velocity: $\mathbf{u}_E^n=\mathbf{u}_L^n-\mathbf{u}_S^n$, using `lagrangian_vn - stokes_vn_old`.
2. Pass `eulerian_vec_c` into `update_ho_params_zstar` at [mo_hydro_ocean_run.f90 (line 1295)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:1295).
3. Forward it to `calc_tke` at [mo_ocean_physics.f90 (line 1024)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/physics/mo_ocean_physics.f90:1024).
4. Inside TKE, the optional field becomes the shear velocity at [mo_ocean_tke.f90 (line 1195)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/physics/mo_ocean_tke.f90:1195):

```
IF (PRESENT(velocity_vec_c)) THEN
  shear_velocity_c => velocity_vec_c
ELSE
  shear_velocity_c => ocean_state%p_diag%p_vn
END IF
```

The physical z-star shear is then calculated as:
$$S^2 = \left| \frac{\mathbf{u}_E(k-1)-\mathbf{u}_E(k)} {\Delta z\,\mathrm{stretch}_c} \right|^2,$$
at [mo_ocean_tke.f90 (line 1295)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/physics/mo_ocean_tke.f90:1295). This `Ssqr` enters the TKE calculation and affects the resulting vertical viscosity `a_veloc_v`.

The full behaviour is therefore:

|Switch|TKE shear production|Velocity diffusion/mixing|
|---|---|---|
|`.FALSE.`|Prognostic Lagrangian velocity|Prognostic Lagrangian velocity|
|`.TRUE.`|Reconstructed Eulerian velocity|Reconstructed Eulerian velocity|

The vertical implicit solver then applies the TKE-derived `a_veloc_v` to the Eulerian predictor through the subtract-Stokes → diffuse → add-Stokes sequence.

So with the switch enabled, the implementation is internally consistent: both the production of vertical viscosity and its application use Eulerian velocity. Other TKE sources—buoyancy production, wind input, dissipation and IDEMIX input—are unchanged.
## [[2026-11-06]]
- diagnostic fields: `stokes_u/v, eulerian/stokes_u/vT`: **source time**, model-resolved cell reconstruction
	- ordinary ICON uses the same general staging
	- all diagnostics are at source time
- `stokes_vn`: **current** prescribed profile translated into the model’s edge-normal space
- `era5_u/vst_3d`: prescribed **current** cell-centered Stokes profile
```
Source state at t_n:
    Lagrangian velocity = p_prog(nold)%vn
    Stokes velocity     = stokes_vn_old

Fresh forcing for the step:
    Stokes velocity     = stokes_vn

Target state:
    Lagrangian velocity = p_prog(nnew)%vn
```
- the staggering difference is one model step → PT1200S in R2B7 case
Finish the 1,2 stage of the z-star implementation of Stokes forcings
## [[2026-11-04]]
see [[ICON-o_Stokes_Status#2026-11-04 Add Eulerian velocity diffusion, Mixing, and Split Stokes diagnostics]]
- Finish the implementation of
	- Velocity diffusion to Eulerian velocity
		- horizontal → involves changes in mixing scheme (e.g., shear production in TKE)
		- vertical → subtract-solve-add
	- Split Stokes diagnostics in heat/salt flux

## [[2026-10-29]]

Figure out the reason why we use the Eulerian vertical velocity in the first time step for a cold-start simulation when calculating the `stokes_shear_tend` with the definition: $w^L\partial_z u^s$.:
- A scheme-consistent $w^L$ requires the time-weighted velocity, which is unavailable until the first horizontal-velocity and free-surface solve → **a general ICON-O cold-start behaviour**
	- The first momentum tendency is evaluated using $w^E=0$. Therefore, ordinary vertical acceleration is also zero during this evaluation. After solving the first horizontal-velocity and free-surface step, ICON constructs `vn_time_weighted` and diagnoses $w^E$ from continuity. The diagnosed vertical velocity is available for the following momentum evaluation
	- The Stokes implementation follows the same startup convention
- Cold starts from rest therefore retain $w^L=0$ during the first momentum evaluation. The Stokes shear tendency begins on the following timestep
- An immediate diagnosis would require the provisional approximation `vn_time_weighted = vn(n)` and would also modify `w_old`, mass fluxes, and—under continuity correction—the provisional sea-surface height.
- Eulerian-restart activation uses this provisional diagnosis because the inherited $w^E$ is inconsistent after converting to Lagrangian velocity. Normal Lagrangian restarts retain their consistent saved $w^L$.
- Consequently, the cold start has a known one-timestep imbalance between the wavy-hydrostatic source and its paired shear tendency.

Below is the explanation:
ICON cannot diagnose the scheme-consistent $w^L$ until the first horizontal-velocity solve has finished. ICON diagnoses vertical velocity from the time-weighted horizontal velocity:
$$ v_n^{\,n+\gamma} = \gamma v_n^{\,n+1} + (1-\gamma)v_n^{\,n}, \qquad \gamma=0.6, $$
followed by
$$ \partial_z w^{\,n+\gamma} = -\nabla_h\cdot\boldsymbol{v}^{\,n+\gamma}. $$
The time-weighted velocity is constructed only after the free-surface and new horizontal velocity have been solved in [mo_ocean_ab_timestepping_mimetic.f90 (line 1270)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90:1270). The normal vertical-velocity diagnosis then occurs at the end of the timestep in [mo_hydro_ocean_run.f90 (line 727)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:727).

At cold-start Stokes insertion, $v_n^{n+1}$ does not exist yet. Therefore, an immediate diagnosis would require the approximation
$$ v_n^{n+\gamma}\approx v_n^n, $$
which is exactly what the Eulerian-restart activation does by manually setting:

```
vn_time_weighted = p_prog(nold)%vn
```

That is a provisional first-order diagnosis, not the normal ICON time-centred one. There are also side effects: `calc_vert_velocity` does more than calculate $w$. It overwrites:
- `w_old`;
- horizontal mass flux and its divergence;
- `w`;
- and, when `use_continuity_correction=.TRUE.`, the provisional next-step sea-surface height:

$$ h^{n+1}=h^n+w_{\mathrm{surface}}\Delta t. $$

See [mo_ocean_ab_timestepping_mimetic.f90 (line 1350)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90:1350) and [the continuity correction (line 1465)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90:1465). Calling it during cold-start initialization is therefore not a harmless diagnostic operation.

The ordinary ICON cold-start procedure also does not diagnose $w$ from the initial horizontal velocity. It initializes the state, writes the $00{:}00$ output, and obtains the first consistent $w$ only after the first velocity solve. The Stokes cold start preserves that general startup convention.

The Eulerian-restart transition is different: it starts from a dynamically developed circulation, and the stored $w^E$ is definitely inconsistent after converting $v^E$ to $v^L$. A provisional rediagnosis is preferable there.

## [[2026-10-27]]
- Finished implementation of namelist switch on the `wavy_hydrostatic_src` and its energy pair `stokes_shear_tend`
## [[2026-10-20]]
- merged with the `origin/master` under the `icon/icon.git`
- Implementation of single year repeat run in the python wave provider under `/etc`
- investigate the magnitude of the wavy-hydrostatic source term and its implication, find details in [[ICON-o_Stokes_Lagrangian_implementation#11. Wavy-Hydrostatic Pressure]]
## [[2026-09-07]]
Find the detailed summary of all implementations here: [[ICON-o_Stokes_Lagrangian_implementation]]
## [[2026-08-21]]
### Transition from noStokes spin-up to withStokes forcing: potential problem with Adam-Bashforth tendencies
During the first restarted timestep, ICON calculates a new `g_n` from the current state. Because a restart is not considered an initial timestep, it then constructs
$$ g_{\mathrm{nimd}} = (1.5+\mathrm{ab\_const})g_n - (0.5+\mathrm{ab\_const})g_{n-1}, $$

using the restored `g_nm1`, in [mo_ocean_ab_timestepping_mimetic.f90 (line 883)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90:883). While `g_n` is ICON’s total explicit horizontal momentum tendency at the current time, `g_nm1` is the corresponding tendency from the previous timestep. 

`g_nimd` is an Adam-Bashforth estimate of the tendency at the intermediate time (n +1/2). The confusing part is that `g_nimd` represents an approximate tendency at the intermediate time $n+\tfrac12$, while that tendency is used to construct a provisional velocity at the new time $n+1$. ICON uses it in the velocity predictor: $$v_n^{\mathrm{pred}} = v_n^{\mathrm{old}} + \Delta t\,g_{\mathrm{nimd}}$$
apart from the separately treated surface-pressure term. The new surface pressure gradient depends on the unknown new sea-surface height $\eta^{n+1}$. ICON therefore cannot calculate the final velocity immediately. ICON uses the `vn_pred` to construct the transport divergence and solve the elliptic free-surface equation for $\eta^{n+1}$. After obtaining it, ICON uses it to correct the velocity at next timestep `v_np1`.

For an ordinary with-Stokes to with-Stokes restart
- Restarted `vn` is Lagrangian.
- `stokes_vn_old` contains the previous Stokes field.
- `g_nm1` contains a wave-inclusive tendency.
- AB timestepping continues consistently.

For a no-Stokes to with-Stokes restart:
- Restarted `vn` is Eulerian.
- `stokes_vn_old` is expected to be zero.
- `g_nm1` contains a no-wave Eulerian tendency.
- The new `g_n` contains the newly activated wave/Lagrangian terms.
- ICON combines the new wave tendency with the old no-wave tendency in the first AB step.

The restart file contains `g_nm1` so that an **unchanged model equation** can continue with second-order AB accuracy. In the no-Stokes to with-Stokes transition, however, the equation and the meaning of `vn` both change. That makes the stored `g_nm1` formally inconsistent with the new `g_n`.

Suppose the activation occurs at timestep $n$: $$g_{n-1}=g_{n-1}^{E}, \qquad g_n=g_n^{L,\mathrm{wave}}.$$
Using the normal AB formula gives $$g_{\mathrm{nimd}} = 1.6g_n^{L,\mathrm{wave}} - 0.6g_{n-1}^{E}.$$

==Adams–Bashforth derives this extrapolation under the assumption that both tendencies are samples of the same smoothly evolving equation. That assumption is **violated** here.==

suggested change:
At the activation time, the relevant acceleration over the following interval is the new wave/Lagrangian acceleration. We have no valid previous sample of that equation. The standard multistep startup solution is therefore: $$g_{\mathrm{nimd}}=g_n^{\mathrm{L, wave}}$$
This is first-order for one step, but it avoids extrapolating the new tendency using an incompatible no-wave history.

It does **not** permanently replace AB timestepping:
```
Activation step:
    g_nimd = g_n_wave

Following step:
    g_nimd = 1.6*g_n_wave_new - 0.6*g_n_wave_old

Later steps:
    normal AB continues
```
We also do not need to erase or rewrite the restart file. The conditional would simply be:
```
IF (is_first_timestep .OR. is_stokes_activation_step) THEN
  g_nimd = g_n
ELSE
  g_nimd = (1.5_wp + ab_const) * g_n &
         - (0.5_wp + ab_const) * g_nm1
END IF
```

## [[2026-08-20]]
We are working in the ICON-o branch:
`/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing`
### Scientific Goal
Implement the surface-wave-modified ICON-o primitive equations using the Lagrangian velocity
$$
\mathbf v^L=\mathbf v^E+\mathbf v^s.
$$

When `l_stokes_forcing = .TRUE.`:
- Prognostic edge-normal `vn` represents $v_n^L$.
- Diagnostic vertical velocity `w` should represent $w^L$.
- Existing advection, diffusion, Coriolis, continuity, and tracer calculations should use the Lagrangian velocity.
- The additional horizontal momentum RHS contains
$$
w^L\partial_z\mathbf v^s,
\qquad
\zeta^s\hat{\mathbf z}\times\mathbf v^L,
\qquad
\partial_t\mathbf v^s.
$$
- Hydrostatic pressure includes the wavy-hydrostatic source
$$
\mathbf v^L\cdot\partial_z\mathbf v^s.
$$

When `l_stokes_forcing = .FALSE.`, ICON should retain its original Eulerian behavior.
### Input Stokes Fields
ERA5/provider fields are stored in `p_as`:
- `p_as%ust_3d`, `p_as%vst_3d`: cell-centered three-dimensional Stokes velocity starting at model level 1.
- `p_as%surf_ust`, `p_as%surf_vst`: separate surface Stokes velocity.
- `p_as%stokes_transport`: Stokes transport used by the vertical-profile reconstruction.

The surface values are used to calculate the derivative between the surface and the first model level.

### Implemented Source Changes
#### Configuration
`l_stokes_forcing = .FALSE.` was added to `ocean_physics_nml`.
The runscript activates the implementation with:
```fortran
&ocean_physics_nml
  l_stokes_forcing = .TRUE.
/
```
#### Stokes Forcing Module
The new module is:
`src/ocean/dynamics/mo_ocean_stokes_forcing.f90`
Its public routines are:
```fortran
prepare_stokes_fields
initialize_lagrangian_velocity_from_stokes
calculate_stokes_pressure_source
calculate_stokes_momentum_rhs

stokes_local_to_cartesian_cells
stokes_surface_local_to_cartesian_cells
stokes_cell_to_edge_normal
stokes_vertical_shear_tendency
stokes_vorticity_tendency
stokes_time_tendency
wavy_hydrostatic_source
```

The module performs:
- Local zonal/meridional to Cartesian conversion.
- Cell-centered Cartesian to edge-normal mapping.
- Top and interior vertical Stokes derivatives.
- $w^L\partial_z\mathbf v^s$.
- Stokes relative vorticity and $\zeta^s\hat{\mathbf z}\times\mathbf v^L$.
- $(v_n^{s,\mathrm{now}}-v_n^{s,\mathrm{old}})/\Delta t$.
- $\mathbf v^L\cdot\partial_z\mathbf v^s$.
- Assembly of the total horizontal Stokes RHS.

#### Persistent State and Diagnostics
Fields added to `t_hydro_ocean_diag` include:
```fortran
stokes_vec_c
stokes_vec_sfc_c
stokes_vn
stokes_vn_old
stokes_shear_tend
stokes_vort_tend
stokes_time_tend
stokes_rhs
stokes_zeta_v
stokes_vn_dual
wavy_hydrostatic_source
```
These fields are allocated and registered in `mo_ocean_state.f90`.
`stokes_vn_old` is registered in the restart list.
#### Momentum Integration
The non-zstar mimetic Adams-Bashforth path now:
1. Prepares the Stokes cell and edge fields.
2. Computes the wavy-hydrostatic source.
3. Includes that source in hydrostatic pressure integration.
4. Computes all horizontal Stokes tendencies.
5. Adds `stokes_rhs` to `g_n`.
The implemented assembly is
$$
R_{\mathrm{Stokes}}
=
R_{\mathrm{shear}}
+
R_{\mathrm{vorticity}}
+
R_{\mathrm{time}}.
$$
These are positive RHS contributions corresponding to the terms written with negative signs on the left-hand side of the continuous momentum equation.
#### Wavy-Hydrostatic Pressure
`calc_internal_press_grad` accepts the optional argument:
```fortran
wavy_hydrostatic_source_c
```
The source is included in:
- The top-level pressure integration.
- Interior trapezoidal vertical integration.
- Partial-cell pressure corrections.
Because ICON stores `pressure_hyd` effectively as $p/\rho_0$, the source is integrated as
```text
wavy_source * vertical_distance
```
without an additional multiplication by $\rho_0$.
### Standalone Operator Tests
The Stokes testbed is implemented in:`src/ocean/testbed/mo_ocean_testbed_stokes_forcing.f90`
and registered through:`src/ocean/testbed/mo_ocean_testbed_operators.f90`
Important test modes are:

| Mode | Test |
|---|---|
| 120 | Surface-to-level-1 Stokes shear and hydrostatic source |
| 121 | Interior vertical Stokes shear and layer placement |
| 122 | Exact Stokes time derivative |
| 123 | Zero first-step time derivative |
| 124 | Zero/uniform Stokes vorticity |
| 125 | Vorticity and vorticity-tendency linearity |

Modes 120–125 completed successfully with their tolerance checks.
### Compilation Status
The implementation was compiled successfully with both configurations:

```text
config/dkrz/levante.nag --enable-bundled-python=mtime,yac
config/dkrz/levante.intel --enable-openmp --enable-bundled-python=mtime,yac
```
Checked logs:
```text
build_nag/build_nag.log
build_intel_test/build_intel_test.log
```

No implementation-related compiler errors were found.
GPU execution has not been tested and is not currently required.
### Runtime Status
Control and Stokes experiments were run with:
```text
exp.ocean_era5_r2b4_noStokes.run
exp.ocean_era5_r2b4_withStokes.run
```

The Stokes run uses:
```fortran
l_stokes_forcing = .TRUE.
vert_cor_type = 0
```

The non-zstar configuration is required because Stokes forcing is currently integrated only into the non-zstar mimetic AB pathway.

The long experiments reached their stop date, wrote outputs and restart data, and completed ocean cleanup. The final `QSUBW_ERROR ... srun` messages appear to be wrapper-level termination messages after model completion rather than ICON crashes.

### Hourly Integration Sanity Check
Hourly diagnostics included:

```text
u
v
w
era5_ust_3d
era5_vst_3d
era5_surf_ust
era5_surf_vst
stokes_vn
stokes_shear_tend
stokes_vort_tend
stokes_time_tend
stokes_rhs
wavy_hydrostatic_source
```
The main findings were:
- The `00:00` record contains the untouched zero Eulerian initial state.
- ERA5 Stokes fields and `stokes_vn` are nonzero at `01:00`.
- Stokes vorticity and wavy-hydrostatic terms are nonzero at `01:00`.
- Stokes shear and time tendencies first become nonzero at `02:00`.
- Horizontal tendencies are generally between $10^{-8}$ and $10^{-5}\ \mathrm{m\,s^{-2}}$.
- The wavy-hydrostatic source is around $10^{-3}\ \mathrm{m\,s^{-2}}$, but it is not a direct horizontal acceleration.
- The maximum RHS closure error is approximately $4.55\times10^{-13}\ \mathrm{m\,s^{-2}}$.
Therefore,
$$
\texttt{stokes\_rhs}
=
\texttt{stokes\_shear\_tend}
+
\texttt{stokes\_vort\_tend}
+
\texttt{stokes\_time\_tend}
$$

is verified to NetCDF output precision.

### Lagrangian Initialization Evidence
At `01:00`, the with-Stokes minus control velocity was compared with the prescribed Stokes velocity.
At 6 m depth:

```text
Correlation u/v:       0.9907 / 0.9901
Vector amplitude ratio: 0.9733
Vector alignment:       0.9905
Relative vector RMSE:   0.1379
```

At 17 m depth:
```text
Correlation u/v:       0.9876 / 0.9883
Vector amplitude ratio: 0.9665
Vector alignment:       0.9880
Relative vector RMSE:   0.1546
```

These results strongly support that $v_n^s$ is being added to the prognostic Lagrangian velocity.
Exact equality is not expected because:
- The output follows one complete one-hour timestep.
- Stokes velocity is mapped from cells to edge normals.
- Output `u/v` is reconstructed from edge-normal velocity.
- Wave forcing begins modifying the Eulerian circulation immediately.

### Startup Ordering Identified
The current startup order is:
1. ICON writes the `jstep=0` output at `00:00`.
2. ICON enters the first timestep and advances `current_time`.
3. The ERA5 provider updates the Stokes fields.
4. The implementation sets $v_n^L=v_n^E+v_n^s.$
5. It sets $v_{n,\mathrm{old}}^s=v_n^s.$
6. The first Stokes time tendency is set to zero.
7. The first evolved output is written at `01:00`.

The zero first-step time tendency is internally consistent with directly inserting the complete first available Stokes field. Using an uninitialized zero history would add the same Stokes velocity twice.
However, this is a startup shortcut rather than a fully time-consistent $t_0$ initialization.

### Remaining Work
#### 1. Make Startup Fully Time-Consistent
The preferred sequence is:
$$
\text{obtain }v^s(t_0)
\rightarrow
v_n^L(t_0)=v_n^E(t_0)+v_n^s(t_0)
\rightarrow
v_{n,\mathrm{old}}^s=v_n^s(t_0)
\rightarrow
\text{diagnose }\mathbf v^L(t_0),w^L(t_0)
\rightarrow
\text{write the initial output}.
$$
The first prognostic step should then use
$$
\frac{v_n^s(t_1)-v_n^s(t_0)}{\Delta t}.
$$

This requires determining how to make the ERA5/YAC Stokes field available at $t_0$ before ICON writes the `jstep=0` output.
Do **not** calculate the first derivative as `(stokes_vn - 0)/dt` while also adding the full `stokes_vn` to `vn`; that would double count the initial Stokes velocity.
#### 2. Diagnose Initial $w^L$
After adding `stokes_vn` to the initial horizontal velocity, immediately diagnose $w^L$ from continuity.
Currently the first momentum calculation receives the initially zero `w`, so `stokes_shear_tend` starts one timestep later.

#### 3. Verify Startup Forcing Timestamp
Confirm whether the first provider field used by initialization corresponds to:
```text
t0
```
or
```text
t0 + dtime
```

This requires tracing the YAC/provider timestamp semantics around `ocean_time_nextStep()` and `update_ocean_surface_refactor()`.

#### 4. Restart Validation
Run a wave restart test and verify:
- Restart `vn` remains Lagrangian.
- `stokes_vn` is not added again.
- `stokes_vn_old` is restored.
- `stokes_time_tend` is continuous across the restart.
- A fallback is defined for older restart files without `stokes_vn_old`.

#### 5. Exact Edge-Space Initialization Check
Immediately after initialization, verify:
$$
v_{n,\mathrm{after}}^L-v_{n,\mathrm{before}}^E-v_n^s=0
$$
to numerical precision.
This avoids contamination from cell-edge remapping and the first dynamics step.

#### 6. Pressure and Sign Validation
Add focused tests for:
- The vertically integrated wavy-hydrostatic pressure contribution.
- The resulting horizontal pressure-gradient contribution.
- The signs of all Stokes terms in `g_n`.
- Partial-cell behavior.

#### 7. Conservation and Physical Validation
Still required:
- Zero-Stokes run reproduces the control.
- Momentum and kinetic-energy budgets.
- Tracer transport consistency with $\mathbf v^L$.
- Sensitivity to model timestep and ERA5 coupling interval.
- Long-term circulation response and spin-up behavior.

#### 8. Unsupported Paths
The current implementation does not yet cover:
- Z-star timestepping.
- GPU execution.
- Other ocean timestepping schemes outside the non-zstar mimetic AB path.

### Recommended Immediate Next Task
1. Trace how ERA5/YAC provides the first field at the experiment start time.
2. Design a dedicated Stokes startup routine that runs before the initial output.
3. Initialize $v_n^L(t_0)$ and `stokes_vn_old`.
4. Reconstruct the cell velocity and diagnose $w^L(t_0)$.
5. Compile with NAG and Intel.
6. Rerun modes 120–125.
7. Rerun the first-day hourly control/Stokes comparison.
8. Confirm that the `00:00` output contains the initialized Lagrangian velocity and that the first shear and time tendencies have the expected timing.


## [[2026-08-14]]
We are working in ICON-o branch:
`/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing`

Goal: implement Stokes/Lagrangian forcing for ICON-o primitive equations.
Physics goal:
- When `l_stokes_forcing = .TRUE.`, prognostic `vn` should mean Lagrangian edge-normal velocity `vn_L`.
- Fresh non-restart start: convert once with `vn_L(t0) = vn_E(t0) + vn_s(t0)`, where `vn_s` is mapped from ERA5 Stokes fields to edge-normal velocity.
- Normal wave restart: do **not** add `vn_s` again; restart `vn` is assumed already Lagrangian.
- Wavy hydrostatic source modifies `pressure_hyd` before `press_grad`.
- Horizontal Stokes compensation terms are added to `g_n`.

Important existing Stokes fields:
- ERA5/provider fields live in `p_as`:
  - `p_as%ust_3d`, `p_as%vst_3d`: cell-centered 3D Stokes, starting at model level 1.
  - `p_as%surf_ust`, `p_as%surf_vst`: separate surface Stokes values.
- Surface values are used for the top vertical derivative.

Current implementation status:
- Do **not** compile immediately if context compaction is unstable; first inspect source.
- Files touched:
  - `src/ocean/config/mo_ocean_nml.f90`
  - `src/ocean/dynamics/mo_ocean_types.f90`
  - `src/ocean/dynamics/mo_ocean_state.f90`
  - `src/ocean/dynamics/mo_ocean_stokes_forcing.f90`
  - `src/ocean/physics/mo_ocean_thermodyn.f90`
  - `src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90`
  - `src/ocean/drivers/mo_hydro_ocean_run.f90`

What appears already landed:
1. `l_stokes_forcing = .FALSE.` added to `ocean_physics_nml`.
2. Added diagnostic/state fields in `t_hydro_ocean_diag`:
   - `stokes_vec_c`
   - `stokes_vec_sfc_c`
   - `stokes_vn`
   - `stokes_vn_old`
   - `stokes_shear_tend`
   - `stokes_vort_tend`
   - `stokes_time_tend`
   - `stokes_rhs`
   - `stokes_zeta_v`
   - `stokes_vn_dual`
   - `wavy_hydrostatic_source`
3. Allocation/registration in `mo_ocean_state.f90` appears present:
   - scalar fields via `add_var`
   - `stokes_vn_old` on `ocean_restart_list`
   - Cartesian vector fields manually allocated/deallocated near `p_vn`/`p_vn_dual`.
4. Wrapper routines appear present in `mo_ocean_stokes_forcing.f90`:
   - `prepare_stokes_fields`
   - `initialize_lagrangian_velocity_from_stokes`
   - `calculate_stokes_pressure_source`
   - `calculate_stokes_momentum_rhs`
5. `calc_internal_press_grad` in `mo_ocean_thermodyn.f90` has optional: `wavy_hydrostatic_source_c`
	- and pressure integration included the wavy source: `pressure_hyd(1) += wavy_source(1) * dz_top   pressure_hyd(jk) += 0.5 * (wavy_source(jk) + wavy_source(jk-1)) * dz`. partial-cell correction also appears modified.
6. `calculate_explicit_term_ab` in `mo_ocean_ab_timestepping_mimetic.f90` appears wired:
    - if `l_stokes_forcing`, calls `prepare_stokes_fields`
    - computes `calculate_stokes_pressure_source`
    - calls `calc_internal_press_grad(..., wavy_hydrostatic_source_c=...)`
    - computes `calculate_stokes_momentum_rhs`
    - otherwise zeros `wavy_hydrostatic_source` and `stokes_rhs`
7. `calculate_explicit_term_g_n_onBlock` adds: `ocean_state%p_diag%stokes_rhs(je,jk,blockNo)`
8. Fresh-start conversion hook appears in `mo_hydro_ocean_run.f90` after `update_ocean_surface_refactor`:
```
IF (l_stokes_forcing .AND. is_initial_timestep(jstep-jstep_shift) .AND. (.NOT. isRestart())) THEN
  call prepare_stokes_fields(...)
  call initialize_lagrangian_velocity_from_stokes(...)
  ocean_state%p_prog(nnew(1))%vn = ocean_state%p_prog(nold(1))%vn
  call calc_scalar_product_veloc_3d(...)
END IF
```

Known concerns / next things to check:
- Implementation has **not been compile-verified**.
- Check syntax carefully before compiling.
- The old-restart fallback for missing `stokes_vn_old` is not implemented. Current assumption: normal wave restart has `stokes_vn_old` in restart file.
- `calculate_stokes_momentum_rhs` updates `stokes_vn_old = stokes_vn` immediately after forming `stokes_time_tend`; this is acceptable for now but slightly earlier than “after successful timestep”.
- v1 only targets non-zstar mimetic AB path.
- Need inspect for line continuation/OpenACC directive syntax, especially around:
    - `mo_ocean_state.f90` manual vector allocations
    - `!$ACC ENTER DATA COPYIN(...)` continuation
    - `mo_ocean_stokes_forcing.f90` import of `sync_e`
    - `mo_ocean_ab_timestepping_mimetic.f90` new `USE` and calls
    - `mo_ocean_thermodyn.f90` optional argument order

Important detail:
- ICON stores `pressure_hyd` effectively as `p/rho0`, so `wavy_hydrostatic_source = v_L . d_z v_s` should be integrated as `source * dz`, **not multiplied by `rho0`**.

Immediate next task in new chat:
1. Inspect current diffs/source state.
2. Fix any obvious syntax/import issues without compiling first if desired.
3. Then compile only touched objects when ready:
    - `mo_ocean_stokes_forcing.o`
    - `mo_ocean_state.o`
    - `mo_ocean_thermodyn.o`
    - `mo_ocean_ab_timestepping_mimetic.o`
    - `mo_hydro_ocean_run.o`
4. If compile errors appear, fix iteratively.

# ICON-o Stokes Forcing Sanity Check

## Initialise Stokes forcing: first few timesteps
### Purpose
The sanity check verifies that the prescribed ERA5 Stokes velocity is correctly received, reconstructed vertically, mapped onto ICON edges, inserted into the Lagrangian velocity, and used to calculate the additional momentum and wavy-hydrostatic terms.
The experiment uses hourly output during the first simulation day. The initial model velocity is zero, allowing the first response to the Stokes velocity to be identified clearly.

### Checks Performed
#### 1. ERA5 Stokes Input
We examined the maximum absolute values of:
- Surface Stokes components `era5_surf_ust` and `era5_surf_vst`
- Three-dimensional Stokes components `era5_ust_3d` and `era5_vst_3d`
- Mapped edge-normal Stokes velocity `stokes_vn`
The initial output at `1985-01-01 00:00` contains zero Stokes fields. The fields become nonzero at `01:00`.
At the first nonzero output:

| Field | Maximum absolute value |
|---|---:|
| Surface $u^s$ | $5.08\times10^{-1}\ \mathrm{m\,s^{-1}}$ |
| Surface $v^s$ | $4.33\times10^{-1}\ \mathrm{m\,s^{-1}}$ |
| 3-D $u^s$ | $1.32\times10^{-1}\ \mathrm{m\,s^{-1}}$ |
| 3-D $v^s$ | $1.23\times10^{-1}\ \mathrm{m\,s^{-1}}$ |
| Edge-normal $v_n^s$ | $1.21\times10^{-1}\ \mathrm{m\,s^{-1}}$ |

The reduced magnitude of the three-dimensional Stokes velocity is consistent with its vertical decay. The difference between the cell-centered components and `stokes_vn` is expected because `stokes_vn` is interpolated and projected onto edge normals.
#### 2. Activation of the New Terms
The first nonzero times are:

| Quantity | First nonzero output |
|---|---|
| ERA5 surface and 3-D Stokes velocity | `01:00`, index 1 |
| Edge-normal `stokes_vn` | `01:00`, index 1 |
| Stokes vorticity tendency | `01:00`, index 1 |
| Wavy-hydrostatic source | `01:00`, index 1 |
| Lagrangian $u$, $v$, and $w$ | `01:00`, index 1 |
| Stokes vertical-shear tendency | `02:00`, index 2 |
| Stokes time tendency | `02:00`, index 2 |

Here, index 0 is the initial output before any model step. Therefore, index 1 is the first evolved output, while index 2 is the second evolved output, although it is the third stored NetCDF record.

#### 3. Why the initial state is all zeros even I initialise the model with Stokes?
Because ICON writes the 1985-01-01 00:00 record before our Stokes initialization is executed.
The actual ordering is:
1. ICON writes the untouched initial state with jstep=0 in [mo_hydro_ocean_run.f90 (line 1748)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:1748). At this point, the configured initial Eulerian velocity is zero.
2. ICON enters ocean_time_step, increments the step, and advances current_time in [mo_hydro_ocean_run.f90 (line 464)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:464).
3. The ERA5 provider fields are updated in [mo_hydro_ocean_run.f90 (line 521)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:521).
4. Only then does our code execute $v_n^L=v_n^E+v_n^s$ in [mo_hydro_ocean_run.f90 (line 524)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:524).
5. The next normal output is written after completing the first model step, at 01:00, in [mo_hydro_ocean_run.f90 (line 808)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:808).
### Why the Terms Start at Different Times
#### Terms Starting at `01:00`
The Stokes vorticity tendency,

$$
\zeta^s\hat{\mathbf z}\times\mathbf v^L,
$$
can be calculated during the first model step because both the mapped Stokes velocity and initialized Lagrangian horizontal velocity are available.

The wavy-hydrostatic source,

$$
\mathbf v^L\cdot\partial_z\mathbf v^s,
$$
also becomes available during the first step because the Lagrangian horizontal velocity and vertical Stokes profile are already defined.

Consequently, `stokes_rhs` is nonzero at `01:00`, but at this time it consists almost entirely of the Stokes vorticity tendency.

#### Time Tendency Starting at `02:00`
The first-step behavior is deliberately defined as
$$
\left.\frac{\partial v_n^s}{\partial t}\right|_{\mathrm{first}}=0.
$$
During initialization, the model sets
$$
v_{n,\mathrm{old}}^s=v_n^s
$$

and adds $v_n^s$ directly to the initial Eulerian velocity. This avoids interpreting the transition from an uninitialized zero array to the first Stokes field as a physical acceleration.

At the following step, two valid Stokes states exist, so the model evaluates
$$
\frac{\partial v_n^s}{\partial t}
\approx
\frac{v_{n,\mathrm{now}}^s-v_{n,\mathrm{old}}^s}{\Delta t}.
$$

Therefore, `stokes_time_tend` first appears at `02:00`.

#### Shear Tendency Starting at `02:00`
The vertical-shear term is

$$
w^L\partial_z\mathbf v^s.
$$

During the first momentum calculation, the diagnostic vertical velocity supplied to this term is still the initially zero $w^L$. The first nonzero $w^L$ is diagnosed afterward from the continuity equation using the updated Lagrangian horizontal transport.

That diagnosed $w^L$ becomes available to the momentum calculation during the following step. Therefore, `stokes_shear_tend` first appears at `02:00`.

### Momentum RHS Closure
We verified

$$
R_{\mathrm{Stokes}}
=
R_{\mathrm{shear}}
+
R_{\mathrm{vorticity}}
+
R_{\mathrm{time}}.
$$

The maximum closure error is between zero and approximately
$$
4.55\times10^{-13}\ \mathrm{m\,s^{-2}},
$$

which is effectively machine/output precision. This confirms that all three stored tendency components are assembled correctly into `stokes_rhs`.

### Tendency Magnitudes
The horizontal Stokes tendencies generally lie between approximately $10^{-8}$ and $10^{-5}\ \mathrm{m\,s^{-2}}$:
- Stokes vorticity tendency: approximately $10^{-8}$–$10^{-7}\ \mathrm{m\,s^{-2}}$
- Vertical-shear tendency: approximately $10^{-7}$–$10^{-6}\ \mathrm{m\,s^{-2}}$
- Stokes time tendency: several $10^{-6}\ \mathrm{m\,s^{-2}}$
- Total Stokes RHS: several $10^{-6}$, approaching $10^{-5}\ \mathrm{m\,s^{-2}}$

The Stokes time tendency and vertical-shear tendency are the largest direct horizontal momentum contributions during this period.

The wavy-hydrostatic source has a larger magnitude, around $10^{-3}\ \mathrm{m\,s^{-2}}$, but it is not a direct horizontal momentum tendency. It is vertically integrated into pressure before its horizontal pressure gradient affects momentum.

### Lagrangian Velocity Initialization
At `01:00` and 6/17 m depth, the with-Stokes minus control velocity was compared with the prescribed cell-centered Stokes velocity over 13,517 wave-active cells.

| Metric | $u$ | $v$ |
|---|---:|---:|
| Correlation | 0.9876 | 0.9883 |
| RMSE | $5.39\times10^{-4}\ \mathrm{m\,s^{-1}}$ | $4.49\times10^{-4}\ \mathrm{m\,s^{-1}}$ |
| Maximum velocity difference | $3.85\times10^{-2}\ \mathrm{m\,s^{-1}}$ | $3.36\times10^{-2}\ \mathrm{m\,s^{-1}}$ |
| Maximum prescribed Stokes velocity | $4.11\times10^{-2}\ \mathrm{m\,s^{-1}}$ | $3.99\times10^{-2}\ \mathrm{m\,s^{-1}}$ |

The combined vector metrics are:
- Vector amplitude ratio: `0.9665`
- Vector alignment: `0.9880`
- Relative vector RMSE: `0.1546`

The approximately 0.99 correlations and vector alignment show that the initial velocity difference closely follows the prescribed Stokes velocity. The amplitude ratio indicates that the difference retains approximately 96.6% of the Stokes amplitude along the Stokes direction.

Exact equality is not expected because the comparison is made after one one-hour model step and because the Stokes field passes through cell-to-edge projection and edge-to-cell velocity reconstruction.

### Conclusion
**The sanity checks show that:**
- ERA5 Stokes fields are received and reconstructed successfully.
- Cell-centered Stokes velocity is mapped successfully to edge-normal velocity.
- The prognostic velocity receives the expected Lagrangian Stokes contribution.
- The vorticity, vertical-shear, time-derivative, and wavy-hydrostatic terms activate according to their required model state.
- The total Stokes momentum RHS is assembled accurately.
- The with-Stokes minus control velocity strongly agrees with the prescribed Stokes velocity during the first evolved timestep.

These tests provide strong numerical evidence that the Stokes forcing pathway is active and wired correctly. They validate the implementation mechanics, while longer physical validation is still needed for signs, conservation behavior, pressure response, and long-term ocean adjustment.

## Standalone Stokes Forcing module Test Modes 120-125
This test suite lives in:
- `src/ocean/testbed/mo_ocean_testbed_stokes_forcing.f90`
- dispatched through `src/ocean/testbed/mo_ocean_testbed_operators.f90`

The purpose is to test each new Stokes-forcing operator in isolation before wiring the terms into the real ICON-o time stepping.
Important convention: the routines return the **raw mathematical product**. They do not apply the final sign from the momentum equation. For example, `stokes_vertical_shear_tendency` returns `+ w^L * d_z(v_s)`, while the later momentum equation caller will subtract it.

### What Each Mode Tests

| Mode | Term Tested | Main Quantity Checked | Expected |
|---|---|---|---|
| 120 | surface `w^L d_z(v_s)` | raw top `w*dudz` | `4` |
| 120 | wavy hydrostatic | `v^L · d_z(v_s)` level 1 | `0.5` |
| 121 | interior `w^L d_z(v_s)` | raw interior `w*dudz` | `3` |
| 121 | wavy hydrostatic | `v^L · d_z(v_s)` level 1 | `0.5` |
| 122 | Stokes time tendency | `(vn_now - vn_old)/dt` | `1` |
| 123 | first-step time tendency | first-step output | `0` |
| 124 | Stokes vorticity tendency | zero Stokes input | `ζ_s = 0`, `vort = 0` |
| 125 | Stokes vorticity tendency | linearity under `vn_s -> 2 vn_s` | scaled output doubles |

### Practical Run Notes
To run a mode, edit:
```text
build_intel_test/run/exp.oce_testbed_stokes.run
```
and set:
```text
test_mode = 120
```
or:
```text
test_mode = 121
test_mode = 122
test_mode = 123
test_mode = 124
test_mode = 125
```
Changing only `test_mode` does not require recompilation.
A successful test should show lines like:
```text
STOKES-CHECK passed: ...
Script run successfully: OK
```
A failed test will show:
```text
STOKES-CHECK failed: ...
```
and ICON will stop via `finish()`.
### Result interpretation
```text
STOKES-TEST mode:         120
 0:  STOKES-TEST local sample jc/jb:           1           1
 !! Raw local diagnostics
 !! hand-computed local mathematical diagnostics at one sample cell
 0:  STOKES-TEST local profile surf/u1/u2/w: surface u_s/level-1 u_s/level-2 u_s/prescribed w^L  
 0:  STOKES-TEST local top invdz/dz_u/wdz_u:       
 0:  STOKES-TEST local interior invdz/dz_u/wdz_u:   
 0:  STOKES-TEST local hydro expected/source: raw/actual model routine output from wavy_hydrostatic_source
 
 !! Model output from mo_ocean_stokes_forcing routines
 !! after ICON mapping/stencils where relevant
 !! max/min/sample: MAXVAL()/MINVAL()/hard-coded index (1,1,1) for 3D edge/vertex fields 
 !! or cell (1,1) for hydro/profile fields
 0:  STOKES-TEST vn_s max/min/sample:  output from stokes_cell_to_edge_normal
 0:  STOKES-TEST shear max/min/sample: output from stokes_vertical_shear_tendency
 0:  STOKES-TEST zeta max/min/sample:  output from stokes_vorticity_tendency
 0:  STOKES-TEST vort max/min/sample:  output from stokes_vorticity_tendency
 0:  STOKES-TEST time max/min/sample:  output from stokes_time_tendency
 0:  STOKES-TEST hydro source max/min/sample:  output from wavy_hydrostatic_source
 0:  STOKES-TEST vorticity scale err/base: for mode 125, errors in the linearity check plus the base vortex max.
```

**Test Results**

|Mode|Log|Main Purpose|Result|
|---|---|---|---|
|120|26877123|surface Stokes jump|Passed|
|121|26877234|first interior Stokes jump|Passed|
|122|26877358|exact time tendency|Passed|
|123|26877460|first-step time tendency|Passed|
|124|26877489|zero Stokes vorticity|Passed|
|125|26877563|vorticity linearity|Passed|

I found no STOKES-TEST check failed, FATAL, ERROR, abort, or NaN markers in the six logs.

**Important Observations**
- Mode 120: local raw top value is exactly as designed: surf=1, u1=0, w=2, invdz=2, so raw w dz_u = 4. The final edge-normal shear sample is 0.8669063, because it has gone through ICON geometric mapping.
- Mode 121: local raw interior value is exactly as designed: surf=1, u1=1, u2=0, w=3, so top derivative is zero and interior w dz_u = 3. Hydro source is 0.5.
- Mode 122: time tendency is exactly 1 everywhere: old field is now - 2, dt=2.
- Mode 123: first-step flag forces time tendency to zero.
- Mode 124: zero Stokes gives exactly zero ζ_s and zero vortex tendency. The printed time=1 is unrelated to this test; it is just the default old/new setup still being printed.
- Mode 125: direct edge-normal Stokes pattern gives nonzero ζ_s and vortex tendency. Scaling vn_s by two gives exactly twice the vorticity and vortex tendency: both scale errors are 0, so the updated vorticity stencil behaves linearly as expected.

### Common Setup
Most modes share this synthetic setup:
```text
n_zlev = 2
dzlev_m(1:2) = 1.0, 1.0
```
So approximately:
```text
surface to level-1 midpoint distance = 0.5
level-1 midpoint to level-2 midpoint distance = 1.0

invdz_top = 2.0
invdz_int = 1.0
```
The testbed initializes:
```text
v_s = 0
surface v_s = 0
w_L = w_value
v_L at cells = (1, 0, 0)
v_L edge-normal = 1
```
The checks use `check_close(...)`. If a check fails, ICON stops with:
```text
STOKES-CHECK failed
```
If it passes, the log prints:
```text
STOKES-CHECK passed
```

### Mode 120: Surface Vertical Shear + Wavy Hydrostatic Source
#### Purpose
Tests the surface-boundary part of:
```text
w^L * d_z(v_s)
```
and also tests:
```text
v^L · d_z(v_s)
```
for the wavy-hydrostatic source.
#### Synthetic Design
```text
surface u_s = 1
level-1 u_s = 0
level-2 u_s = 0
v_s = 0
w_L = 2
v_L = (1, 0, 0)
```
So only the surface-to-level-1 derivative is nonzero.
#### Raw Expected Vertical Shear
Top interface:
```text
d_z u_s = (u_s(surface) - u_s(level 1)) * invdz_top
        = (1 - 0) * 2
        = 2

w_L * d_z u_s = 2 * 2
              = 4
```
Interior interface:
```text
d_z u_s = (u_s(level 1) - u_s(level 2)) * invdz_int
        = (0 - 0) * 1
        = 0

w_L * d_z u_s = 0
```
#### Expected Check Results
```text
surface raw wdz_u = 4
surface interior raw wdz_u = 0
```
#### Wavy-Hydrostatic Expected Result
The hydrostatic source uses `d_z(v_s)`, not `w_L * d_z(v_s)`:
```text
v^L · d_z(v_s)
```
Since `v_L = (1,0,0)`, this reduces to the mapped `d_z u_s`.
ICON maps interface values to layer centers using:
```text
center(level 1)
= (zdist_top * dz_top + zdist_int * dz_int) / (2 * prism_thick)
```
For mode 120:
```text
dz_top = 2
dz_int = 0
zdist_top = 0.5
zdist_int = 1.0
prism_thick = 1.0

hydro_source(level 1)
= (0.5 * 2 + 1.0 * 0) / (2 * 1.0)
= 0.5
```
Expected:
```text
hydro source level 1 = 0.5
```

#### Notes
The final `shear_tend` printed in the log is after:
1. interface-to-layer-center mapping;
2. cell-vector to edge-normal projection.

So it is not expected to be `4`. In our logs it was around:
```text
shear_tend sample ≈ 0.8669
```
That is diagnostic, not the hard-coded unit-test value.

### Mode 121: Interior Vertical Shear + Wavy Hydrostatic Source
#### Purpose
Tests the interior vertical derivative path of:
```text
w^L * d_z(v_s)
```
and again tests:
```text
v^L · d_z(v_s)
```
for the wavy-hydrostatic source.
#### Synthetic Design
```text
surface u_s = 1
level-1 u_s = 1
level-2 u_s = 0
v_s = 0
w_L = 3
v_L = (1, 0, 0)
```
So the surface derivative is zero, and only the level-1 to level-2 derivative is nonzero.
#### Raw Expected Vertical Shear
Top interface:
```text
d_z u_s = (u_s(surface) - u_s(level 1)) * invdz_top
        = (1 - 1) * 2
        = 0

w_L * d_z u_s = 0
```
Interior interface:
```text
d_z u_s = (u_s(level 1) - u_s(level 2)) * invdz_int
        = (1 - 0) * 1
        = 1

w_L * d_z u_s = 3 * 1
              = 3
```
#### Expected Check Results
```text
interior surface raw wdz_u = 0
interior raw wdz_u = 3
```
#### Wavy-Hydrostatic Expected Result
Again, hydrostatic source uses `d_z u_s`, not `w_L * d_z u_s`.
```text
dz_top = 0
dz_int = 1

hydro_source(level 1)
= (0.5 * 0 + 1.0 * 1) / (2 * 1.0)
= 0.5
```
Expected:
```text
hydro source level 1 = 0.5
```
#### Notes
The final mapped/projection `shear_tend` is expected to be around:
```text
1.30036
```

because roughly:
```text
mapped center value ≈ 1.5
edge projection factor ≈ 0.8669
1.5 * 0.8669 ≈ 1.30035
```

Again, the hard check is on the raw derivative/product and hydro source, not on this geometry-dependent edge value.

### Mode 122: Exact Stokes Time Tendency
#### Purpose
Tests:
```text
d(v_s)/dt = (v_s(now) - v_s(old)) / dt
```
#### Synthetic Design
The test sets:
```text
stokes_vn_old = stokes_vn_now - 2
dt = 2
is_first_timestep = false
```
Therefore:
```text
d(v_s)/dt = (v_now - (v_now - 2)) / 2
          = 2 / 2
          = 1
```
#### Expected Check Result
Every entry of `time_tend` should be exactly:
```text
1
```
The check is:
```text
max(abs(time_tend - 1)) = 0
```

### Mode 123: First-Step Stokes Time Tendency
#### Purpose
Tests the special first-step behavior of:
```text
stokes_time_tendency
```
At the first timestep, the caller may not have a valid old Stokes velocity yet, so the routine should return zero.
#### Synthetic Design
```text
is_first_timestep = true
```
Even though `stokes_vn_old` is set later in the testbed, the routine ignores it because this is the first step.
#### Expected Check Result
```text
time_tend = 0 everywhere
```
The check is:
```text
max(abs(time_tend)) = 0
```

### Mode 124: Zero Stokes Vorticity Tendency
#### Purpose
Tests the zero-input behavior of:
```text
ζ_s * zhat × v^L
```
where `ζ_s` is derived from `stokes_vn`.
#### Synthetic Design
```text
stokes_vn = 0
v_L edge-normal = 1
```
Since the Stokes velocity is zero, its curl/vorticity should also be zero:
```text
ζ_s = curl(v_s) = 0
```
Therefore:
```text
ζ_s * zhat × v^L = 0
```
#### Expected Check Results
```text
max(abs(stokes_zeta)) = 0
max(abs(vort_tend)) = 0
```
This is a clean exact zero test for the Stokes-vortex operator.

### Mode 125: Stokes Vorticity Linearity Check
#### Purpose
Tests the linearity of:
```text
ζ_s = curl(v_s)
```
and the resulting vortex tendency:
```text
ζ_s * zhat × v^L
```
This avoids trying to hand-derive an exact curl value on the ICON grid.
#### Synthetic Design
Instead of starting from cell-centered `u_s/v_s`, this mode directly prescribes an edge-normal Stokes pattern:
```text
stokes_vn(edge, level, block)
= mod(edge + 3*block + 5*level, 7) + 1
```
Then the test computes the vorticity tendency once with:
```text
stokes_vn
```
and again with:
```text
2 * stokes_vn
```
Because curl is linear:
```text
curl(2 * v_s) = 2 * curl(v_s)
```
and because `v_L` is unchanged:
```text
vort_tend(2 * v_s) = 2 * vort_tend(v_s)
```
#### Expected Check Results
```text
max(abs(zeta_scaled - 2*zeta_base)) = 0
max(abs(vort_scaled - 2*vort_base)) = 0
```
The log prints:
```text
STOKES-TEST vorticity scale err/base:
  zeta_scale_error, vort_scale_error, vort_base_max
```
Expected:
```text
zeta_scale_error = 0
vort_scale_error = 0
```
`vort_base_max` is diagnostic only, just to confirm the base pattern is not trivially zero.



# Stokes Forcing implementation Plan

## [[2026-11-06]] Z-star coordinate implementation: 2nd Stage
The second stage solve:
- option for applying velocity diffusion to Eulerian velocity
- split prognostic velocity/flux to Stokes/Eulerian contributions
### Implemented Plan 
Z-star Eulerian diffusion/mixing and split Stokes diagnostics
- ==**Summary**==: 
	- Extend the successful CPU-only z-star noWavy Stokes implementation so velocity diffusion and vertical mixing can operate on reconstructed Eulerian velocity, and enable the existing split Eulerian/Stokes eddy diagnostics. 
	- Follow ordinary ICON’s native z-star algorithms: retain `stretch_c` in cell-centered vertical gradients and `stretch_e` in edge-centered implicit diffusion. Reuse the common Eulerian reconstruction and diagnostic routines. Leave z-level behavior untouched.
- ==**Implementation changes**==
	- In the z-star driver, after Stokes profile preparation and cold-start/restart activation, call the existing `prepare_eulerian_velocity` from `mo_ocean_stokes_forcing.f90`:
	  - Reconstruct source-time `eulerian_vn = lagrangian_vn - stokes_vn_old`.
	  - Produce cell vectors, dual vectors, vorticity, and—when `stokes_eddydiag>0`—local `eulerian_u/v`.
	  - Pass Eulerian fields to z-star physics only when `l_stokes_eulerian_velocity_diffusion=.TRUE.`.
	- In z-star momentum timestepping:
	  - When Eulerian diffusion is enabled, call the existing horizontal `velocity_diffusion` with `eulerian_vn`, `eulerian_vort`, and `eulerian_vec_c`.
	  - Before PP predictor processing and native z-star implicit vertical diffusion, subtract current `stokes_vn` from `vn_pred`.
	  - Run the existing z-star solver with `stretch_e`, then add `stokes_vn` back.
	  - Preserve the current prognostic-Lagrangian path exactly when the switch is false.
	- Extend the native z-star physics interfaces with optional velocity inputs:
	  - `update_ho_params_zstar` and `calc_vertical_stability_zstar` select Eulerian cell velocity when supplied.
	  - Preserve the existing `stretch_c` correction in physical vertical shear.
	  - Forward Eulerian velocity to `calc_tke`; its existing z-star metric treatment remains unchanged.
	  - Extend `update_PP_scheme_zstar` and `ICON_PP_Edge_scheme_zstar` similarly.
	  - For `PPscheme_ICON_Edge_vnPredict_type`, use the already shifted Eulerian predictor.
	  - IDEMIX remains unchanged; only its TKE component receives Eulerian velocity.
	- Reuse the existing split-diagnostic infrastructure unchanged:
	  - `calc_stokes_eddydiag` continues to calculate `stokes_u/v = lagrangian_u/v - eulerian_u/v`.
	  - `stokes_eddydiag=1` produces velocity and temperature-product splits.
	  - `stokes_eddydiag=2` additionally produces salinity-product splits.
	  - Products remain pointwise on moving z-star model layers; no remapping or additional metric factor is introduced.
	- Update namelist validation:
	  - Permit Eulerian reconstruction, Eulerian diffusion, and split diagnostics for `vert_cor_type=1`.
	  - Retain requirements for Stokes forcing, mimetic 3-D dynamics, noWavy hydrostatics, and CPU execution.
	  - Retain `stokes_eddydiag>0` requiring `eddydiag=.TRUE.`.
	  - Reject z-star Eulerian diffusion with nonzero `LeithClosure_order`; ordinary ICON’s native `update_ho_params_zstar` does not provide the corresponding dynamic Leith update.
	  - Add no new namelist switches.
- ==**Interfaces and invariants**==
	- Add optional Eulerian velocity arguments only to internal z-star physics/PP interfaces; add no state, restart, or output variables.
	- Continue using `stokes_vn_old` for source-time Eulerian reconstruction and current `stokes_vn` for predictor shift-out/shift-back.
	- Keep profile reconstruction in `mo_ocean_stokes_forcing_zstar`; do not create `prepare_eulerian_velocity_zstar`.
	- Preserve cold-start, Eulerian-restart activation, and normal Stokes-restart handling.
	- Preserve the original no-Stokes and z-level arithmetic branches.
- ==**Deferred work**==
	- Wavy-hydrostatic pressure and Stokes vertical-shear tendency.
	- Z-star vertical-gradient metric treatment.

### Main Change
- Reconstructs Eulerian velocity after z-star Stokes preparation using `stokes_vn_old` in [mo_hydro_ocean_run.f90 (line 1233)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:1233).
- Applies horizontal and vertical diffusion to Eulerian velocity while retaining the native `stretch_e` solver in [mo_ocean_ab_timestepping_zstar.f90 (line 800)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_zstar.f90:800).
- Routes Eulerian cell velocity through z-star stability, TKE, and IDEMIX+TKE while retaining `stretch_c` metrics in [mo_ocean_physics.f90 (line 978)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/physics/mo_ocean_physics.f90:978).
- Extends the z-star PP path to select Eulerian shear in [mo_ocean_pp_scheme.f90 (line 251)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/physics/mo_ocean_pp_scheme.f90:251).
- Enables both features for z-star and rejects dynamic Leith closure with Eulerian diffusion in [mo_ocean_nml_crosscheck.f90 (line 164)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/config/mo_ocean_nml_crosscheck.f90:164).

The existing state allocation and `calc_stokes_eddydiag` routines were reused unchanged. No state, restart, output variables, or z-level routines were added or modified. Source/interface and diff whitespace checks passed. As requested, I did not compile or run simulations.

---

## [[2026-11-05]] Z-star coordinate implementation: 1st Stage
The first stage solve:
- add Stokes forces to the momentum equation timestepping
- namelist switch on, allow cold start and restart from Eulerian
### Implemented Plan
Z-star noWavy Stokes forcing
- ==**Summary**==
	- Implement an isolated z-star Stokes path for R2B7L72 supporting Stokes time and vorticity tendencies, cold starts, Eulerian-restart activation, and normal Stokes restarts.
	- Keep the existing z-level profile reconstruction and timestep path unchanged. Compilation and all runtime testing will be performed by you.
- ==**Implementation changes**==
	- Add `mo_ocean_stokes_forcing_zstar.f90` containing:
	  - Z-star Phillips-profile reconstruction using physical depth below the moving surface:
	    `abs(depth_CellMiddle) * stretch_c`.
	  - `prepare_stokes_fields_zstar`, reconstructing `ust_3d/vst_3d` before calling the common Stokes mapping routines.
	  - A noWavy momentum wrapper reusing the existing time-tendency and vorticity operators.
	  - CPU-only support initially.
	- In the z-star driver:
	  - Reconstruct and prepare the profile after ERA5 forcing, z-star coordinate updates, and any IAU update.
	  - Reconstruct every timestep, including timesteps without new ERA5 data.
	  - On cold start, add Stokes velocity once to the incoming Eulerian velocity and initialize `stokes_vn_old`.
	  - On Eulerian-restart activation, perform the same one-time conversion, synchronize velocity state, recompute velocity diagnostics and z-star vertical velocity, and mark the step as an activation step.
	  - On a normal Stokes restart, retain the restarted Lagrangian velocity and `stokes_vn_old` without another shift.
	  - Reset `l_stokes_activation_step` at the start of every z-star timestep.
	- In z-star momentum timestepping:
	  - Compute only Stokes vorticity and time tendencies.
	  - Add `stokes_rhs` to the explicit z-star momentum RHS.
	  - Keep the exact fixed-layer derivative:
	    `(stokes_vn_now(k)-stokes_vn_old(k))/dtime`.
	  - Use the existing constant-model-level curl operator, matching native z-star horizontal momentum.
	  - Reset Adams–Bashforth history on cold start and Eulerian-restart activation.
	  - Preserve the original no-Stokes arithmetic branch.
	  - Keep shear and wavy-hydrostatic source fields zero.
	- Update namelist validation:
	  - Permit `l_stokes_from_eulerian_restart` for `vert_cor_type=1`.
	  - Require mimetic, non-shallow-water, noWavy configuration for z-star Stokes.
	  - Reject z-star Stokes with wavy hydrostatics, Eulerian velocity diffusion, nonzero `stokes_eddydiag`, or OpenACC.
	  - Add no new namelist switches.
- ==**Interfaces and invariants**==
	- Export:
	  - `prepare_stokes_fields_zstar(..., stretch_c, ..., ust_3d, vst_3d, ...)`
	  - `calculate_stokes_momentum_rhs_zstar_nowavy(...)`
	- Extend internal z-star free-surface interfaces with `is_stokes_activation_step`.
	- Add no state variables, restart variables, or output fields.
	- Keep `stokes_vn_old` as the restart-continuation field.
	- Grid motion influences the newly reconstructed `stokes_now(k)` through `stretch_c`; do not add ALE corrections, remapping, or an explicit grid-velocity term.
	- Do not modify the existing z-level reconstruction or z-level timestepping implementation.
- ==**Deferred work**==
	- Wavy-hydrostatic pressure and Stokes vertical-shear tendency.
	- Z-star vertical-gradient metric treatment.
	- Eulerian velocity diffusion and mixing.
	- Split Eulerian/Stokes eddy diagnostics.

### Main changes
- Added [mo_ocean_stokes_forcing_zstar.f90 (line 46)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_stokes_forcing_zstar.f90:46):
    - Phillips-profile reconstruction with `abs(depth_CellMiddle) * stretch_c`.
    - Per-timestep z-star preparation using the existing mapping operators.
    - CPU-only noWavy momentum wrapper reusing the existing time and vorticity tendencies.
    - The time derivative remains exactly:   
        ```
        (stokes_vn_now - stokes_vn_old) / dtime
        ```    
- Updated the z-star driver in [mo_hydro_ocean_run.f90 (line 515)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:515):
    - Reconstructs Stokes fields every timestep after forcing, coordinate updates, and IAU.
    - Handles cold-start Eulerian-to-Lagrangian conversion.
    - Handles one-time Eulerian-restart activation and resets the activation flag each timestep.
    - Recomputes velocity diagnostics and z-star vertical velocity after conversion.
    - Leaves normal Stokes restarts unshifted.
- Updated [mo_ocean_ab_timestepping_zstar.f90 (line 689)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_zstar.f90:689):
    - Computes only Stokes vorticity and time tendencies.
    - Adds `stokes_rhs` to the explicit momentum RHS.
    - Resets Adams–Bashforth history for cold starts and activation steps.
    - Preserves the original no-Stokes RHS expression and operation ordering.
    - Keeps shear and wavy-hydrostatic sources zero.
- Updated validation in [mo_ocean_nml_crosscheck.f90 (line 117)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/config/mo_ocean_nml_crosscheck.f90:117):
    - Allows Eulerian-restart activation for z-star.
    - Requires mimetic, non-shallow-water, noWavy configuration.
    - Rejects z-star Stokes with Eulerian diffusion, split diagnostics, wavy hydrostatics, or OpenACC.

### Stokes profile reconstruction on z-star
compare to the z-level (previous) implementation, two implementations use the same Phillips profile and the same cell-to-edge mapping. The important difference is when physical layer depth can change.
==🟢Z-level==
```
couple_ocean_to_era5_provider
  └─ reconstruct_era5_stokes_profile_phillips
       └─ produces p_as%ust_3d and p_as%vst_3d

calculate_explicit_term
  └─ prepare_stokes_fields
       ├─ local u/v → Cartesian cell vector
       └─ Cartesian cell vector → stokes_vn on edges
```
The relevant routines are:
- ERA5 reception: [couple_ocean_to_era5_provider (line 212)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/coupling/mo_ocean_era5_provider_coupling.f90:212)
- Reconstruction call after receiving new data: [coupling line 350 (line 350)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/coupling/mo_ocean_era5_provider_coupling.f90:350)
- Actual z-level reconstruction: [reconstruct_era5_stokes_profile_phillips (line 373)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/coupling/mo_ocean_era5_provider_coupling.f90:373)
- Mapping inside z-level momentum: [mo_ocean_ab_timestepping_mimetic.f90 (line 464)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90:464)
- Common mapping routine: [prepare_stokes_fields (line 62)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_stokes_forcing.f90:62)
The reconstruction only occurs when new ERA5 data are actually received. Between coupling times, the previous `ust_3d/vst_3d` profile is retained.
That is sufficient for z-level because `depth_CellMiddle` never moves.
==🟢Z-star==
```
update_ocean_surface_refactor
  └─ receive/retain surface Stokes fields

update_zstar_variables
  └─ update stretch_c from the current free surface

prepare_stokes_fields_zstar
  ├─ reconstruct_stokes_profile_phillips_zstar
  │    └─ produces stretch-aware ust_3d/vst_3d
  └─ prepare_stokes_fields
       └─ common cell-vector and edge-normal mapping
```
The relevant routines are:
- Surface-forcing update: [mo_hydro_ocean_run.f90 (line 1162)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:1162)
- Z-star-coordinate update: [mo_hydro_ocean_run.f90 (line 1170)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:1170)
- Per-timestep z-star preparation: [mo_hydro_ocean_run.f90 (line 1212)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:1212)
- Z-star wrapper: [prepare_stokes_fields_zstar (line 46)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_stokes_forcing_zstar.f90:46)
- Actual z-star reconstruction: [reconstruct_stokes_profile_phillips_zstar (line 128)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_stokes_forcing_zstar.f90:128)
==🔴**This path runs every ocean timestep, including timesteps without new ERA5 data.**

> [!Attention] 
> No mathematical difference, only the physical depth $d$ differs:
> - Z-level: `abs(depth_CellMiddle)`
> - Z-star: `abs(depth_CellMiddle) * strech_c`
> 	- the reference-coordinate depth multiple by `strech_c` converts it into the current physical depth below the moving surface
> 	- No separate SSH displacement is needed because z-star stretches the entire water column relative to the instantaneous surface.

In both cases, the reconstructed Stokes velocities ultimately live on model layers:
- `ust_3d/vst_3d`: cell centers at every model level \(k\)
- `stokes_vec_c`: Cartesian cell vector at level \(k\)
- `stokes_vn`: edge-normal Stokes velocity at level \(k\)

For z-star, `stretch_c` is one value for the entire water column, not a separate value for each layer. If a column has: $$\text{stretch}_c < 1,$$
then every model layer in that column becomes physically thinner and its center moves closer to the instantaneous sea surface. For the second layer: $$d_2^{z^\star} = d_{2,\mathrm{reference}}\,\text{stretch}_c.$$
Therefore, if `stretch_c < 1`: $$d_2^{z^\star}<d_{2,\mathrm{reference}},$$
and because the Phillips Stokes profile decreases with depth: $$u^S_2(z^\star)>u^S_2(z\text{-level})$$
in magnitude, assuming the same surface Stokes velocity and transport. The reconstructed Stokes velocity becomes progressively larger because that model layer now samples a shallower part of the prescribed profile.

The important distinction is:
- At the same model-layer index \(k\), Stokes velocity changes because that layer moves physically.
- At the same physical depth, the prescribed analytical Stokes profile has not changed. it is just the point we select on that depth change, so the values will also change. So the actual profile is not "moving", it's the coordinate moving, and we are projecting the actual physical profile to the coordinate.
---

## [[2026-11-04]] Eulerian velocity diffusion, new diagnostics
### Implemented Plan:
Eulerian Diffusion, Mixing, and Split Stokes Diagnostics
- ==**Summary**==: Keep $\boldsymbol{u}^L$ as the only prognostic velocity, but reconstruct the source-time Eulerian velocity $$\boldsymbol{u}^{E,n}=\boldsymbol{u}^{L,n}-\boldsymbol{u}^{S,n}$$for velocity diffusion, TKE/PP shear calculations, velocity-dependent viscosity coefficients, and separate Eulerian/Stokes diagnostics. Existing behavior remains the default.
- ==**Configuration and State**==
	- Add `l_stokes_eulerian_velocity_diffusion=.FALSE.` to `ocean_physics_nml`.
	  - When enabled with `l_stokes_forcing=.TRUE.`, horizontal and vertical velocity diffusion and their mixing coefficients use Eulerian velocity.
	  - Log the selected diffusion velocity.
	- Add `stokes_eddydiag=0` to `ocean_diagnostics_nml`:
	  - `0`: disabled;
	  - `1`: Eulerian/Stokes $u,v$ and temperature products;
	  - `2`: additionally include salinity products.
	  - Require `eddydiag=.TRUE.` when nonzero so the legacy Lagrangian products remain available for comparison.
	- Allocate Eulerian working fields only when either new feature requires them:
	  - Eulerian edge-normal velocity;
	  - cell Cartesian vector;
	  - dual-grid vector and vorticity;
	  - local $u^E,v^E$ only when diagnostics are enabled.
	- Do not add prognostic or restart variables. Reconstruct all Eulerian fields after every cold start, Lagrangian restart, and Eulerian-restart activation.
	- Reject invalid diagnostic levels and fail clearly if the new functionality is requested with z-star or OpenACC.
- ==**Physics Implementation**==
	- After Stokes activation handling and before `update_ho_params`, construct $$v_n^{E,n}=v_n^{L,n}-v_{n,\mathrm{old}}^S.$$Reconstruct its cell, dual-grid, local-coordinate, and vorticity representations without overwriting the existing Lagrangian `p_diag` fields.
	- Pass the Eulerian velocity view into:
	  - Richardson-number/vertical-stability calculation;
	  - PP and TKE horizontal-shear calculations;
	  - TKE shear production and resulting $A_v$;
	  - velocity-dependent Leith viscosity calculations.
	- Leith forms use Eulerian horizontal vorticity/divergence. Variants involving $w$ retain the existing diagnosed $w$, consistent with the agreed absence of a separate vertical Stokes velocity.
	- Horizontal diffusion computes$$D_h\!\left(\boldsymbol{u}^{E,n}\right)$$
	  using the matching Eulerian vorticity or cell vector for both curl–curl and div–grad forms. The resulting tendency is still added to the Lagrangian momentum equation.
	- Apply vertical implicit diffusion through subtract–solve–add:$$\boldsymbol{r}^{E}=\widetilde{\boldsymbol{u}}^{L}-\boldsymbol{u}^{S,*},$$$$\left(I-\Delta tD_v[A_v^E]\right)\boldsymbol{u}^{E,*}=\boldsymbol{r}^{E},$$$$\boldsymbol{u}^{L,*}=\boldsymbol{u}^{E,*}+\boldsymbol{u}^{S,*}.$$
	  Use the same current `stokes_vn` for subtraction and addition, operate on `vn_pred`, and cover both standard and mass-matrix predictor branches. For `PPscheme_ICON_Edge_vnPredict_type`, subtract Stokes before calculating PP coefficients.
	- Leave momentum advection, tracer transport, pressure terms, surface stress, bottom drag, Langmuir source, Stokes RHS, wavy-hydrostatic logic, and `stokes_vn_old` lifecycle unchanged.
	- With the switch disabled, or without Stokes forcing, preserve the original calculation path.
- **Diagnostics**: Register the following cell-centred, non-restart fields beside the existing `eddydiag` variables in `groups_oce_eddy`:
	- Level 1:
	  - `eulerian_u`, `eulerian_v`, `stokes_u`, `stokes_v`;
	  - `eulerian_uT`, `eulerian_vT`;
	  - `stokes_uT`, `stokes_vT`.
	- Level 2:
	  - `eulerian_uS`, `eulerian_vS`;
	  - `stokes_uS`, `stokes_vS`.
	Use the legacy diagnostic time pairing: $$uT_{\mathrm{legacy}}=u^{L,n}T^{n+1},$$
	$$uT_{\mathrm{legacy}}=u^{E,n}T^{n+1}+u^{S,n}T^{n+1},$$
	with the analogous relations for $vT$ and salinity. Derive local Stokes velocity as Lagrangian minus reconstructed Eulerian velocity to guarantee spatial and temporal closure. Existing `uT`, `vT`, `wT`, and salinity diagnostics remain unchanged; no separated vertical products are added.
### Key changes:
Key changes:
- Added `l_stokes_eulerian_velocity_diffusion=.FALSE.` and `stokes_eddydiag=0/1/2`, including validation and logging in [mo_ocean_nml_crosscheck.f90 (line 142)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/config/mo_ocean_nml_crosscheck.f90:142).
- Added conditional Eulerian working fields and split diagnostic outputs in [mo_ocean_state.f90 (line 1371)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_state.f90:1371).
- Reconstructs source-time Eulerian velocity from \(v_n^L-v_{n,\mathrm{old}}^S\) in [mo_ocean_stokes_forcing.f90 (line 95)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_stokes_forcing.f90:95).
- Routes Eulerian velocity through horizontal diffusion, PP/TKE shear, Richardson stability and Leith viscosity.
- Implements vertical subtract–solve–add in both predictor branches in [mo_ocean_ab_timestepping_mimetic.f90 (line 665)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90:665).
- Adds Eulerian/Stokes velocity, temperature-product and optional salinity-product diagnostics in [mo_ocean_diagnostics.f90 (line 3527)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/diagnostics/mo_ocean_diagnostics.f90:3527).
- Adds testbed modes 127–130 for reconstruction, zero-Stokes behavior, diagnostic closure and predictor subtract/add in [mo_ocean_testbed_stokes_forcing.f90 (line 417)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/testbed/mo_ocean_testbed_stokes_forcing.f90:417).
### Summary
Keep the Lagrangian velocity as the only prognostic velocity, but reconstruct the source-time Eulerian velocity: $$v_n^{E,n}=v_n^{L,n}-v_n^{S,n}.$$
The reconstructed Eulerian velocity can optionally be used for:
- horizontal velocity diffusion;
- implicit vertical velocity diffusion;
- TKE/PP shear production and viscosity coefficients;
- velocity-dependent Leith viscosity;
- separate Eulerian and Stokes scalar-flux diagnostics.
Existing behaviour remains the default.
### Configuration
```
&ocean_physics_nml
  l_stokes_forcing                     = .TRUE.
  l_stokes_eulerian_velocity_diffusion = .TRUE.
/

&ocean_diagnostics_nml
  eddydiag        = .TRUE.
  stokes_eddydiag = 1
/
```
with the below options:

| Option                                         | Behaviour                                                |
| ---------------------------------------------- | -------------------------------------------------------- |
| `l_stokes_eulerian_velocity_diffusion=.FALSE.` | Diffusion and mixing use prognostic Lagrangian velocity  |
| `l_stokes_eulerian_velocity_diffusion=.TRUE.`  | Diffusion and mixing use reconstructed Eulerian velocity |
| `stokes_eddydiag=0`                            | No split diagnostics                                     |
| `stokes_eddydiag=1`                            | Eulerian/Stokes velocities and temperature products      |
| `stokes_eddydiag=2`                            | Additionally diagnose salinity products                  |
`stokes_eddydiag>0` requires both `l_stokes_forcing=.TRUE.` and `eddydiag=.TRUE.`

### Eulerian velocity reconstruction
At each source timestep:
$$\boldsymbol{u}^{E,n} = \boldsymbol{u}^{L,n}-\boldsymbol{u}^{S,n}.$$
The model reconstructs matching Eulerian representations:

```
eulerian_vn
eulerian_vec_c
eulerian_vn_dual
eulerian_vort
eulerian_u
eulerian_v
```

These are diagnostic/working fields only. No new prognostic or restart velocity is introduced. Working fields are allocated only when Eulerian diffusion or split diagnostics require them.

### Diffusion and Mixing
Horizontal diffusion uses the Eulerian edge velocity together with the matching Eulerian cell vector and vorticity: $$\boldsymbol{D}_h=\boldsymbol{D}_h\left(\boldsymbol{u}^{E,n}\right).$$
The Eulerian velocity is also used for:
- Richardson-number and vertical-stability calculations;
- TKE shear production;
- PP shear calculations;
- resulting vertical viscosity coefficients;
- velocity-dependent harmonic and biharmonic Leith closures.

Leith variants involving vertical velocity retain the existing diagnosed \(w\).
==**Implicit vertical diffusion uses subtract–solve–add**:==$$\boldsymbol{r}^{E} = \widetilde{\boldsymbol{u}}^{L} - \boldsymbol{u}^{S,*}, $$$$ \left(I-\Delta tD_v[A_v^E]\right)\boldsymbol{u}^{E,*} = \boldsymbol{r}^{E}, $$$$ \widetilde{\boldsymbol{u}}^{L,*} = \boldsymbol{u}^{E,*} + \boldsymbol{u}^{S,*}. $$
The operation is applied directly to `vn_pred` in both standard and mass-matrix **predictor** branches.
### Split diagnostics
The following non-restart fields are registered in `groups_oce_eddy`.
For `stokes_eddydiag=1`:
```
eulerian_u
eulerian_v
stokes_u
stokes_v
eulerian_uT
eulerian_vT
stokes_uT
stokes_vT
```
For `stokes_eddydiag=2`, additionally:
```
eulerian_uS
eulerian_vS
stokes_uS
stokes_vS
```
The diagnostics follow the legacy `eddydiag` time pairing and satisfy:
$$u^L T=u^E T+u^S T, \qquad v^L T=v^E T+v^S T, $$
with analogous salinity closure at level 2.
Local diagnostic Stokes velocity is calculated as:
$$\boldsymbol{u}^S = \boldsymbol{u}^L-\boldsymbol{u}^E, $$
which guarantees closure after spatial reconstruction. These `stokes_u` and `stokes_v` diagnostics are distinct from the prescribed `era5_ust_3d` and `era5_vst_3d` forcing fields.
Existing `uT`, `vT`, `wT`, `uS`, `vS`, and `wS` diagnostics remain unchanged. No separated vertical Stokes products are added.
### Changed Fortran Files
| File                                                      | Change                                                                                                  |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `src/ocean/config/mo_ocean_nml.f90`                       | Adds `l_stokes_eulerian_velocity_diffusion` and `stokes_eddydiag`                                       |
| `src/ocean/config/mo_ocean_nml_crosscheck.f90`            | Validates switch combinations, supported configurations and startup logging                             |
| `src/ocean/dynamics/mo_ocean_types.f90`                   | Adds Eulerian working fields and split diagnostic fields                                                |
| `src/ocean/dynamics/mo_ocean_state.f90`                   | Conditionally allocates Eulerian fields and registers split output variables                            |
| `src/ocean/dynamics/mo_ocean_stokes_forcing.f90`          | Adds Eulerian reconstruction and predictor Stokes shift routines                                        |
| `src/ocean/drivers/mo_hydro_ocean_run.f90`                | Reconstructs source-time Eulerian velocity and passes it into mixing physics                            |
| `src/ocean/physics/mo_ocean_physics.f90`                  | Routes optional Eulerian velocity, vorticity and edge velocity through stability and Leith calculations |
| `src/ocean/physics/mo_ocean_pp_scheme.f90`                | Uses Eulerian shear in PP viscosity calculations                                                        |
| `src/ocean/physics/mo_ocean_tke.f90`                      | Uses Eulerian shear for TKE production and viscosity                                                    |
| `src/ocean/dynamics/mo_ocean_velocity_diffusion.f90`      | Allows horizontal diffusion to use matching Eulerian vector and vorticity fields                        |
| `src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90` | Selects Eulerian horizontal diffusion and implements vertical subtract–solve–add                        |
| `src/ocean/diagnostics/mo_ocean_diagnostics.f90`          | Calculates Eulerian/Stokes velocity and scalar-product diagnostics                                      |
| `src/ocean/testbed/mo_ocean_testbed_stokes_forcing.f90`   | Adds test modes 127–130                                                                                 |
| `src/ocean/testbed/mo_ocean_testbed_operators.f90`        | Extends Stokes testbed dispatch through mode 130                                                        |

### Compatibility and Restrictions
- `l_stokes_forcing=.FALSE.` follows the original no-Stokes physics path.
- The new diffusion switch defaults to `.FALSE.`.
- Split diagnostics default to disabled.
- Optional velocity arguments fall back to the original Lagrangian fields.
- Existing Stokes RHS, wavy-hydrostatic source and `stokes_vn_old` lifecycle are unchanged.
- Initial support is restricted to the CPU, z-level, mimetic ocean configuration.
- OpenACC, z-star and shallow-water configurations are rejected when the new functionality is requested.
### 🌟Restart Operational Note
For the first continuation from an Eulerian spin-up:
```
l_stokes_forcing               = .TRUE.
l_stokes_from_eulerian_restart = .TRUE.
```
After writing the first Lagrangian restart, all subsequent Stokes restarts must use:
```
l_stokes_forcing               = .TRUE.
l_stokes_from_eulerian_restart = .FALSE.
```
The activation segment must not automatically continue with the transition flag still enabled, because that would add Stokes velocity again.

---

## [[2026-10-27]] Add switch-on Wavy-hydrostatic Energy Pair
### Summary
Add the default-enabled namelist switch:
```
LOGICAL :: l_stokes_wavy_hydrostatic = .TRUE.
```
It jointly controls the energy-consistent pair
$$ S_w=\boldsymbol{u}^{L}\cdot\partial_z\boldsymbol{u}^{S}, \qquad \boldsymbol{T}_{\mathrm{shear}} =w^{L}\partial_z\boldsymbol{u}^{S}. $$
Disabling the switch removes both terms while retaining the Stokes vorticity and time tendencies.
### Configuration
```
&ocean_physics_nml
  l_stokes_forcing           = .TRUE.
  l_stokes_wavy_hydrostatic  = .TRUE.   ! or .FALSE.
/
```

|Stokes forcing|Wavy-hydrostatic switch|Behaviour|
|---|---|---|
|`.FALSE.`|either|Original no-Stokes behaviour|
|`.TRUE.`|`.TRUE.`|Full wavy-pressure and Stokes RHS|
|`.TRUE.`|`.FALSE.`|No wavy-pressure source or shear tendency; vorticity and time tendencies remain|

### Timestepping Logic
When enabled: $$ \mathrm{stokes\_rhs} = \boldsymbol{T}_{\mathrm{shear}} +\boldsymbol{T}_{\mathrm{vort}} +\boldsymbol{T}_{\mathrm{time}}. $$
The wavy-hydrostatic source is calculated and passed to the hydrostatic-pressure integration.
When disabled: $$ S_w=0,\qquad \boldsymbol{T}_{\mathrm{shear}}=0, $$$$ \mathrm{stokes\_rhs} = \boldsymbol{T}_{\mathrm{vort}} +\boldsymbol{T}_{\mathrm{time}}. $$
The ordinary hydrostatic-pressure integration is used without the optional wave source. Stokes fields are still prepared, and `stokes_vn_old` is still updated every timestep.

### Restart Behaviour
`l_stokes_from_eulerian_restart` remains independent and unchanged:
- Eulerian-to-Lagrangian restart conversion is preserved.
- First-step Stokes time-derivative suppression is preserved.
- Restart activation is not controlled by `l_stokes_wavy_hydrostatic`.

### Diagnostics and Logging
The existing diagnostics remain registered:
```
wavy_hydrostatic_source
stokes_shear_tend
stokes_vort_tend
stokes_time_tend
stokes_rhs
```
When the pair is disabled, the first two should be zero. Startup logging explicitly reports:
```
Wavy-hydrostatic source and Stokes vertical-shear tendency are disabled
```

### Validation Status
- NAG clean build: successful.
- Intel clean build: successful.
- Nine-day R2B4L40 experiment with the pair disabled: successful.
- Runtime log confirms the pair was disabled.
- Testbed mode `126` was added to check enabled/disabled RHS composition and `stokes_vn_old` updates, but has not yet been executed.

---

## [[2026-08-21]] Fix no-Stokes to with-Stokes Restart Transition
### Summary
Add an explicit, one-time conversion from an Eulerian no-wave restart to the Lagrangian velocity convention:
$$
v_n^L=v_n^E+v_n^s.
$$
The activation step will rebuild dependent velocity diagnostics, initialize Stokes history, and restart Adams-Bashforth at first order. Cold starts, no-wave continuations, and wave-to-wave restarts remain unchanged.
Implementation will include static inspection only. All compilation and simulation testing will be performed by the user.
### Assumptions
- The incoming transition restart contains Eulerian `vn`.
- Existing restart storage for `g_n`, `g_nm1`, `w`, and `stokes_vn_old` remains unchanged.
- The restart file’s timestamped state remains Eulerian; the first post-restart model state becomes Lagrangian.
- Fresh-start `00:00` output reordering is outside this change.
- Initial support is limited to the non-zstar mimetic Adams-Bashforth pathway.
### Configuration
Add to `ocean_physics_nml`:
```fortran
LOGICAL :: l_stokes_from_eulerian_restart = .FALSE.
```

Define these modes:

| Stokes forcing | Transition option | Behavior |
|---|---|---|
| false | false | Original ICON-O |
| true | false, cold start | Existing Stokes cold start |
| true | false, restart | Restart `vn` is assumed Lagrangian |
| true | true, restart | Restart `vn` is Eulerian and converted once |
| false | true | Configuration error |

Add cross-checks requiring:
- `l_stokes_forcing=.TRUE.`;
- an actual restart;
- the supported non-zstar mimetic AB configuration.

Print a clear startup message stating that the incoming restart velocity is assumed Eulerian. Do not infer the convention from `stokes_vn_old`, since a valid Stokes field can be zero.

### One-Time Activation
In the ocean driver, introduce:
```fortran
l_stokes_activation_pending
l_stokes_activation_step
```
Initialize `pending` from the two namelist switches and `isRestart()`. Reset `activation_step` to false at the beginning of each ocean timestep.

After the first ERA5 provider update:
1. Prepare the cell-centered, surface, and edge-normal Stokes fields.
2. Add Stokes velocity to wet edge levels:
   $$
   v_n^L=v_n^E+v_n^s.
   $$
3. Update `p_prog(nold)%vn`.
4. Copy the converted velocity to `p_prog(nnew)%vn`.
5. Set:
   ```fortran
   stokes_vn_old = stokes_vn
   ```
6. Set `l_stokes_activation_step=.TRUE.`.
7. Clear `l_stokes_activation_pending`.

Use a private driver-level mother routine for orchestration. Keep mapping and array-level operations in `mo_ocean_stokes_forcing` to avoid circular module dependencies.

### Dependent Diagnostics
Before the first wave-enabled momentum calculation:
1. Reconstruct cell-centered `p_diag%p_vn` from converted Lagrangian `vn`.
2. Initialize `p_diag%vn_time_weighted` from converted `vn`.
3. Synchronize the edge field.
4. Call `calc_vert_velocity` once to diagnose activation-time $w^L$:
   $$
   \partial_z w^L=-\nabla_h\cdot\mathbf v^L.
   $$

This is a one-time pre-step diagnosis. It ensures that the first Stokes shear tendency uses $w^L$, rather than the vertical velocity inherited from the Eulerian restart.
Do not add a separate pressure calculation. During normal tendency assembly, the existing pathway will:
1. Calculate $\mathbf v^L\cdot\partial_z\mathbf v^s$ using rebuilt `p_vn`.
2. Recalculate `press_hyd` and `press_grad`.
3. Apply the wavy-hydrostatic pressure gradient.
4. Use the rediagnosed $w^L$ in the Stokes shear tendency.
The standard post-momentum `calc_vert_velocity` call remains unchanged.
### Stokes Time Derivative
Pass the activation flag to the Stokes momentum calculation:
```fortran
l_stokes_derivative_startup = is_first_timestep                 &
  & .OR. l_stokes_activation_step
```
On the activation step:
$$
\partial_t v_n^s=0,
\qquad
v_{n,\mathrm{old}}^s=v_n^s.
$$

This avoids adding $(v_n^s-0)/\Delta t$ after the full Stokes velocity has already been inserted into `vn`.
Starting with the next step:
$$
\partial_t v_n^s
=
\frac{v_{n,\mathrm{now}}^s-v_{n,\mathrm{old}}^s}{\Delta t}.
$$

### Adams-Bashforth Bootstrap
Thread a separate activation flag through the free-surface wrapper, mimetic solver, explicit tendency assembly, and velocity predictor.
Define:
```fortran
l_ab_startup = is_first_timestep .OR. l_stokes_activation_step
```
Use:
```fortran
IF (l_ab_startup) THEN
  g_nimd = g_n
ELSE
  g_nimd = (1.5_wp + ab_const) * g_n                      &
         - (0.5_wp + ab_const) * g_nm1
END IF
```
The activation flag affects only:
- the Stokes time derivative;
- the AB tendency combination.
It must not activate the general ICON cold-start advection branch.
After the existing end-of-step tendency-pointer swap, the first wave-enabled `g_n` becomes valid AB history. Normal AB2 resumes automatically on the following timestep.

### Runscript Usage
No-Stokes spin-up:
```fortran
l_stokes_forcing = .FALSE.
l_stokes_from_eulerian_restart = .FALSE.
```
First continuation from its Eulerian restart:
```fortran
l_stokes_forcing = .TRUE.
l_stokes_from_eulerian_restart = .TRUE.
```
Every later wave-to-wave restart:
```fortran
l_stokes_forcing = .TRUE.
l_stokes_from_eulerian_restart = .FALSE.
```

The transition option must not remain enabled for an already Lagrangian restart because that would add Stokes velocity twice.
### Compilation and Runtime Verification
The user will perform all checks requiring a compiled model or simulation:
- [x] Compile with NAG.
- [x] Compile with Intel.
- [x] Run the no-wave-to-wave restart experiment.
- [ ] Confirm:$v_{n,\mathrm{after}}-v_{n,\mathrm{before}}-v_n^s=0$ to numerical precision on wet edges.
- [ ] Confirm on the activation step:
   - `stokes_time_tend = 0`;
   - `g_nimd = g_n`;
   - `p_vn` is Lagrangian;
   - the wavy pressure source is active;
   - the shear tendency uses rediagnosed $w^L$.
- [x] Confirm on the second wave-enabled step:
   - finite-difference `stokes_time_tend` is active;
   - normal AB2 has resumed;
   - AB history is wave-enabled.
- [x] Run a wave-to-wave restart with the transition option false and confirm:
   - no second Stokes insertion;
   - `stokes_vn_old` is restored;
   - results agree with an uninterrupted wave run.
- [x] Confirm that no-wave and ordinary cold-start configurations remain operational.

---

## [[2026-08-14]] Add Stokes forcings into timestepping
### Summary
Implement Stokes forcing behind `l_stokes_forcing`. When enabled, ICON’s prognostic `vn` means **Lagrangian edge-normal velocity**. The model converts Eulerian initial velocity to Lagrangian velocity only for fresh starts, and never adds Stokes velocity again on a normal restart.
The timestep keeps the split insertion:
- wavy hydrostatic source modifies `pressure_hyd` before `press_grad`;
- horizontal Stokes RHS is added to `g_n`;
- kinetic energy and scalar-product diagnostics are always based on Lagrangian `vn`.
### Key Changes
#### 1. Namelist And State Fields
Add to `ocean_physics_nml`:
```fortran
LOGICAL :: l_stokes_forcing = .FALSE.
```
Add Stokes diagnostics/state fields:
```fortran
stokes_vec_c
stokes_vec_sfc_c
stokes_vn
stokes_vn_old
stokes_shear_tend
stokes_vort_tend
stokes_time_tend
stokes_rhs
stokes_zeta_v
stokes_vn_dual
wavy_hydrostatic_source
```
`stokes_vn_old` must be restart-persistent, because it is needed for:
```fortran
d_t vn_s = (stokes_vn_now - stokes_vn_old) / dt
```
All other Stokes fields can be ordinary diagnostics.

#### 2. Stokes Module Wrappers
Add wrapper routines in `mo_ocean_stokes_forcing.f90`:
```fortran
prepare_stokes_fields(...)
initialize_lagrangian_velocity_from_stokes(...)
calculate_stokes_pressure_source(...)
calculate_stokes_momentum_rhs(...)
```
`prepare_stokes_fields` converts:
```fortran
p_as%ust_3d, p_as%vst_3d
p_as%surf_ust, p_as%surf_vst
```
into:
```fortran
stokes_vec_c
stokes_vec_sfc_c
stokes_vn
```
The 3D Stokes fields start at model level 1. The separate surface values are used only for the top vertical derivative.
`calculate_stokes_pressure_source` computes:
```fortran
wavy_hydrostatic_source = v_L . d_z v_s
```
Do not multiply by `rho0`; ICON integrates `pressure_hyd` as `p/rho0`.
`calculate_stokes_momentum_rhs` computes:
```fortran
stokes_rhs =
    stokes_shear_tend
  + stokes_vort_tend
  + stokes_time_tend
```
#### 3. Fresh Start Versus Restart
Use existing restart status from `isRestart()`.
For a fresh non-restart run with `l_stokes_forcing=.TRUE.`:
```fortran
call prepare_stokes_fields(...)

ocean_state%p_prog(nold(1))%vn =
  ocean_state%p_prog(nold(1))%vn + stokes_vn

ocean_state%p_prog(nnew(1))%vn =
  ocean_state%p_prog(nold(1))%vn

stokes_vn_old = stokes_vn
```
Then immediately recompute scalar-product diagnostics:
```fortran
call calc_scalar_product_veloc_3d(... ocean_state%p_prog(nold(1))%vn ...)
```
This makes `p_diag%p_vn`, `p_diag%kin`, and `p_diag%p_vn_dual` Lagrangian from the first momentum step.
For a normal restart with `l_stokes_forcing=.TRUE.`:
```fortran
! Do not add stokes_vn to vn.
! Restart vn is already assumed to be vn_L.
```
Still call `prepare_stokes_fields` after forcing is available so current `stokes_vn` exists, but use the restarted `stokes_vn_old` for the first post-restart `d_t vn_s`.
If `stokes_vn_old` is missing from an old restart file, fallback for that first step is:
```fortran
stokes_vn_old = stokes_vn
stokes_time_tend = 0.0_wp
```
and print a warning.

#### 4. Kinetic Energy Rule
Because horizontal advection uses `p_diag%kin` and `p_diag%grad`, the kinetic energy must correspond to Lagrangian velocity whenever waves are enabled.
Therefore:
- after fresh-start conversion, immediately rerun `calc_scalar_product_veloc_3d`;
- on restart, rerun `calc_scalar_product_veloc_3d` from restarted `vn_L` before the first momentum step;
- in normal timestepping, the existing pre-momentum scalar-product call remains correct because `p_prog(nold(1))%vn` is already Lagrangian.
#### 5. Wavy Hydrostatic Pressure
Extend `calc_internal_press_grad` with optional:
```fortran
wavy_hydrostatic_source_c
```
Wave-enabled pressure integration:
```fortran
pressure_hyd(1) =
  rho(1) * grav/rho0 * dz_top
  + wavy_source(1) * dz_top
  + bc_total_top_potential

pressure_hyd(jk) =
  pressure_hyd(jk-1)
  + 0.5 * (rho(jk) + rho(jk-1)) * grav/rho0 * dz
  + 0.5 * (wavy_source(jk) + wavy_source(jk-1)) * dz
```
Also include the same wave increment in the bottom partial-cell correction for `press_L` and `press_R`.
When `l_stokes_forcing=.FALSE.`, call the routine without the optional source and recover original ICON behavior.

#### 6. Momentum Time Step
In `calculate_explicit_term_ab`:
```fortran
call veloc_adv_horz_mimetic(...)

call calculate_density(...)

if (l_stokes_forcing) then
  call calculate_stokes_pressure_source(...)
else
  wavy_hydrostatic_source = 0.0_wp
end if

call calc_internal_press_grad(..., wavy_hydrostatic_source_c=wavy_hydrostatic_source, ...)

if (l_stokes_forcing) then
  call calculate_stokes_momentum_rhs(...)
else
  stokes_rhs = 0.0_wp
end if

call veloc_adv_vert_mimetic(...)
call velocity_diffusion(...)
```

Then in `calculate_explicit_term_g_n_onBlock`:

```fortran
ocean_state%p_aux%g_n = &
  - ocean_state%p_diag%press_grad       &
  - ocean_state%p_diag%grad             &
  - ocean_state%p_diag%veloc_adv_horz   &
  - ocean_state%p_diag%veloc_adv_vert   &
  + ocean_state%p_diag%laplacian_horz   &
  + ocean_state%p_diag%stokes_rhs
```

After the Stokes RHS is computed and no error occurred:

```fortran
stokes_vn_old = stokes_vn
```

### Test Plan
- `l_stokes_forcing=.FALSE.`: baseline behavior unchanged.
- Fresh start with synthetic Stokes: confirm `vn = vn_E + vn_s`.
- Wave restart: confirm `vn` is not incremented by `vn_s` again.
- Restart with persisted `stokes_vn_old`: first post-restart `stokes_time_tend` uses the restarted old field.
- Restart without `stokes_vn_old`: first post-restart `stokes_time_tend = 0` with warning.
- Zero Stokes: zero `stokes_rhs` and zero `wavy_hydrostatic_source`.
- Known one-column wavy source: expected `pressure_hyd` increment.
- Lagrangian kinetic-energy test: `p_diag%kin` changes after fresh-start conversion and matches `vn_L`.
### Assumptions
- V1 targets the non-zstar mimetic AB path.
- Fresh non-restart wave-enabled starts read Eulerian velocity.
- Normal wave-enabled restarts contain Lagrangian `vn`.
- Restart compatibility with old non-wave restart files is handled by the missing-`stokes_vn_old` fallback, but switching on waves from an old restart should be treated as a fresh conversion case only if explicitly configured later.
