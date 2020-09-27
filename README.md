# RAPID-NRT-flood-maps-on-AWS
Product Info, Background, and Data Structure

# Product Info
RAPID NRT flood maps is an inundation dataset, delineated based on the Synthetic Aperture Radar (SAR) imagery from Sentinel-1 and multiple ancillary data, including high-resolution topography, high-resolution water occurrence, land cover classification, river width, hydrography, and water type databases. It labeled inundated and dry areas in each binary flood maps, no further process is required before using the data.

# Backgroud
High-resolution water mask of flooding is a fundamental data source for hazard assessment and relief organization. Here we introduce RAPID NRT flood maps, an unprecedented high-resolution (10 m) flood inundation dataset over the Contiguous United States (CONUS), derived from the Sentinel-1 SAR imagery using an automated Radar Produced Inundation Diary (RAPID) system. To enable the big data processing at national scale, we implemented a triggering mechanism that relies on both in-situ stream stage observations and satellite precipitation estimations to initially identify potential flooded areas within which we acquire and process overpassing SAR images. The flood trigger detects two types of flooding, fluvial and pluvial, as depicted by Fig. 1 (Yang et al. 2020). For the fluvial flooding, it applies the National Weather Service (NWS) flood stage threshold to U.S. Geological Survey (USGS) stream stage measurements to provide the daily flood status (flooded or unflooded) at around 4,455 stations. Based on the spatial proximity and temporal continuity of the daily potential flood polygons, we further define flood events. Take the advantage of the acquisition intervals of Sentinel-1 (6 days average), RAPID NRT flood maps have the capacity to capture middle to large inundation events lasting from several days to months. See Shen et al. (2019a) for more information about the detail kernel algorithm used to generate these data.<br />

![alt text](https://github.com/QingYang6/RAPID-NRT-flood-maps-on-AWS/blob/master/Figure%201.png)
Fig. 1. The automated process of the RAPID system, from flood event discovery to production of an inundation map.


# Data Types
The final product contains two sub-datasets:  flood inundation events information, stored as time series of multi-polygon in ESRI shapefile format with a unique ID, start date, and ending date fields; and binary flood extent rasters (10m resolution) with each pixel labeled as 1 (flooded) and 0 (nonflooded) derived based on the flood and dry-time SAR images.  We also generate a separate list to link the file name of each flood extent raster to the associated event ID to facilitate event-wise queries.<br />


# Data Search Suggestion
To quick locate the flood maps of interest from our database, two strategies: a) find the SAR image ID and date using extent based Web searching interface, such as Copernicus Open Accessed Hub (https://scihub.copernicus.eu/dhus/#/home) or Alaksa Satellite Facility (https://search.asf.alaska.edu/#/), then search the flood maps; b) event wise query based on the flood events and linking SAR images information available in our dataset. <br /><br />
We also provide "quick look" pictures for each flood map to show the map location and flood severity. The flood severity quick look picture is consisted of flood inundation (red), permanent water body (blue) and impervious surface (gray). Both quick looks are stored in the .jpeg format that does not require a GIS tool to view. An example of the “quick look” pitrues is shown as follow:<br />

![alt text](https://github.com/QingYang6/RAPID-NRT-flood-maps-on-AWS/blob/master/FloodMap_quicklookexample.png)<br />
Fig. 2. Example of flooded image location and flood map quick look.

Moreover, we are working on extend our dataset to the global scale and by now providing a near-real-time (NRT) flood inundation maps for global flood events with quick look poster. Limited by the knowledge of flood event location outside of the CONUS, the NRT system now only deal with the emergency response activated by UN disaster charters. A flood map example of recent flood event happened in India and Bangladesh caused by Tropical Cyclone Amphan is shown as follow:<br />

![alt text](https://github.com/QingYang6/RAPID-NRT-flood-maps-on-AWS/blob/master/Flood%20maps%20Cyclone%20Amphan%200522.png)<br />
Fig. 3. NRT flood maps of Tropical Cyclone Amphan.

# Data Structure
An web explorer of the database is available at  https://rapid-nrt-flood-maps.s3.amazonaws.com/index.html. <br /><br />
Archive flood events are stored in #Archive_Flood_Events/. The start date, end date, dynamic and maximal extent are stored in separate folder with unique event ID. Shapefile consist of the maximal potential flooded zone from all flood events is locate at Archive_Flood_Events/TotalFloodEventsInfo.shp. To browse the events captrued by SAR images, refer to #Archive_Flood_Events/EventswithDFOandImages.shp or #Archive_Flood_Events/List_EventsLinktoDFOandFloodMaps.xlsx <br /><br />
Archive flood maps are grouped by flood and non-flood water masks, located in a unique run path. The structure of the run path is: https://rapid-nrt-flood-maps.s3.amazonaws.com/index.html#RAPID_Archive_Flood_Maps/date/flooding_<SAR_image_name>.<br />
The format of the date is YYYYMMDD representing the date of flood occurrence. The structure of the SAR_image_name is: MMM_BB_TTTR_LFPP_YYYYMMDDTHHMMSS_YYYYMMDDTHHMMSS_OOOOOO_DDDDDD_CCCC. <br />

For each individual water mask, the data are structured in the following way:

<run_path>/flood_WM_<SAR_image_name>.tif<br />
<run_path>/non_flood_WM1_<SAR_image_name>.tif<br />
…<br />
<run_path>/non_flood_WMn_<SAR_image_name>.tif<br />

The "quick look" pictures are located in the date path, and structured as:
ImageExtentLook_<CCCC>.jpeg<br />
floodquicklook_<SAR_image_name>.jpeg<br />
 
The global NRT flood maps are available at https://rapid-nrt-flood-maps.s3.amazonaws.com/index.html#Global_Flood_Event/. Data are simply grouped by the corresponding event name. 

# Projection
The data in GeoTIFF format use the WGS 1984 projection, is ideal for both analysis and mapping.

# Raster Layer Descriptions
| Type       | File Name     |
| :------------- | :----------: |
|  Flood Water Mask | flood_WM_<SAR_image_name>.tif   |
| Non-flood Water Mask   | non_flood_WMn_<SAR_image_name>.tif  |

Flood Water Mask<br />
This layer represents the binary inundation water mask delineated from flood SAR image.

Non-flood Water Mask<br />
This layer represents the binary water mask delineated from non-flood SAR image. Non-flood water mask here act as the dry time reference to the flood water mask for the purpose of change detection (Shen et al, 2019a & 2019b).  One non-flood water mask is the minimum require, 3 to 5 are expected in most of the case.

# References
[1]  Yang, Q., Shen, X., Anagnostou, E. N., Mo, C., Eggleston, J. R. and Kettner, A.J., 2020: A High-Resolution Flood Inundation Archive (2016–the Present) from Sentinel-1 SAR
Imagery over CONUS. Bulletin of the American Meteorological Society, Under Review.<br />
[2]  Shen, X., Anagnostou, E. N., Allen, G. H., Brakenridge, G. R. and Kettner, A. J., 2019a: Near Real-Time Nonobstructed Flood Inundation Mapping by Synthetic Aperture Radar. Remote Sensing of Environment, 221, 302-335, doi:10.1016/j.rse.2018.11.008.<br />
[3]  Shen, X., Dacheng W., Kebiao M., Anagnostou, E.N. and Hong Y., 2019b: Inundation Extent Mapping by Synthetic Aperture Radar: A Review, Remote Sensing, 11, 879.

