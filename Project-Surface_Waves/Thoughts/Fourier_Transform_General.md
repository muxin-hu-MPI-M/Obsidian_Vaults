---
tags:
  - project/surfwaves
  - wave
  - wave/surface_wave
  - Fourier_Transform
Last Eddited: 2025-12-04
---
## Fourier Transform
The video link: [https://youtu.be/gTOzmE7_-mU?si=hh7dkCD_TSs3r_HI](https://youtu.be/gTOzmE7_-mU?si=hh7dkCD_TSs3r_HI)
The paper link:

- ==Fourier transform==:
	- ~={red}**It takes a signal in time (or space) and expresses is as a sum of sinusoidal waves with different frequencies, amplitudes and phases**=~
	- It decomposes any signal into its building-block waves
	- If $X(f)$ is the Fourier Transform of $x(t)$, then the Power Spectral Density (PSD) is:
	  $$
	    PSD(f)=\frac{|X(f)|^2}{(\text{normalisation})}
		$$
		Hence, PSD represents the distribution of power (i.e., variance) per unit frequency.
	- If the FT is applied to a *time series*, then the PSD tells you:
		- ~={red}**how the variance (energy) of a signal is distributed across frequencies or wavenumbers**=~.
		- Which time scales dominate variability. It answers what time scales control the **variability** of the scaler quantity
		- Variability is dominated by rapid (high frequency) or slow (low frequency) processes