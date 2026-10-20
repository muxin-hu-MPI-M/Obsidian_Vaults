---
tags:
  - project/surfwaves
  - ICON
  - ICON-O
Last Eddited: 2026-09-07
---
# ICON-O Stokes/Lagrangian Momentum Implementation
## 1. Purpose
The implementation changes the meaning of ICON-O's prognostic horizontal
velocity when wave forcing is enabled:

$$
\mathbf v^L=\mathbf v^E+\mathbf v^s.
$$

- With `l_stokes_forcing=.FALSE.`, `vn` remains the original Eulerian velocity.
- With `l_stokes_forcing=.TRUE.`, `vn` represents Lagrangian velocity throughout
  the model.
- Existing advection, kinetic energy, continuity, vertical velocity, and tracer transport then naturally use Lagrangian velocity.
- Additional Stokes compensation terms and the wavy-hydrostatic pressure correction are added explicitly.
The implemented horizontal Stokes RHS is
$$
G_{\mathrm{Stokes}}
=
w^L\partial_z\mathbf v^s
+\zeta^s\hat{\mathbf z}\times\mathbf v^L
+\partial_t\mathbf v^s.
$$
These terms are positive in `g_n` because they were negative on the left-hand side of the continuous momentum equation.

## 2. Main Files
### ERA5 input and profile reconstruction
- `etc/era5g_wave_provider.py`
- `src/coupling/mo_ocean_era5_provider_coupling.f90`
- `src/ocean/boundary/mo_ocean_surface_types.f90`
- `src/ocean/boundary/mo_ocean_forcing.f90`
- `src/ocean/boundary/mo_ocean_surface_refactor.f90`
### Configuration and model state
- `src/ocean/config/mo_ocean_nml.f90`
- `src/ocean/config/mo_ocean_nml_crosscheck.f90`
- `src/ocean/dynamics/mo_ocean_types.f90`
- `src/ocean/dynamics/mo_ocean_state.f90`
### Momentum and pressure implementation
- `src/ocean/dynamics/mo_ocean_stokes_forcing.f90`
- `src/ocean/physics/mo_ocean_thermodyn.f90`
- `src/ocean/dynamics/mo_ocean_ab_timestepping.f90`
- `src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90`
- `src/ocean/drivers/mo_hydro_ocean_run.f90`
### Operator tests
- `src/ocean/testbed/mo_ocean_testbed_operators.f90`
- `src/ocean/testbed/mo_ocean_testbed_stokes_forcing.f90`

## 3. Overall Data Flow
```text
ERA5 wave data
    |
    v
surface Stokes velocity + Stokes transport
    |
    v
reconstruct 3-D Stokes profile at ICON cell mid-levels
    |
    v
convert local (u_s,v_s) to Cartesian cell vectors
    |
    v
map cell vectors to edge-normal stokes_vn
    |
    +--> initialize or activate vn_L = vn_E + stokes_vn
    |
    +--> calculate vertical-shear tendency
    +--> calculate Stokes-vorticity tendency
    +--> calculate Stokes time tendency
    +--> calculate wavy-hydrostatic source
    |
    v
assemble Lagrangian momentum tendency g_n
    |
    v
Adams-Bashforth velocity predictor and free-surface solve
    |
    v
new Lagrangian vn, diagnosed w_L, and Lagrangian tracer transport
```

## 4. Configuration
Two switches are available in `ocean_physics_nml`:
```fortran
l_stokes_forcing               = .FALSE.
l_stokes_from_eulerian_restart = .FALSE.
```

| Configuration | Meaning |
|---|---|
| Both false | Original ICON-O |
| Stokes true, transition false, cold start | Initialize Lagrangian velocity from the first Stokes field |
| Stokes true, transition false, restart | Restart velocity is already Lagrangian |
| Both true on restart | Convert an Eulerian no-wave restart to Lagrangian velocity once |
| Stokes false, transition true | Invalid configuration |

The transition option is restricted to the z-level, mimetic, non-shallow-water configuration.
## 5. Stokes State Fields

