---
tags:
  - code
  - ICON-O
  - ICON/experiment
Last Eddited: 2026-08-11
---
# ICON-o Stokes Forcing Sanity Check

## Purpose
The sanity check verifies that the prescribed ERA5 Stokes velocity is correctly received, reconstructed vertically, mapped onto ICON edges, inserted into the Lagrangian velocity, and used to calculate the additional momentum and wavy-hydrostatic terms.

The experiment uses hourly output during the first simulation day. The initial model velocity is zero, allowing the first response to the Stokes velocity to be identified clearly.

## Checks Performed
### 1. ERA5 Stokes Input
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

### 2. Activation of the New Terms
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

### 3. Why the initial state is all zeros even I initialise the model with Stokes?
Because ICON writes the 1985-01-01 00:00 record before our Stokes initialization is executed.
The actual ordering is:
1. ICON writes the untouched initial state with jstep=0 in [mo_hydro_ocean_run.f90 (line 1748)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:1748). At this point, the configured initial Eulerian velocity is zero.
2. ICON enters ocean_time_step, increments the step, and advances current_time in [mo_hydro_ocean_run.f90 (line 464)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:464).
3. The ERA5 provider fields are updated in [mo_hydro_ocean_run.f90 (line 521)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:521).
4. Only then does our code execute $v_n^L=v_n^E+v_n^s$ in [mo_hydro_ocean_run.f90 (line 524)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:524).
5. The next normal output is written after completing the first model step, at 01:00, in [mo_hydro_ocean_run.f90 (line 808)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/drivers/mo_hydro_ocean_run.f90:808).
## Why the Terms Start at Different Times
### Terms Starting at `01:00`
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

### Time Tendency Starting at `02:00`
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

### Shear Tendency Starting at `02:00`
The vertical-shear term is

$$
w^L\partial_z\mathbf v^s.
$$

During the first momentum calculation, the diagnostic vertical velocity supplied to this term is still the initially zero $w^L$. The first nonzero $w^L$ is diagnosed afterward from the continuity equation using the updated Lagrangian horizontal transport.

That diagnosed $w^L$ becomes available to the momentum calculation during the following step. Therefore, `stokes_shear_tend` first appears at `02:00`.

## Momentum RHS Closure
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

## Tendency Magnitudes
The horizontal Stokes tendencies generally lie between approximately $10^{-8}$ and $10^{-5}\ \mathrm{m\,s^{-2}}$:
- Stokes vorticity tendency: approximately $10^{-8}$–$10^{-7}\ \mathrm{m\,s^{-2}}$
- Vertical-shear tendency: approximately $10^{-7}$–$10^{-6}\ \mathrm{m\,s^{-2}}$
- Stokes time tendency: several $10^{-6}\ \mathrm{m\,s^{-2}}$
- Total Stokes RHS: several $10^{-6}$, approaching $10^{-5}\ \mathrm{m\,s^{-2}}$

The Stokes time tendency and vertical-shear tendency are the largest direct horizontal momentum contributions during this period.

The wavy-hydrostatic source has a larger magnitude, around $10^{-3}\ \mathrm{m\,s^{-2}}$, but it is not a direct horizontal momentum tendency. It is vertically integrated into pressure before its horizontal pressure gradient affects momentum.

## Lagrangian Velocity Initialization
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

## Conclusion
**The sanity checks show that:**
- ERA5 Stokes fields are received and reconstructed successfully.
- Cell-centered Stokes velocity is mapped successfully to edge-normal velocity.
- The prognostic velocity receives the expected Lagrangian Stokes contribution.
- The vorticity, vertical-shear, time-derivative, and wavy-hydrostatic terms activate according to their required model state.
- The total Stokes momentum RHS is assembled accurately.
- The with-Stokes minus control velocity strongly agrees with the prescribed Stokes velocity during the first evolved timestep.

These tests provide strong numerical evidence that the Stokes forcing pathway is active and wired correctly. They validate the implementation mechanics, while longer physical validation is still needed for signs, conservation behavior, pressure response, and long-term ocean adjustment.

