<!-- TOC -->
* [Ground Reflectance Using Landsat 7 Satellite Images](#ground-reflectance-using-landsat-7-satellite-images)
  * [§0. Prelude](#0-prelude-)
  * [Raw Toronto Image](#raw-toronto-image)
<!-- TOC -->

# Ground Reflectance Using Landsat 7 Satellite Images

## §0. Prelude 
In order to accurately classify surface types within a satellite image, it is important to derive _**Reflectance**_ Values first from the Digital Numbers (DN) or Brightness Values.  The underlying reason is each surface type reflects radiation in very specific thresholds.  For example, water absorbs most incident solar radiation have a reflectance of near 0%.  Clouds appear very right near 100% as it reflects most incident solar radiation.  

## Raw Toronto Image
Data is achieved from an open data source http://earthexplorer.usgs.gov/, if you are interested in.   The naming convention used, for example, “LT40250112010034EDC00_B3.TIF” indicates that the file is the Digital Number (DN) values of
band **3** of the Landsat TM4 sensor, it is acquired on day 034, 2010. The image is located on path
025, row 011 of the Global Landsat coordinates.









