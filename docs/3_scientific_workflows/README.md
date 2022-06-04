# 3. SCIENTIFIC-WORKFLOWS 
These notebooks are example workflows from start to finish that you would typically see in a real-world situation.  

## [3.1 FLNRO-esque Kubeflow Pipeline](01-flnro-esque-pipeline.ipynb)
- This notebook will demonstrate how to create a Daily Normalized Difference Snow Index (NDSI) Average Pipeline for an Area of Interest with MODIS data.
- The user will:
    - Query MODIS snow product data using a bounding box AOI and date ranges
    - Threshold the NDSI
    - Create mosaics from raw MODIS granules by merging different tiles together
    - Compute the NDSI average over the date range
    - Visualize the NDSI averages