# Standalone Stokes Forcing Test Modes 120-125

This test suite lives in:
- `src/ocean/testbed/mo_ocean_testbed_stokes_forcing.f90`
- dispatched through `src/ocean/testbed/mo_ocean_testbed_operators.f90`

The purpose is to test each new Stokes-forcing operator in isolation before wiring the terms into the real ICON-o time stepping.

Important convention: the routines return the **raw mathematical product**. They do not apply the final sign from the momentum equation. For example, `stokes_vertical_shear_tendency` returns `+ w^L * d_z(v_s)`, while the later momentum equation caller will subtract it.

---

## Common Setup
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

---

## Mode 120: Surface Vertical Shear + Wavy Hydrostatic Source
### Purpose
Tests the surface-boundary part of:
```text
w^L * d_z(v_s)
```
and also tests:
```text
v^L · d_z(v_s)
```
for the wavy-hydrostatic source.
### Synthetic Design
```text
surface u_s = 1
level-1 u_s = 0
level-2 u_s = 0
v_s = 0
w_L = 2
v_L = (1, 0, 0)
```
So only the surface-to-level-1 derivative is nonzero.
### Raw Expected Vertical Shear
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
### Expected Check Results
```text
surface raw wdz_u = 4
surface interior raw wdz_u = 0
```
### Wavy-Hydrostatic Expected Result
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

### Notes
The final `shear_tend` printed in the log is after:
1. interface-to-layer-center mapping;
2. cell-vector to edge-normal projection.

So it is not expected to be `4`. In our logs it was around:
```text
shear_tend sample ≈ 0.8669
```
That is diagnostic, not the hard-coded unit-test value.

---

## Mode 121: Interior Vertical Shear + Wavy Hydrostatic Source
### Purpose
Tests the interior vertical derivative path of:
```text
w^L * d_z(v_s)
```
and again tests:
```text
v^L · d_z(v_s)
```
for the wavy-hydrostatic source.
### Synthetic Design
```text
surface u_s = 1
level-1 u_s = 1
level-2 u_s = 0
v_s = 0
w_L = 3
v_L = (1, 0, 0)
```
So the surface derivative is zero, and only the level-1 to level-2 derivative is nonzero.
### Raw Expected Vertical Shear
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
### Expected Check Results
```text
interior surface raw wdz_u = 0
interior raw wdz_u = 3
```
### Wavy-Hydrostatic Expected Result
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
### Notes
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

---
## Mode 122: Exact Stokes Time Tendency
### Purpose
Tests:
```text
d(v_s)/dt = (v_s(now) - v_s(old)) / dt
```
### Synthetic Design
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
### Expected Check Result
Every entry of `time_tend` should be exactly:
```text
1
```
The check is:
```text
max(abs(time_tend - 1)) = 0
```

---

## Mode 123: First-Step Stokes Time Tendency
### Purpose
Tests the special first-step behavior of:
```text
stokes_time_tendency
```
At the first timestep, the caller may not have a valid old Stokes velocity yet, so the routine should return zero.
### Synthetic Design
```text
is_first_timestep = true
```
Even though `stokes_vn_old` is set later in the testbed, the routine ignores it because this is the first step.

### Expected Check Result
```text
time_tend = 0 everywhere
```
The check is:
```text
max(abs(time_tend)) = 0
```

---

## Mode 124: Zero Stokes Vorticity Tendency
### Purpose
Tests the zero-input behavior of:
```text
ζ_s * zhat × v^L
```
where `ζ_s` is derived from `stokes_vn`.
### Synthetic Design
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
### Expected Check Results
```text
max(abs(stokes_zeta)) = 0
max(abs(vort_tend)) = 0
```
This is a clean exact zero test for the Stokes-vortex operator.

---

