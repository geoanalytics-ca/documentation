# GEOAnalytics Canada Platform Documentation

## Why Create the GEO Analytics Canada Demonstration Platform?
Research on climate-change, ecosystem modeling, and environmental and natural resources monitoring is based on the collection, management, analysis, and dissemination of geospatial data.

Satellite Earth observation (EO) has been transformed by the massive increase in availability of open EO data, which began in 2008 with the opening of the Landsat data archive by the United States. It was given further impetus by the European Commission in making data from the Copernicus series of radar and optical satellites fully free and open since 2014.

On one hand, the increased availability of EO data makes it easier for scientists, businesses, and government decision makers to obtain insights from long, dense time series of multiple EO datasets. However, users cannot find, access, and use this wealth of EO data to its potential by following traditional approaches to download data and analyze it in a local computing environment.

As the volume of satellite EO data continues to grow, new analytical possibilities arise requiring new approaches to data management and processing.

***The solution is to bring the user to the data.***

To demonstrate how cloud computing systems can overcome issues with traditional approaches to satellite EO data analytics, Hatfield created the GEO Analytics Canada Demonstration Platform. This provides data, tools, and compute resources in the same environment and enables users to gain experience and understand the benefits of working in the cloud. 

This section will contain tutorials/documentation for users to get familiar with the GEOAnalytics platform. Within the tutorial_notebooks folder, you will find subfolders containing tutorial Jupyter notebooks ranging from showing the basics of Python, to full scientific, real-world workflows. 

## Table of Contents (within tutorial_notebooks)
> **Getting Started** (1_getting_started)
> - Introduction to Python
> - Jupyter Notebooks
> - Authentication
> - Desktop Virtual Machines
> - File Browser
> - SpatioTemporal Asset Catalog Introduction
> - Gitlab
> - Kubeflow Pipelines

> **Real World Examples** (2_real_world_examples)
> - Accessing Data
> - NDVI with Dask
> - NDSI with Dask
> - NDWI with Dask
> - Change Detection

> **Scientific Workflows** (3_scientific_workflows)
> - FLNRO-esque NDSI Average Pipeline

> **Static HTML Pages** (_build)
>  - Folders containing the HTML versions of the notebooks:
>     - `1_getting_started`
>     - `2_real_world_examples`
>     - `3_scientific_workflows`

> **Images** (images)
> - Folders containing the static images used in documentations within the notebooks.

> **Other files** 
> - Instructions on rebuilding HTML Docs: `tutorial_notebooks/build-doc.md.txt`
> - nbsphinx configuration script: `conf.py`
> - The Index reStructuredText file linking to respective files: `index.rst`
> - Makefiles for executing the build commands: `make.bat` & `Makefile`