| Field                     | Horizontal location    | Vertical location                    |
| ------------------------- | ---------------------- | ------------------------------------ |
| `stokes_vec_sfc_c`        | Cell center            | Actual surface boundary              |
| `stokes_vec_c`            | Cell center            | Prism centers, `jk=1:n_zlev`         |
| `stokes_vn`               | Edge, normal component | Prism centers                        |
| `stokes_vn_old`           | Edge, normal component | Prism centers                        |
| `stokes_zeta_v`           | Vertex                 | Prism-center levels                  |
| `stokes_vn_dual`          | Vertex                 | Prism-center levels                  |
| `w`                       | Cell center            | Vertical interfaces, `jk=1:n_zlev+1` |
| `wavy_hydrostatic_source` | Cell center            | Prism centers                        |
| `stokes_shear_tend`       | Edge, normal component | Prism centers                        |
| `stokes_vort_tend`        | Edge, normal component | Prism centers                        |
| `stokes_time_tend`        | Edge, normal component | Prism centers                        |
| `stokes_rhs`              | Edge, normal component | Prism centers                        |
|                           |                        |                                      |

Only `stokes_vn_old` must persist through a normal wave restart, so it is registered in `ocean_restart_list`.

You are right that we must distinguish **horizontal location** from **vertical location**. My earlier labels described only the horizontal ICON C-grid staggering.

Therefore:
- `wavy_hydrostatic_source` is horizontally cell-centered and vertically at prism centers.
- `stokes_rhs` is horizontally edge-centered and vertically at prism centers.
- Neither final field is stored at a vertical interface.
## 6. ERA5 Stokes Preparation

`update_ocean_surface_refactor` receives:
```fortran
p_as%surf_ust
p_as%surf_vst
p_as%stokes_transport
```

The Phillips-based reconstruction creates:
```fortran
p_as%ust_3d
p_as%vst_3d
```

The 3-D values begin at the first ICON cell mid-level, not at the surface.
Therefore, the separate surface values are retained for the upper boundary
derivative:

$$
\left.\partial_z\mathbf v^s\right|_{\mathrm{top}}
\approx
\frac{\mathbf v^s_{\mathrm{surface}}-\mathbf v^s_1}
{\Delta z_{\mathrm{surface},1}}.
$$

`prepare_stokes_fields` then:
1. Converts local zonal/meridional components to Cartesian cell vectors.
2. Converts the surface components separately.
3. Maps the 3-D cell vectors to edge-normal `stokes_vn`.
4. Synchronizes the edge field.

## 7. Fresh-Start Initialization

After the first ERA5 provider update:
```fortran
CALL prepare_stokes_fields(...)
CALL initialize_lagrangian_velocity_from_stokes(...)
```

The initializer performs
$$
v_n^L=v_n^E+v_n^s
$$

on wet edge levels and sets
```fortran
stokes_vn_old = stokes_vn
p_prog(nnew)%vn = p_prog(nold)%vn
```

It immediately calls `calc_scalar_product_veloc_3d` again. Consequently:
- `p_diag%p_vn` is Lagrangian.
- Reconstructed `u` and `v` are Lagrangian.
- `p_diag%kin` is Lagrangian kinetic energy.
- The first momentum calculation does not use stale Eulerian kinetic energy.

The written `00:00` cold-start record remains zero because ICON writes it before the first provider update. The first actual prognostic step nevertheless uses the initialized Lagrangian velocity.

## 8. Eulerian-Restart Activation
For a no-Stokes restart followed by wave activation:

```fortran
l_stokes_forcing               = .TRUE.
l_stokes_from_eulerian_restart = .TRUE.
```

A one-shot activation routine:

1. Prepares the current Stokes fields.
2. Adds `stokes_vn` to the restarted Eulerian `vn`.
3. Copies the converted velocity to both prognostic time levels.
4. Sets `stokes_vn_old=stokes_vn`.
5. Initializes `vn_time_weighted` from the converted velocity.
6. Synchronizes the edge velocity.
7. Reconstructs Lagrangian cell velocity and kinetic energy.
8. Calls `calc_vert_velocity` to diagnose activation-time $w^L$.
9. Marks this timestep as the Stokes activation step.

The activation flag is cleared immediately, so Stokes velocity cannot be
inserted again on later steps.

For a wave-to-wave restart, the transition switch must remain false because
the restart velocity is already Lagrangian.

## 9. Momentum Timestep

The main sequence is:

