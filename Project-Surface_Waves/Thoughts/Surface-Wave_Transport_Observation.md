---
tags:
  - wave/surface_wave
  - project/surfwaves
Last Eddited: 2025-12-01
---
The note for the Video: Waves Across the Pacific ([https://www.youtube.com/watch?v=MX5cKoOm6Pk](https://www.youtube.com/watch?v=MX5cKoOm6Pk))

# Relationship between wavelength, wavenumber and frequency

Notation:
- wavelength: $\lambda$
- wavenumber: $k=\frac{2\pi}{\lambda}$
- Frequency:
    - ordinary frequency (cycles per second): $f=\frac{1}{T}$
    - angular frequency (radians per second): $\omega=2\pi f=\frac{2\pi}{T}$

the wavenumber and frequency are related via **Dispersion relationship**:

$$ \omega=\omega(k)=\sqrt{gk} \quad \quad \text{(in deep-water limit)} $$

# **Gravity waves are dispersive: longer waves travel faster**

This happens because the restoring force (gravity) interacts with wavelength in a way that makes **longer waves feel “more gravity” per unit curvature**, causing them to propagate faster.

## **Why gravity waves behave this way**

A gravity wave is supported by the balance between:
- **gravity**, which pushes the fluid back toward level, and
- **inertia**, which makes the fluid overshoot.

Short waves have **high curvature** (lots of bending of the surface), so:
- They require more acceleration to move water up and down.
- They are slowed by water’s inertia.
- They cannot transmit the gravitational restoring force efficiently across crests.

Long waves have **gentler curvature**:
- Gravity acts coherently over a larger horizontal scale.
- Less vertical acceleration is needed.
- Water parcels move more “in phase.”
- Energy propagates more efficiently.

Thus **long waves travel faster**.

## **Mathematical reason: deep-water gravity wave dispersion**

In deep water, the dispersion relation is:
$$ \omega^2 = gk, $$
where
- $k$ = wavenumber = $2\pi/\lambda$
- $\omega$ = angular frequency
- $g$ = gravity

Phase speed:
$$ c = \frac{\omega}{k} = \sqrt{\frac{g}{k}}. $$
Since **long waves have small $k$**:
$$ k\downarrow \quad \Rightarrow \quad c \uparrow $$
Thus:

- Longer wavelength → smaller wavenumber → faster wave.

This matches exactly what is measured in oceans.


## **Real-world example: ocean swells**

Storm-generated ocean swells:

- Short, steep waves appear first near the storm.
- Far from the storm, only **longer, faster waves survive**, arriving first at coasts.
- The wave period increases with distance because the fastest (long-period) waves outrun the rest.

This is the classical **wave dispersion** observed by mariners for centuries.


## Summary

**Long-period (long-wavelength) gravity waves travel faster because:**

- The gravitational restoring force acts more efficiently over long distances.
- Short waves involve large curvature and high vertical acceleration → slowed by inertia.
- The dispersion relation $c=\sqrt{gk}$ mathematically shows the inverse relation between speed and wavenumber.

If you want, I can also explain:

- why shallow-water waves behave differently (non-dispersive),
- how group velocity differs from phase velocity, or
- how this connects to surface wave energy and dispersion diagrams.

# **Why do observations say “the group velocity is nearly half the wave speed”?**

For **deep-water gravity waves**, the dispersion relation is:

$$ \omega^2 = gk $$

Phase velocity (speed of individual crests):

$$ c = \frac{\omega}{k} = \sqrt{\frac{g}{k}} $$

Group velocity (speed of wave energy):

$$ c_g=\frac{\partial \omega}{\partial k} = \frac12 \sqrt{\frac{g}{k}} = \frac12 c. $$

**This is a universal result for deep-water gravity waves:**

$$ \boxed{c_g = \frac12 c} $$

So the statement “group velocity is about half the phase velocity” is directly predicted from the physics—**it is not an empirical coincidence**.

This is why swell energy moves slower than the visible crests.


## **How do scientists measure group velocity if they only record wave period?**

They **do not directly measure crests moving** across the ocean.

Instead, they track **energy packets** arriving at many stations.

### **Step 1 — Measure wave period at each buoy**

At each station, an instrument records:
- the surface elevation time series
- the dominant wave period $T$

Since for deep-water gravity waves:

$$ c = \frac{gT}{2\pi}, \qquad c_g = \frac{c}{2} $$

just measuring $T$ already gives a theoretical value for both speeds.

> This is because, we have the phase speed
> 
> $$c=\sqrt{g/k}$$
> and because $\omega=\frac{2\pi}{T}$ and $\omega=\sqrt{gk}$, we have
> $$ k=\frac{\omega^2}{g}=\frac{(2\pi/T)^2}{g}=\frac{4\pi^2}{gT^2} $$
> then the group velocity is $c=\sqrt{g/k}$

### **Step 2 — Observe arrival times of swell groups at widely separated stations**

Storm in Southern Ocean → creates a wave field with many different wavelengths.

At multiple buoys (e.g., distributed from Southern Ocean → Hawaii → Alaska), they record:
- when the **energy** (wave packet) first arrives,
- and the **dominant period** at that arrival.

Long-period waves arrive first because they have the largest group speed.

Shorter-period waves arrive later.

This spreading in time is called **dispersion**.


### **Step 3 — Compute observed group velocity**

If two buoys are separated by distance DDD, and the same swell group arrives at times $t_1$ and $t_2$:

$$ c_g^{\text{obs}} = \frac{D}{t_2 - t_1}. $$

This gives the **actual propagation speed of the wave packet**, not the motion of individual crests.


### **Step 4 — Compare with the theoretical value**

Using the measured swell period $T$:

$$ c_g^{\text{theory}}=\frac12\sqrt{\frac{g}{k}} = \frac12 \frac{gT}{2\pi}. $$

And the observations show:

$$ c_g^{\text{obs}} \approx \frac12 c^{\text{phase}}. $$

Which beautifully confirms deep-water dispersion theory.

# From the Observation by Monk, their conclusions

- **The early arrivals of the low-frequency waves** (i.e., long period wave; long waves) → long waves travel faster. **Which is following by a subsequent higher frequencies**
    - this can be observed in the Figure of wave spectrum (wave energy again wave frequency).
    - In the spectrum plot, the peak indicates where we have the first arrivals
- As time continues, the spectrum ‘peak’ shifted towards higher frequency and became stronger until the peak diminished
    - The entire wave group starting from lower frequency to higher frequency because of dispersion
- **The duration of the wave group** (i.e., time where we have observed the peak until the vanish of the peak in high frequency) **increases when the wave travel further**
    - because the dispersion stretch the wave train
- **The attenuation of the waves** (which is observed from station to stations) **is largely the result of interaction between waves from the SAME storm**
    - The high frequencies were reduced quite a bit in the beginning, but not the low frequencies
    - However, after the first more significant reduction, the attenuation was small for all frequencies, until the very end of the wave journey (From Samona to Alaska)
        - No significant loss of energy across the equator trade wind zone!!!
- **The attenuation (or interaction between waves) is weak once waves are well sorted by dispersion**
    - which explains why the attenuation is initially large and then become negligible
- **The attenuation is weak at low frequencies but strong in low frequencies**