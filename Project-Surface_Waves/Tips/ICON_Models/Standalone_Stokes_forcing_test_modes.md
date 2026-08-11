---
tags:
  - code
  - ICON-O
  - ICON/experiment
Last Eddited: 2026-08-11
---
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

# What Each Mode Tests

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

# Practical Run Notes
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