```text
ocean_time_step
  -> reconstruct velocity diagnostics and kinetic energy
  -> update_ocean_surface_refactor
       -> receive ERA5 forcing and reconstruct Stokes profile
  -> initialize/activate Lagrangian velocity when required
  -> calculate_density
  -> create_pressure_bc_conditions
  -> update_ho_params
  -> solve_free_surface_eq_ab
       -> solve_free_sfc_ab_mimetic
            -> top_bound_cond_horz_veloc
            -> calculate_explicit_term_ab
                 -> horizontal advection using vn_L
                 -> prepare current Stokes fields
                 -> calculate wavy-hydrostatic source
                 -> calculate corrected pressure_hyd and press_grad
                 -> calculate all horizontal Stokes tendencies
                 -> calculate ordinary vertical advection
                 -> calculate horizontal diffusion
                 -> assemble g_n
                 -> apply AB startup or normal AB2
                 -> calculate vn predictor
            -> construct and solve free-surface equation
  -> calc_normal_velocity_ab
  -> calc_vert_velocity
  -> tracer_transport
  -> update velocity and AB time levels
```

## 10. Individual Stokes Terms

### Vertical-shear term

$$
w^L\partial_z\mathbf v^s
$$

`stokes_vertical_shear_tendency`:
1. Calculates the Stokes derivative at vertical interfaces.
2. Uses the separate surface Stokes value at the top.
3. Multiplies by interface $w^L$.
4. Sets the bottom-interface contribution to zero.
5. Maps interface vectors to cell mid-levels.
6. Maps cell vectors to edge-normal tendencies.

### Stokes-vorticity term
$$
\zeta^s\hat{\mathbf z}\times\mathbf v^L
$$

`stokes_vorticity_tendency`:
1. Maps `stokes_vn` to the dual vertex representation.
2. Calls `rot_vertex_ocean_3d` to calculate $\zeta^s$.
3. Applies the nonlinear-Coriolis-style vertex-to-edge stencil.
4. Uses Lagrangian `vn`.
5. Deliberately excludes planetary vorticity $f$.

### Stokes time derivative
$$
\partial_t v_n^s
\approx
\frac{v_{n,\mathrm{now}}^s-v_{n,\mathrm{old}}^s}{\Delta t}.
$$

It is set to zero during:
- The first cold-start timestep.
- The Eulerian-to-Lagrangian restart activation step.

Afterward, the finite difference is calculated normally and `stokes_vn_old` is updated.

### Total horizontal Stokes RHS
The momentum equation is solved for edge-normal velocity `vn`. Therefore, its tendency must also be edge-normal:

$$
G^s_e(k)
=
\left[
w^L\partial_z\mathbf v^s
+\zeta^s\hat{\mathbf z}\times\mathbf v^L
+\partial_t\mathbf v^s
\right]_e.
$$

For the shear term:
1. $w^L\partial_z\mathbf v^s$ is calculated at vertical interfaces.
2. It is mapped vertically to prism centers.
3. The cell-centered vector is mapped horizontally to edge-normal form.
4. 
So the final `stokes_rhs(je,jk,jb)` occupies exactly the same discrete location as:
```fortran
p_prog%vn(je,jk,jb)
p_aux%g_n(je,jk,jb)
```

The essential distinction is therefore:
```text
wavy_hydrostatic_source : horizontal cell center + vertical prism center
stokes_rhs               : horizontal edge normal + vertical prism center
```

The temporary vertical derivatives are the quantities located at interfaces.
```fortran
stokes_rhs = stokes_shear_tend  &
           + stokes_vort_tend   &
           + stokes_time_tend
```

It is added directly to the momentum tendency:
```fortran
g_n = -press_grad
    - grad
    - veloc_adv_horz
    - veloc_adv_vert
    + laplacian_horz
    + stokes_rhs
```

## 11. Wavy-Hydrostatic Pressure
The vertical Stokes derivative is first calculated temporarily at interfaces:

$$
\left.\partial_z\mathbf v^s\right|_{i=1}
=
\frac{\mathbf v^s_{\mathrm{surface}}-\mathbf v^s_1}
{\Delta z_{\mathrm{surface},1}}.
$$

Interior derivatives are calculated between adjacent prism centers. These interface derivatives are then mapped to prism centers:

```fortran
CALL map_vec_prismtop2center_on_block(...)
```

Only afterward is the hydrostatic source formed:

$$
S_c(k)
=
\mathbf v_c^L(k)\cdot
\left(\partial_z\mathbf v^s\right)_c(k).
$$

Thus `wavy_hydrostatic_source(:,:,1)` is located at the center of the **first/top prism**, not directly at the sea surface. The surface derivative contributes to this first-prism-center value.

`calc_internal_press_grad` receives this source as an optional argument and vertically integrates it together with the ordinary hydrostatic contribution.

Because ICON stores the pressure potential effectively as $p/\rho_0$, the implementation adds

