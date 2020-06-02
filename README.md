# RAPID-NRT-flood-maps-on-AWS
Product Info, Background, and Data Structure

# Product Info
RAPID NRT flood maps is an inundation dataset, delineated based on the Synthetic Aperture Radar (SAR) imagery from Sentinel-1 and multiple ancillary data, including high-resolution topography, high-resolution water occurrence, land cover classification, river width, hydrography, and water type databases. It labeled inundated and dry areas in each binary flood maps, no further process is required before using the data.

# Backgroud
High-resolution water mask of flooding is a fundamental data source for hazard assessment and relief organization. Here we introduce RAPID NRT flood maps, an unprecedented high-resolution (10 m) flood inundation dataset over the Contiguous United States (CONUS), derived from the Sentinel-1 SAR imagery using an automated Radar Produced Inundation Diary (RAPID) system. To enable the big data processing at national scale, we implemented a triggering mechanism that relies on both in-situ stream stage observations and satellite precipitation estimations to initially identify potential flooded areas within which we acquire and process overpassing SAR images. The flood trigger detects two types of flooding, fluvial and pluvial, as depicted by Fig. 1. For the fluvial flooding, it applies the National Weather Service (NWS) flood stage threshold to U.S. Geological Survey (USGS) stream stage measurements to provide the daily flood status (flooded or unflooded) at around 4,455 stations. Based on the spatial proximity and temporal continuity of the daily potential flood polygons, we further define flood events. Take the advantage of the acquisition intervals of Sentinel-1 (6 days average), RAPID NRT flood maps have the capacity to capture middle to large inundation events lasting from several days to months. See Shen et al. (2019a) for more information about the detail kernel algorithm used to generate these data.

![alt text](https://github.com/QingYang6/RAPID-NRT-flood-maps-on-AWS/blob/master/Figure%201.png)
Fig. 1. The automated process of the RAPID system, from flood event discovery to production of an inundation map.


# Data Types
The data product is raster data containing open water mask delineated from flood and non-flood SAR images (Shen et al, 2019a) at a 10m resolution across the CONUS.

# Data Structure
Data are group by flood and non-flood water masks, located in a unique run path. The structure of the run path is: date/flooding_<SAR_image_name>. The format of the date is YYYYMMDD representing the date of flood occurrence. The structure of the SAR_image_name is: MMM_BB_TTTR_LFPP_YYYYMMDDTHHMMSS_YYYYMMDDTHHMMSS_OOOOOO_DDDDDD_CCCC.  A full explanation of the SAR image convention can be found in the Sentinel program at this url: https://sentinel.esa.int/web/sentinel/user-guides/sentinel-1-sar/naming-conventions. Note that there could be multiple flood SAR images in the same date.

For each individual water mask, the data are structured in the following way:

/<run_path>/flood_WM_<SAR_image_name>.tif<br />
/<run_path>/non_flood_WM1_<SAR_image_name>.tif<br />
…<br />
/<run_path>/non_flood_WMn_<SAR_image_name>.tif

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
[1]  Shen, X., Anagnostou, E. N., Allen, G. H., Brakenridge, G. R. & Kettner, A. J. Near Real-Time Nonobstructed Flood Inundation Mapping by Synthetic Aperture Radar. Remote Sensing of Environment 221, 302-335, doi:10.1016/j.rse.2018.11.008 (2019a).<br />
[2]  Shen, X., Dacheng W., Kebiao M., Anagnostou, E.N. and Hong Y.: Inundation Extent Mapping by Synthetic Aperture Radar: A Review, Remote Sensing, 11, 879, (2019b).

