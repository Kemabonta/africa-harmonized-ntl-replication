# Africa harmonized nighttime lights (1992–2024 replication products)

Author: Tam Kemabonta

This repository holds Africa-masked, DMSP-like digital number (DN) grids produced while replicating and stress-testing the Li et al. (2020) VIIRS-to-DMSP harmonization. Files are annual GeoTIFFs for Africa only.

## What is in this repository

Four product families are included. Years are those that exist for each family.

### 1. `li_replicated_monthly/published_coefficients/`

Exact replication of the Li et al. (2020) method applied to EOG monthly VIIRS composites, using the published sigmoid coefficients. Files:

`LiRep_Monthly_DN_published_{2013..2024}.tif`

### 2. `li_replicated_monthly/refit2013/`

Same monthly-input pipeline as above, but with sigmoid coefficients refit on the 2013 calibration overlap and then frozen for later years. Files:

`LiRep_Monthly_DN_refit2013_{2013..2024}.tif`

### 3. `noaa_rebuild/published_coefficients/`

Independent rebuild that applies the same Li-style sigmoid to NOAA/EOG annual VNL v2 `average_masked` composites, using the published coefficients. Files:

`NOAA_Rebuild_DN_published_{2014..2024}.tif`

### 4. `noaa_rebuild/refit2013/`

Same annual-input rebuild with the 2013 refit coefficients. Files:

`NOAA_Rebuild_DN_refit2013_{2014..2024}.tif`

## Grid

- Domain: Africa, longitude 26°W–64°E, latitude 40°S–40°N
- Resolution: 30 arc-seconds (~1 km)
- Values: unsigned 8-bit DN, range 0–63
- Coordinate reference: EPSG:4326 (WGS 84 geographic)
- Compression: DEFLATE

## Method

The conversion follows Li et al. (2020, *Scientific Data*): VIIRS radiance is processed to a DMSP-like DN with a sigmoid. In short, monthly or annual VIIRS radiance is cleaned at a low radiance floor, density-smoothed, log-transformed, then mapped through

DN = round( sigmoid(x; a, b, c, d) )

and clipped to 0–63.

**Published coefficients** (Li et al.): a = 6.5, b = 57.4, c = 1.9, d = 10.8.

**2013 refit coefficients** (fit on the 2013 calibration overlap for this Africa grid, then frozen): a = 5.818386489094427, b = 56.979953621316824, c = 1.5446508432770731, d = 11.163695761894049 (about 5.82, 56.98, 1.54, 11.16).

Neither product family was tuned to Li’s 2014–2024 published grids.

## Inputs

- **Monthly replication** (`li_replicated_monthly/`): EOG monthly VIIRS `vcmcfg` composites.
- **NOAA rebuild** (`noaa_rebuild/`): NOAA/EOG annual VNL version 2 `average_masked` composites.

## Key findings

- Background noise in the monthly VIIRS files, after smoothing and the sigmoid, becomes DN 6–7 over dark land.
- The monthly-file replication matches Li in about 84% of Africa cells in 2014 and about 8% in 2024 (identical DN).
- The NOAA annual rebuild matches Li in about 87–94% of Africa cells for 2014–2024.
- Li’s paper does not document how background noise is handled.
- Li re-uploaded the 2014 and later data on Figshare in 2025; work here uses those current files.

Together, these results suggest that Li’s post-2013 products were built from noise-masked annual-type inputs or with an extra cleaning step that is not described in the paper.

## Citation

**Data (harmonized global nighttime light grids):**  
Li, X., et al. Harmonized global nighttime light dataset 1992–2018. Figshare. https://doi.org/10.6084/m9.figshare.9828827

**Paper:**  
Li, X., Zhou, Y., Zhao, M., & Zhao, X. (2020). A harmonized global nighttime light dataset 1992–2018. *Scientific Data*, 7, 168. https://doi.org/10.1038/s41597-020-0510-y

If you use these replication products, please also cite this repository and name Tam Kemabonta as author.

## License

License is TBD.