$$
S_{\mathrm{wave}}\Delta z
$$

without multiplying by $\rho_0$. The partial-cell pressure-gradient correction includes the same source. 
In the end, ICON vertically integrates the kinematic pressure:
$$\Pi=\frac{p}{\rho_0}$$
$$ \Pi_k=\Pi_{k-1} + \left[ \frac{g}{\rho_0}\frac{\rho_k+\rho_{k-1}}{2} + \frac{S_k+S_{k-1}}{2} \right]\Delta z_k , $$

where
$$ S=\mathbf v^L\cdot\partial_z\mathbf v^s . $$

The ordinary density contribution is implemented at [mo_ocean_thermodyn.f90 (line 238)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/physics/mo_ocean_thermodyn.f90:238), and the wavy term is added in the first and subsequent layers at [mo_ocean_thermodyn.f90 (line 261)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/physics/mo_ocean_thermodyn.f90:261). 
==**ICON then differences the cumulative column pressures between neighbouring cells to obtain the edge-normal pressure acceleration**== in `press_grad` in the horizontal momentum tendency term at line 342.

### Magnitude of the Wavy-hydrostatic Pressure
The value
$$ \max\left|\mathbf v^L\cdot\partial_z\mathbf v^s\right| =7.86\times10^{-3}\ {\rm m\,s^{-2}} $$
is locally significant, but it must not be compared directly with `stokes_rhs`. They enter the model differently.
The distribution of the wavy-hydrostatic pressure source in the first simulation day is:
![[Screenshot 2026-10-20 at 10.19.08.png|546]]
- The magnitude is mostly within the 1e-2
#### Comparison With Gravity
The ordinary hydrostatic source is approximately
$$ \frac{\rho}{\rho_0}g\approx9.81\ {\rm m\,s^{-2}}. $$
Therefore,
$$ \frac{7.86\times10^{-3}}{9.81} \approx8.0\times10^{-4}, $$
or about $0.08\%$ of the full gravitational term. It is small compared with gravity itself.

However, the dynamically relevant hydrostatic pressure gradient depends on horizontal density differences, not the common reference-density contribution. An equivalent density anomaly is
$$ \rho'_{\mathrm{equiv}} = \frac{\rho_0}{g} \mathbf v^L\cdot\partial_z\mathbf v^s. $$
Using ICON’s $\rho_0=1025.022\ \mathrm{kg\,m^{-3}}$ and $g=9.80665\ \mathrm{m\,s^{-2}}$,

$$ \rho'_{\mathrm{equiv}} \approx \frac{1025}{9.81}(7.86\times10^{-3}) \approx0.82\ {\rm kg\,m^{-3}}. $$
Thus:

| Wavy source                          | Equivalent density anomaly |
| ------------------------------------ | -------------------------- |
| $10^{-4}\ {\rm m\,s^{-2}}$           | $0.010\ {\rm kg\,m^{-3}}$  |
| $10^{-3}\ {\rm m\,s^{-2}}$           | $0.105\ {\rm kg\,m^{-3}}$  |
| $5\times10^{-3}\ {\rm m\,s^{-2}}$    | $0.523\ {\rm kg\,m^{-3}}$  |
| $7.86\times10^{-3}\ {\rm m\,s^{-2}}$ | $0.82\ {\rm kg\,m^{-3}}$   |
| $10^{-2}\ {\rm m\,s^{-2}}$           | $1.04\ {\rm kg\,m^{-3}}$   |
It does not mean that ICON modifies temperature, salinity or `rho`; it does not represent a real water mass with a $1\ \mathrm{kg\,m^{-3}}$ density anomaly.

### Does it reach the deep ocean
~={red}**Yes**=~: in this ICON-O implementation, a surface-confined wavy-hydrostatic source can modify the pressure gradient throughout the entire water column.

