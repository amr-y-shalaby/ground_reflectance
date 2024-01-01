<!-- TOC -->

* [§0. Prelude](#0-prelude-)
* [Raw Toronto Image](#raw-toronto-image)

<!-- TOC -->

# §0. Prelude

In order to accurately classify surface types within a satellite image, it is important to derive _**Reflectance**_
Values first from the Digital Numbers (DN) or Brightness Values. The underlying reason is each surface type reflects
radiation in very specific thresholds. For example, water absorbs most incident solar radiation with a reflectance of
near 0%. Clouds appear very right at nearly 80% as it reflects most incident solar radiation.

# §1. Raw Toronto Image

Data is achieved from an open data source http://earthexplorer.usgs.gov/, if you are interested in. The naming
convention used, for example, “LT40250112010034EDC00_B3.TIF” indicates that the file is the Digital Number (DN) values
of band **3** of the Landsat TM4 sensor, it is acquired on day 034, 2010. The image is located on path
025, row 011 of the Global Landsat coordinates.

![](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/Data/Toronto_band4_Near_IR.png "Landsat 7 in Near Infrared")

# §2. Landsat 7 Channel 4 (Near Infrared) Coefficients

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

# §4. Raster Calculate to Convert Digital Number to Reflectance
Utilizing Python's [RasterIO Library]([https://rasterio.readthedocs.io/en/stable/) to convert the recorded Digital Number (DN) or Brightness Value (BV) from 0 to 255 in the stored 8-bit scale to a Percentage Value.  
The latter is achieved vai the function [compute_reflectance](https://github.com/amr-y-shalaby/ground_reflectance/blob/main/Python/preprocessor.py#L113-L131) via the equation

ρ = ( Reflected Irradiance ) ÷ ( Incident Irradiation )

ρ = ( π x Radiance ) ÷ ( Incident Irradiation )

ρ = ( π x { (L<sub>total</sub>) - L<sub>path_radiance</sub>) } ÷ ( Solar Irradiation )

where 

Solar Irradiation =  E x T = E<sub>0</sub> Cosine(θ<sub>s</sub>)
E<sub>0</sub> is Solar Irradiance at the top of the atmosphere
θ<sub>s</sub> is the Solar Zenith Angle



