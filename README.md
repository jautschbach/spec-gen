spec-gen.py
===========

Python tool to broaden calculated spectra (energies, intensities) with Gaussian
or Lorentzian functions. 

What types of spectra can `spec-gen.py` handle?

* Electronic absorption and Circular Dichroism (CD)
* Infrared vivrational spectra (IR) and Vibrational Circular Dichroism (VCD)
* MCD A and B terms
* Emission and Circularly Polarized Luminescence (to be added, not yet available)

The input must be in the format:

```
#NExcit TypeSpec
E1 I1
E2 I2
...
```

Where `NExcit` is an the number of excitations (no space between `#`
and number), `TypeSpec` can be one of `abs, ir, cd, vcd, mcda, mcdb`, and `E` must
be in units proportional to the energy (energies in eV or wavenumbers
in inverse centimeter are good choices; wavelengths in nanometers are
*bad*). The intensity should be given as dimensionless oscillator
strengths for absorption, rotatory strengths in 10E-40 cgs units for cd, or
molar ellipticity [ $\theta$ ] for mcd A/B.
The energy needs to be given in units of electronvolt (eV) if you want
absolute intensities. The hardcoded
conversion factors for the absorption coefficient and CD intensity
in liter/(mol cm) are from Pulm et al., Chem. Phys. 1997, 224, 143.

The `--help` option gives a preview of the functionality.
Some tips:

* Gaussian and Lorentzian Broadening Value should match the column-1 unit. See comments below regarding the definition of the broadening parameter.
* Integration is given in spec-gen.log If it not close to 1, you will need to modify the quadrature and padding.
* You must give at least an input file name and a broadening type and value, 
otherwise the script won't do anything.

If the broadening value is given as negative, the script will apply an
empirical broadening calculated in wavenumber units of inverse cm as 7.5 times sqrt(excitation wavenumber) [Brown, A.; Kemp, C. M.; Mason, S. F.; Electronic absorption, polarized excitation, and circular dichroism spectra of [5]-helicene (Dibenzo[c,g]phenanthrene). J. Chem. Soc. (A) 1971, 751–755.]. This will only work properly if the first column in the data files is in units of eV.

With Gaussian broadening, the normalized broadening function used is (x = frequency or energy, a = center of the peak)

$$
G = \frac{1}{\sigma \sqrt{2\pi}} \exp \left [ \frac{-(x-a)^2}{2 \sigma^2} \right ]
$$

The `spec-gen.py` input parameter for Gaussian broadening is $\sigma$, the usual standard deviation represented by the Gaussian. The peak's full width at half maximum (FWHM) is $2\sigma \sqrt{\ln 4}$ or approximately 2.3548 $\sigma$. Conversely, if you need broadening with a given FWHM, the corresponding input is $\sigma$ = FWHM/2.3548. With Lorentzian broadening, the normalized function used is 

$$
L = \frac{\gamma/2}{π} ~ \frac{1}{(x-a)^2 + (γ/2)^2}
$$

with a FWHM of $\gamma$. The `spec-gen.py` input parameter for Lorentzian broadening is equal to $\gamma$.

Spectra are padded beyond the lowest and highest transition energy. The default padding is 5 times the chosen broadening (its absolute value, rather). You can change the default with the `--padding` option. If the broadening is given as a negative number, to trigger the empirical broadening formula described above, the absolute value is used to generate the padding. 