Define the cumulative wave-pressure potential
$$ \Phi_w(z)=\int_{\rm surface}^{z} S(z')\,d\ell . $$
If \(S\) becomes negligible below the wave-affected layer \(z_s\), then
$$ \Phi_w(z>z_s)=\Phi_w(z_s). $$
It is constant with depth below \(z_s\), but if it varies horizontally,
$$ -\nabla_h\Phi_w $$
remains a pressure acceleration at every deeper level. The code contains no removal of this depth-uniform component, taper, or deep cutoff.

Therefore, the implementation can:
- launch a barotropic/free-surface adjustment;
- change depth-integrated transport;
- initially accelerate deep velocities;
- potentially modify deeper balanced circulation.

But ICON’s free-surface solver does respond. The pressure-modified velocity predictor is vertically integrated into the surface-height equation at [mo_ocean_ab_timestepping_mimetic.f90 (line 1034)](/work/mh0033/m301254/proj_surfwave/icon-2026-06-ocean-era5/m301254/era5g-wave-forcing/src/ocean/dynamics/mo_ocean_ab_timestepping_mimetic.f90:1034), and the resulting surface-height pressure gradient is subsequently applied to every wet level at line 1245. Thus ICON permits sea level to compensate much of the depth-uniform conservative pressure force. The compensation need not be instantaneous or complete, particularly with bathymetry, Coriolis effects and horizontally varying wave forcing.

However:
- $0.82$ or $1.04\ \mathrm{kg\,m^{-3}}$ is not a density anomaly inserted into ICON. It is only a pointwise density-equivalent scaling.
- $1.04\ \mathrm{kg\,m^{-3}}$ is significant but not extraordinarily large for the upper ocean. It is comparable to strong seasonal or frontal contrasts, larger than many weak mesoscale anomalies, and smaller than common surface-to-thermocline density changes.
- Whole-column pressure communication is an intentional consequence of hydrostatic integration, not automatically an error.
- The maximum local source alone cannot determine whether the deep circulation is strongly or spuriously affected. The decisive quantity is its vertically integrated, horizontally varying pressure potential.

## 12. Adams-Bashforth Bootstrap
The implementation defines:
```fortran
l_ab_startup = is_first_timestep .OR. is_stokes_activation_step
```

During a cold start or Eulerian-restart activation:

$$
g_{\mathrm{nimd}}=g_n.
$$

This uses a first-order Euler step and avoids combining a new wave/Lagrangian
tendency with old no-wave AB history.

On subsequent timesteps:

$$
g_{\mathrm{nimd}}
=
(1.5+\mathrm{ab\_const})g_n
-(0.5+\mathrm{ab\_const})g_{n-1}.
$$

At the end of the activation step, the usual pointer swap makes the
wave-enabled $g_n$ valid AB history, so normal AB2 resumes automatically.

The activation flag affects only:

- The Stokes time derivative.
- The AB tendency combination.

It does not trigger ICON's general cold-start advection branch.

## 13. Vertical Velocity and Tracers

After the corrected horizontal velocity is obtained:

```fortran
CALL calc_vert_velocity(...)
```

diagnoses $w^L$ from continuity using Lagrangian horizontal transport.

The existing tracer-transport pathway then uses the Lagrangian horizontal and
vertical transport. No separate Stokes tracer-advection operator is required.

## 14. No-Wave Behavior

When `l_stokes_forcing=.FALSE.`:

```fortran
wavy_hydrostatic_source = 0
stokes_rhs               = 0
```

`calc_internal_press_grad` is called without the optional wave source, and the
original ICON-O momentum pathway is preserved.

## 15. Verification Completed

- NAG compilation passed.
- Intel compilation passed.
- Standalone Stokes operator modes 120-125 passed.
- Surface and interior vertical-shear derivatives matched analytical
  expectations.
- Time tendency gave the exact finite difference and zero startup value.
- Uniform/zero Stokes fields produced zero vorticity tendency.
- Vorticity and its tendency passed the linear scaling test.
- The diagnosed `stokes_rhs` closure error was approximately $10^{-13}$.
- First-hour wave-minus-control velocity correlated with prescribed Stokes
  velocity at approximately $0.99$.
- Wave-to-wave restart completed without a second Stokes insertion.
- No-wave-to-wave restart correctly converted Eulerian velocity, zeroed the
  activation-step Stokes time derivative, and bootstrapped AB at first order.

## 16. Remaining Limitations

- The validated implementation targets the non-zstar mimetic Adams-Bashforth
  pathway.
- GPU/OpenACC execution has not been validated and is not currently required.
- The cold-start `00:00` output is written before Stokes insertion.
- Cold starts from rest have zero initial $w^L$, so the shear tendency begins
  on the following step; Eulerian-restart activation explicitly rediagnoses
  $w^L$.
- Surface drag currently receives velocity diagnostics reconstructed from
  prognostic `vn`. With waves enabled, this can mean Lagrangian rather than
  Eulerian ocean current; the drag-relative-velocity treatment still needs
  physical review.
- `stokes_vn_old` is updated during tendency calculation rather than only after
  complete timestep success.