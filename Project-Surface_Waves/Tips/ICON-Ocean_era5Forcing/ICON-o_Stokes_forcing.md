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



# Status: [[2026-08-14]]
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