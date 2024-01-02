<!-- TOC -->
* [§0. Prelude](#0-prelude)
* [§0.1 Python Requirements](#01-python-requirements)
* [§1. Raw Toronto Image](#1-raw-toronto-image)
* [§2. Landsat 7 Channel 4 (Near Infrared) Coefficients](#2-landsat-7-channel-4-near-infrared-coefficients)
* [§3. Deriving Irradiance For Incident Radiation](#3-deriving-irradiance-for-incident-radiation)
* [§4. Raster Calculator to Convert Digital Number (DN) to Reflectance](#4-raster-calculator-to-convert-digital-number-dn-to-reflectance)
* [§5. Ground Reflectance Output](#5-ground-reflectance-output)
* [§6. Sample Reflectance Calculator](#6-sample-reflectance-calculator)
<!-- TOC -->

# §0. Prelude

In order to accurately classify surface types within a satellite image, it is important to derive _**Reflectance**_
Values first from the Digital Numbers (DN) or Brightness Values. The underlying reason is each surface type reflects
radiation in very specific thresholds. For example, water absorbs most incident solar radiation with a reflectance of
near 0%. Clouds appear very right at nearly 80% as it reflects most incident solar radiation.

# §0.1 Python Requirements

The needed Python packages are specified in [requirements.txt](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/requirements.txt) which are as follows:

* SQLAlchemy~=2.0.23
* rasterio~=1.3.9
* numpy~=1.26.1
* Pillow~=10.1.0


# §1. Raw Toronto Image

Data is achieved from an open data source http://earthexplorer.usgs.gov/, if you are interested in. The naming
convention used, for example, “LT40250112010034EDC00_B3.TIF” indicates that the file is the Digital Number (DN) values
of band **3** of the Landsat TM4 sensor, it is acquired on day 034, 2010. The image is located on path
025, row 011 of the Global Landsat coordinates.

![](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/output/Toronto_band4_Near_IR.png "Toronto Original Image in Near Infrared")

![](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/output/toronto_original_image_histogram.png "Toronto Original DN Histogram")


# §2. Landsat 7 Channel 4 (Near Infrared) Coefficients

Channel 4 coefficients are stored as ASCII in the downloaded meta-data from Landsat 7 data source data source http://earthexplorer.usgs.gov/, and in turn, stored in the [Config.ini](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/Python/config.ini#L12-L20) File.

| Coefficient             |  Value   |                      Units                      |                                   Description                                    |
|-------------------------|:--------:|:-----------------------------------------------:|:--------------------------------------------------------------------------------:|
| Offset                  |  -4.50   | W m<sup>-2</sup> μm<sup>-1</sup>sr<sup>-1</sup> |                 Radiant Flux Per Unit Wavelength Per Solid Angle                 |
| Gain                    | 0.635294 |        W m<sup>-2</sup> μm<sup>-1</sup>         |                 Radiant Flux Per Unit Wavelength Per Solid Angle                 |
| Path Radiance           |   1.5    | W m<sup>-2</sup> μm<sup>-1</sup>sr<sup>-1</sup> | Atmospheric Scattering Effect - Radiant Flux Per Unit Wavelength Per Solid Angle |
| Atmosphere Transitivity |   0.8    |                    unit-free                    |       Percentage of Radiation Atmosphere Transmits at Pertient Wavelength        |
| Solar Radiance          |   714    | W m<sup>-2</sup> μm<sup>-1</sup>sr<sup>-1</sup> |                 Radiant Flux Per Unit Wavelength Per Solid Angle                 |
| Zenith Angle            |   41.4   |                     Degrees                     |    Zenith Angle at which Satellite Instrument is Receiving Incident Radiation    |
| Channel                 |    4     |           Near Infrared 0.76-0.90 μm            |                           Satellite Instrument Number                            |
| Cell Size               |    30    |                     meters                      |       Landsat 7 uses 30 meters X 30 meters of Ground Resolution Per Pixel        |

# §3. Deriving Irradiance For Incident Radiation

Ground Reflectance, ρ, is a unit-free metric calculated as the Received Irradiance divided by Incident Radiation. For
example, a surface has 60% Reflectance as it reflects 60% of all incident radiation in all directions.

ρ = ( Reflected Irradiance ) ÷ ( Incident Irradiation ) =  ( π x Radiance ) / ( Incident Irradiation )

# §4. Raster Calculator to Convert Digital Number (DN) to Reflectance
Utilizing Python's [RasterIO Library]([https://rasterio.readthedocs.io/en/stable/) to convert the recorded Digital Number (DN) or Brightness Value (BV) from 0 to 255 in the stored 8-bit scale to a Percentage Value.  
The latter is achieved vai the function [compute_reflectance](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/Python/preprocessor.py#L113-L131) via the equation

ρ = ( Reflected Irradiance ) ÷ ( Incident Irradiation )

ρ = ( π x Radiance ) ÷ ( Incident Irradiation )

ρ = ( π x { (L<sub>total</sub>) - L<sub>path_radiance</sub>) } ÷ ( Solar Irradiation )

where 

Solar Irradiation =  E x T = E<sub>0</sub> Cosine(θ<sub>s</sub>)

E<sub>0</sub> is Solar Irradiance at the top of the atmosphere

θ<sub>s</sub> is the Solar Zenith Angle

# §5. Ground Reflectance Output

Observe that the Digital Numbers are no longer distributed between [ 0, 255 ] but between [ 0, 1] depending on surface type.  Because of the predominance of the _Water Surface_ which absorbs radiation, there is a pronounced peak near a reflectance of " 0 ".

![](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/output/toronto_ground_reflectance.png "Toronto Reflectance  in Near Infrared")

![](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/output/toronto_ground_reflectance_histogram.png "Toronto Ground Reflectance Histogram")

![](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/output/toronto_original_vs_reflectance.png "Toronto Original vs Reflectance Range")


# §6. Sample Reflectance Calculator

The Excel file, [reflectance_sample_calculator.xlsx](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/output/reflectance_sample_calculator.xlsx), has been attached to demonstrate per-pixel calculation.
