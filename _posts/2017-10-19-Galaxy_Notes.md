---
layout: post
title:  "Galaxy Notes"
date:   2017-10-19
desc: "Galaxy Notes"
keywords: "Galaxy, Notes"
categories: [Galaxy]
tags: [Notes]
icon: icon-smile
---

# MaNGA Project

## Overview

MaNGA uses integral field unit (IFU) spectroscopy to measure spectra for hundreds of points within each galaxy.

MaNGA will provide *two-dimensional maps of stellar velocity and velocity dispersion, mean stellar age and star formation history, stellar metallicity, element abundance ratio, stellar mass surface density, ionized gas velocity, ionized gas metallicity, star formation rate and dust extinction* for a statistically powerful sample. 

redshift z~0.03

selection cuts applied only to **redshift, *i*-band luminosity,** and for a subset of galaxies, **NUV-r color.**

- Roughly flat stellar mass distribution with M > 10^9 M☉ 


- IFU size from 19,37,61,91,127 fibers, diameters from 12″ to 32″

**Data Reduction Pipeline (DRP)**

objra, objdec, ifuglon, ifuglat, ifura, ifudec, airmsmed, seemed, g/r/i/zfwhm, zmin, zmax, szmin, szmax, ezmin, ezmax

nsa_elpetro_absmag(Absolute magnitude in rest-frame SDSS ?-band, from elliptical Petrosian fluxes (NSA)), 

nsa_elpetro_flux(Elliptical SDSS-style Petrosian flux in SDSS -?band (using r-band aperture) (NSA)), 

nsa_sersic_absmag(Absolute magnitude in rest-frame SDSS ?-band, from Sersic fluxes (NSA))

nsa_sersic_flux(Two-dimensional, single-component Sersic fit flux in SDSS ?-band (fit using r-band structural parameters) (NSA))

nsa_sersic_th50: 50% light radius of two-dimensional, single-component Sersic fit to r-band (NSA)

nsa_sersic_phi: Angle (E of N) of major axis in two-dimensional, single-component Sersic fit in r-band (NSA)

nsa_sersic_ba: Axis ratio b/a from two-dimensional, single-component Sersic fit in r-band (NSA)

nsa_sersic_n: Sersic index from two-dimensional, single-component Sersic fit in r-band (NSA)

nsa_petro_flux_r: Azimuthally-averaged SDSS-style Petrosian flux in SDSS r-band (using r-band aperture) (NSA)

nsa_petro_th50: Azimuthally averaged SDSS-style Petrosian 50% light radius (derived from r band) (NSA)

nsa_extinction_r: Galactic extinction from Schlegel, Finkbeiner, and Davis (1997), in SDSS r-band (NSA)