## Mode 125: Stokes Vorticity Linearity Check
### Purpose
Tests the linearity of:
```text
ζ_s = curl(v_s)
```
and the resulting vortex tendency:
```text
ζ_s * zhat × v^L
```
This avoids trying to hand-derive an exact curl value on the ICON grid.
### Synthetic Design
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
### Expected Check Results
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

---

## What Each Mode Tests

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

---

## Practical Run Notes
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


## Result interpretation
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


# Stokes Forcing Integration Plan 1

## Summary

Implement the Lagrangian Stokes framework behind a new ocean namelist switch. When enabled, the prognostic edge-normal velocity `vn` is treated as Lagrangian velocity after a one-time startup conversion:

```fortran
vn_L(t0) = vn_E(t0) + vn_s(t0)
```

The implementation is split into two physics insertion points:

- **Wavy hydrostatic source** modifies `pressure_hyd` before `press_grad` is calculated.
- **Horizontal Stokes compensation terms** are added to `ocean_state%p_aux%g_n` before AB extrapolation.

The existing standalone operators in `mo_ocean_stokes_forcing.f90` remain the low-level tested building blocks.

## Key Changes

### 1. Stokes Module Wrappers

Extend `mo_ocean_stokes_forcing.f90` with two public wrapper routines.

Add:

```fortran
calculate_stokes_pressure_source(...)
calculate_stokes_momentum_rhs(...)
```

`calculate_stokes_pressure_source` will:

- convert `p_as%ust_3d/vst_3d` to cell-centered Cartesian `stokes_vec_c`;
- convert `p_as%surf_ust/surf_vst` to surface Cartesian `stokes_vec_sfc_c`;
- compute:

```fortran
wavy_hydrostatic_source_c = v_L . d_z v_s
```

Keep `wavy_hydrostatic_source` as the raw acceleration source, with units `m s-2`. Do **not** multiply by `rho0` there, because ICON stores `pressure_hyd` effectively as `p/rho0`.

`calculate_stokes_momentum_rhs` will:

- reuse the same Stokes diagnostics;
- map Stokes to edge-normal `stokes_vn`;
- compute:

```fortran
shear_tend_e = w_L * d_z v_s
vort_tend_e  = zeta_s * z x v_L
time_tend_e  = d_t vn_s
```

- sum:

```fortran
stokes_rhs_e = shear_tend_e + vort_tend_e + time_tend_e
```

The wrappers should call the existing low-level routines rather than duplicate logic.

### 2. Namelist And State Fields

Add a new switch in `ocean_physics_nml`:

```fortran
LOGICAL :: l_stokes_forcing = .FALSE.
```

Do not reuse `l_couple_icon_waves`; that switch is for wave coupling, while this feature should also work with ERA5-reconstructed Stokes fields.

Add persistent diagnostic fields to `t_hydro_ocean_diag`:

```fortran
stokes_vec_c                  ! cell center, mid-level, Cartesian vector
stokes_vec_sfc_c              ! cell center, surface, Cartesian vector
stokes_vn                     ! edge normal, mid-level
stokes_vn_old                 ! edge normal, mid-level, previous forcing time
stokes_shear_tend             ! edge normal, mid-level
stokes_vort_tend              ! edge normal, mid-level
stokes_time_tend              ! edge normal, mid-level
stokes_rhs                    ! edge normal, mid-level
stokes_zeta_v                 ! vertex, mid-level
stokes_vn_dual                ! vertex, mid-level, Cartesian vector
wavy_hydrostatic_source       ! cell center, mid-level
```

Use `lrestart_cont=.TRUE.` for `stokes_vn_old`. The other Stokes diagnostics can be non-restart diagnostics.

### 3. Initialization

After `p_as` has been populated by `update_ocean_surface_refactor` for the first model time, perform a one-time wave initialization if `l_stokes_forcing=.TRUE.`.

Steps:

```fortran
call stokes_local_to_cartesian_cells(...)
call stokes_surface_local_to_cartesian_cells(...)
call stokes_cell_to_edge_normal(...)

ocean_state%p_prog(nold(1))%vn = ocean_state%p_prog(nold(1))%vn + stokes_vn
ocean_state%p_prog(nnew(1))%vn = ocean_state%p_prog(nold(1))%vn

stokes_vn_old = stokes_vn
```

Then immediately recompute velocity diagnostics from the converted Lagrangian velocity:

```fortran
call calc_scalar_product_veloc_3d(... ocean_state%p_prog(nold(1))%vn ...)
```

This is necessary because `p_diag%p_vn` must represent `v_L`, not the original Eulerian velocity.

For v1, assume fresh wave-enabled starts read Eulerian initial velocity. Restarts from a previous wave-enabled Lagrangian run will need a later restart policy to avoid adding `vn_s` twice.

### 4. Wavy Hydrostatic Pressure

Modify `calc_internal_press_grad` to accept an optional argument:

```fortran
REAL(wp), INTENT(in), OPTIONAL :: wavy_hydrostatic_source_c(nproma,n_zlev,alloc_cell_blocks)
```

Original first-level pressure-potential integration:

```fortran
pressure_hyd(jc,1,jb) =
  rho(jc,1,jb) * grav/rho0 * dz_top
  + bc_total_top_potential(jc,jb)
```

Wave-enabled version:

```fortran
pressure_hyd(jc,1,jb) =
  rho(jc,1,jb) * grav/rho0 * dz_top
  + wavy_hydrostatic_source_c(jc,1,jb) * dz_top
  + bc_total_top_potential(jc,jb)
```

Original deeper-level integration:

```fortran
pressure_hyd(jk) =
  pressure_hyd(jk-1)
  + 0.5 * (rho(jk) + rho(jk-1)) * grav/rho0 * dz
```

Wave-enabled version:

```fortran
pressure_hyd(jk) =
  pressure_hyd(jk-1)
  + 0.5 * (rho(jk) + rho(jk-1)) * grav/rho0 * dz
  + 0.5 * (wavy_source(jk) + wavy_source(jk-1)) * dz
```

Also update the bottom partial-cell correction so `press_L` and `press_R` include the same wavy-source trapezoidal increment.

When `l_stokes_forcing=.FALSE.`, call `calc_internal_press_grad` without the optional source and recover the original behavior.

### 5. Momentum Time Step

In `calculate_explicit_term_ab`, after density is available and before `calc_internal_press_grad`:

```fortran
if (l_stokes_forcing) then
  call calculate_stokes_pressure_source(...)
else
  wavy_hydrostatic_source = 0.0_wp
end if
```

Then call:

```fortran
call calc_internal_press_grad(..., wavy_hydrostatic_source_c = ocean_state%p_diag%wavy_hydrostatic_source, ...)
```

After `press_grad`, `veloc_adv_vert_mimetic`, and diffusion are available, compute the horizontal Stokes RHS:

```fortran
if (l_stokes_forcing) then
  call calculate_stokes_momentum_rhs(...)
else
  stokes_rhs = 0.0_wp
end if
```

Modify `calculate_explicit_term_g_n_onBlock` to add `stokes_rhs`:

```fortran
ocean_state%p_aux%g_n = &
  - ocean_state%p_diag%press_grad       &
  - ocean_state%p_diag%grad             &
  - ocean_state%p_diag%veloc_adv_horz   &
  - ocean_state%p_diag%veloc_adv_vert   &
  + ocean_state%p_diag%laplacian_horz   &
  + ocean_state%p_diag%stokes_rhs
```

The plus sign is intentional: the Stokes compensation terms appear with negative signs on the LHS of the Lagrangian momentum equation, so they enter the RHS tendency positively.

After the Stokes RHS has been computed for the timestep, update:

```fortran
stokes_vn_old = stokes_vn
```

so the next call to `stokes_time_tendency` has the correct previous forcing field.

## Test Plan

Keep standalone test modes `120-125` and add integration-level checks:

- `l_stokes_forcing=.FALSE.`: compile and run unchanged baseline.
- Zero ERA5 Stokes fields: all Stokes diagnostics and `stokes_rhs` are zero; model matches baseline.
- Constant vertical Stokes profile: `stokes_shear_tend` is zero.
- Horizontally uniform Stokes: `stokes_zeta_v` and `stokes_vort_tend` are zero.
- Prescribed old/new Stokes fields: `stokes_time_tend = (vn_s_now - vn_s_old)/dt`.
- One-column pressure test: known `wavy_hydrostatic_source_c` produces the expected additional `pressure_hyd` increment.
- Fresh-start Lagrangian conversion test: after initialization, `vn = vn_E + vn_s` at selected edges.

## Assumptions

- V1 targets the non-zstar mimetic AB path in `mo_ocean_ab_timestepping_mimetic.f90`.
- `p_as%ust_3d/vst_3d` are mid-level Stokes velocities starting at model level 1.
- `p_as%surf_ust/surf_vst` are separate surface values and are used only for the top vertical derivative.
- The initial velocity supplied to a fresh wave-enabled run is Eulerian and should be converted once to Lagrangian.
- `p_diag%w` used in the Stokes shear term is already the diagnosed vertical velocity from the Lagrangian velocity state available at that timestep.



# Stokes Forcing Integration Plan With Restart Handling

## Summary

Implement Stokes forcing behind `l_stokes_forcing`. When enabled, ICON’s prognostic `vn` means **Lagrangian edge-normal velocity**. The model converts Eulerian initial velocity to Lagrangian velocity only for fresh starts, and never adds Stokes velocity again on a normal restart.

The timestep keeps the split insertion:

- wavy hydrostatic source modifies `pressure_hyd` before `press_grad`;
- horizontal Stokes RHS is added to `g_n`;
- kinetic energy and scalar-product diagnostics are always based on Lagrangian `vn`.

## Key Changes

### 1. Namelist And State Fields

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

### 2. Stokes Module Wrappers

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

### 3. Fresh Start Versus Restart

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

### 4. Kinetic Energy Rule

Because horizontal advection uses `p_diag%kin` and `p_diag%grad`, the kinetic energy must correspond to Lagrangian velocity whenever waves are enabled.

Therefore:

- after fresh-start conversion, immediately rerun `calc_scalar_product_veloc_3d`;
- on restart, rerun `calc_scalar_product_veloc_3d` from restarted `vn_L` before the first momentum step;
- in normal timestepping, the existing pre-momentum scalar-product call remains correct because `p_prog(nold(1))%vn` is already Lagrangian.

### 5. Wavy Hydrostatic Pressure

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

### 6. Momentum Time Step

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

## Test Plan

- `l_stokes_forcing=.FALSE.`: baseline behavior unchanged.
- Fresh start with synthetic Stokes: confirm `vn = vn_E + vn_s`.
- Wave restart: confirm `vn` is not incremented by `vn_s` again.
- Restart with persisted `stokes_vn_old`: first post-restart `stokes_time_tend` uses the restarted old field.
- Restart without `stokes_vn_old`: first post-restart `stokes_time_tend = 0` with warning.
- Zero Stokes: zero `stokes_rhs` and zero `wavy_hydrostatic_source`.
- Known one-column wavy source: expected `pressure_hyd` increment.
- Lagrangian kinetic-energy test: `p_diag%kin` changes after fresh-start conversion and matches `vn_L`.

## Assumptions

- V1 targets the non-zstar mimetic AB path.
- Fresh non-restart wave-enabled starts read Eulerian velocity.
- Normal wave-enabled restarts contain Lagrangian `vn`.
- Restart compatibility with old non-wave restart files is handled by the missing-`stokes_vn_old` fallback, but switching on waves from an old restart should be treated as a fresh conversion case only if explicitly configured later.



# Status: 

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

Suppose the activation occurs at timestep \(n\): $$g_{n-1}=g_{n-1}^{E}, \qquad g_n=g_n^{L,\mathrm{wave}}.$$
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