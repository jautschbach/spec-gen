spec-gen.py
===========

Python tool to broaden calculated spectra (energies, intensities) with Gaussian
or Lorentzian functions. 

What types of spectra can `spec-gen.py` handle?

* Electronic absorption and Circular Dichroism (CD)
* Infrared vivrational spectra (IR) and Vibrational Circular Dichroism (VCD)
* Emission and Circularly Polarized Luminescence (to be added, not yet available)

The input must be in the format:

```
#NExcit TypeSpec
E1 I1
E2 I2
...
```

Where `NExcit` is an the number of excitations (no space between `#`
and number), `TypeSpec` can be one of `abs, ir, cd, vcd`, and `E` must
be in units proportional to the energy (energies in eV or wavenumbers
in inverse centimeter are good choices; wavelengths in nanometers are
*bad*). The intensity should be given as dimensionless oscillator
strengths for absorption, or rotatory strengths in 10E-40 cgs units.
The energy needs to be given in units of electronvolt (eV) if you want
absolute intensities. The hardcoded
conversion factors for the absorption coefficient and CD intensity
in liter/(mol cm) are from Pulm et al., Chem. Phys. 1997, 224, 143.

The `--help` option gives a preview of the functionality.
Some tips:

* Gaussian and Lorentzian Broadening Value should match the column-1 unit.
* Integration is given in spec-gen.log If it not close to 1, you will need to modify the quadrature and padding.
* You must give at least an input file name and a broadening type and value, 
otherwise the script won't do anything.

If the broadening value is given as negative, the script will apply an
empirical broadening calculated in wavenumber units of inverse cm as 7.5 times sqrt(excitation wavenumber) [Brown, A.; Kemp, C. M.; Mason, S. F.; Electronic absorption, polarized excitation, and circular dichroism spectra of [5]-helicene (Dibenzo[c,g]phenanthrene). J. Chem. Soc. (A) 1971, 751–755.]. This will only work properly if the first column in the data files is in units of eV.

Spectra are padded beyond the lowest and highest transition energy. The default padding is 5 times the chosen broadening (its absolute value, rather). You can change the default with the `--padding` option. If the broadening is given as a negative number, to trigger the empirical broadening formula described above, the absolute value is used to generate the padding. 