**In these catalogs, each galaxy has a unique [MaNGA-ID](http://www.sdss.org/dr14/manga/getting-started/#mangaid)**. Each galaxy has a unique MaNGA-ID, but a galaxy may have multiple PLATE-IFUDESIGN designations if it has been observed multiple times.

row-stacked spectra (**RSS**) and datacubes (**CUBE**) files that are sampled either linearly (**LIN**) or log-linearly (**LOG**) in wavelength



**DAP**:These science-grade data cubes are then processed by the MaNGA Data Analysis Pipeline (DAP), which measures the shape and location of various spectral features, fits stellar population models, and performs a variety of other analyses necessary to derive astrophysically meaningful quantities from the calibrated data cubes.



 a LINEAR version with a wavelength step of 1.0 Angstroms per pixel, and a LOG version with a wavelength step of 1e-4 in units of log10(lambda/Angstroms) per pixel. 

### RSS & CUBE

The **RSS** file contains *all* spectra acquired in each exposure in a single 2D array; for example, an **RSS** file for a 19-fiber bundle may contain 19(fibers)*4(dither sets)*3(exposures per dither)=228 spectra, each with the same number of spectral pixels. 

The **CUBE** file uses the **RSS** spectra to construct a uniformly sampled datacube, a 3D array with WCS coordinates (RA, DEC, λ); the spatial sampling is set to 0.5″ spaxels, but the spatial point-spread function (PSF) in the datacube is roughly 2.5″ at FWHM. 

when reforming the raw spectra to a useful diagram, drp uses either LINEAR wavelength coordinate or LOG wavelength coordinate.

The *difficulty with the CUBE data* is due to the significant covariance between adjacent spaxels. Each spaxel is approximately 20% of the FWHM of the spatial PSF meaning that a single fiber contributes to many spaxels. Analysis of the datacube should then account for this spatial covariance; i.e., one should not treat the datacube as containing independent spectra. 

### Observing Strategy

The typical site seeing has a full width at half maximum (FWHM) of 1.5″. Our hexagonal-formatted fiber bundles are made from 2″-core-diameter fibers with 0.5″ gaps between adjacent fiber cores. So MaNGA use **dithering**. The three “dither” positions form a equilateral triangle with 1.44″ on a side.

Data in CUBE is a regular 0.5 arcsecond grid (in r.a. and dec., these two directions).

#### How to get the list of observed plates?

**Python**

It is possible to obtain a list of all the observed plates from the [drpall](http://www.sdss.org/dr14/manga/manga-data/metadata/) file, as well as select plates that contain certain targets in which you are interested. Using [Astropy](http://docs.astropy.org/en/latest/) and [Numpy](http://docs.scipy.org/doc/numpy-1.10.1/index.html) to obtain a list of all reduced plates, you can do

```
from astropy.table import Table
import numpy as np
drpall = Table.read('drpall-v2_1_2.fits')
plates = np.unique(drpall['plate'])
```

At this point you may want to reject plates without galaxy targets, for instance:

```python
targets = drpall[np.where((drpall[‘mngtarg1'] > 0) | (drpall['mngtarg3'] > 0))]
plates = np.unique(drpall['plate'])
```

If you are interested in finding all plates that contain [ancillary targets](http://www.sdss.org/dr14/manga/manga-target-selection/ancillary-targets/) for a specific program, you can use the `manga_target3` column (see more about [bitmasks](http://www.sdss.org/dr14/algorithms/bitmasks/)). For instance, the bit for the Milky Way analogues program is 13. To get the plates for the two observed targets in DR14, we do

```
targets = drpall[np.where(drpall['mngtarg3'] & 1<<13)]
plates = np.unique(targets['plate'])
```

# Schechter Mass Function
The Press–Schechter formalism predicts that the number of objects with mass between <script type="math/tex">M</script> and <script type="math/tex">M+dM</script> is:

<script type="math/tex; mode=display"> N(M)dM=\frac{1}{\sqrt{\pi}}(1+\frac{n}{3})\frac{\bar\rho}{M^2}\left(\frac{M}{M^*}\right)^{(3+n)/6} \exp\left(-\left(\frac{M}{M^*}\right)^{(3+n)/3}\right) dM</script>

where <script type="math/tex">{\bar {\rho }}</script> is the mean (baryonic and dark) matter density of the universe, <script type="math/tex">n</script> is the index of the power spectrum of the fluctuations in the early universe <script type="math/tex">P(k)\propto k^{n}</script>, and<script type="math/tex">M^*</script>is a critical mass above which structures will form.
<br/>

<br/>

# <script type="math/tex">D_{4000}</script> Break

[Literature](http://adsabs.harvard.edu/full/1997A%26A...325.1025P) and [Galaxy Zoo](http://www.galaxyzooforum.org/index.php?topic=1923.0) talked about <script type="math/tex">D_{4000} </script> break. Briefly speaking, <script type="math/tex">D_{4000}</script> break is the ratio between the average flux density in <script type="math/tex">[4050A,4250A]</script> and in <script type="math/tex">[3750A,3950A]</script>. We can see from Fig.1, the ratio of average flux density in this two area can be calculated easily.

<center><img src="{{ site.img_path }}/Galaxy_Notes/hkbreak.jpg" width="70%"></center>

<center>Fig.1 <script type="math/tex">D_{4000}</script> break for galaxy</center>

<br/>

And <script type="math/tex">D_4000</script> break can be a proxy for stellar age of one galaxy, as Fig.2 shows.

<center><img src="{{ site.img_path }}/Galaxy_Notes/D4000.jpg" width="70%"></center>

<center>Fig.2 <script type="math/tex">D_{4000}</script>  is a proxy for stellar age</center>

<br/>


As we can see, <script type="math/tex">D_{4000}</script> ratio is proportional to the stellar age in a galaxy.





