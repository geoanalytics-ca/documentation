# 2. REAL-WORLD EXAMPLES 
These are single use-case examples of how to do what is described. These examples will be used with each other to form scientific workflows in the next group of notebooks. 

## [2.1 Accessing/Downloading Data](01-accessing-data.ipynb)
- This notebook shows the multiple methods of accessing data such as MODIS, VIIRS, Sentinel-2, Landsat-8, and Sentinel-1. - Users will learn to:
    - Access the public data buckets in JupyterHub.
    - Download data that is not available in the public buckets in GEOAnalytics.
    - Index data into STAC.

## [2.2 NDVI With Dask](02-ndvi-with-dask.ipynb)
- This tutorial notebook demonstrates how to find, visualize, and analyze the Normalized Difference Vegetation Index (NDVI) with Sentinel 2 imagery, efficiently using Dask.

## [2.3 NDSI With Dask](03-ndsi-with-dask.ipynb)
- In this notebook, the user will use MODIS to calculate the Normalized Difference Snow Index to estimate the presence of snow in a pixel and can be extrapolated to real-world situations with avalanches and spring-runoff conditions. 

## [2.4 NDWI With Dask](04-ndwi-with-dask.ipynb)
- The user will use Landsat-8 data to compute the Normalized Difference Water Index (NDWI). 

## [2.5 Change Detection](05-change-detection.ipynb)
- This notebook goes through a naive approach of identifying Deforestation within Canadian forests by computing the Normalized Difference Vegetation Index (NDVI).
