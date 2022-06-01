# 1. GETTING STARTED 
The “Getting Started” section will encompass the basic functionalities of the GEOAnalytics Canada platform. The user will be shown how they can achieve their goals when using the platform.  

## [1.1: Python Introduction](01-python-intro.ipynb)
- This notebook user will go through the basics of Python as well as common geospatial libraries that are used throughout the tutorials. The notebook will cover basics of Python, Rasterio, xarray, and GeoPandas.

## [1.2: Jupyter_Notebooks](02-jupyter-notebooks.ipynb)
- The user will be shown how to interact with GEOAnalytics Canada JupyterHub environment and the benefits/caveats that are associated with it.  
- The notebook will explain:
    - How to navigate through Jupyter and launch new notebooks and terminals. 
    - Where to launch Dask clusters. 
    - The locations of GEOAnalytics specific folders (e.g. public mounted buckets, private mounted buckets, personal storage – nfs).
    - How the Jupyter environment is set up (conda vs pip).

## [1.3: Authentication](03-authentication.ipynb)
- Through this notebook, users will:
    - Be shown what they can do with their level of authentication including system and API access. 
    - Learn about what they can do with their username and password (what systems they can access) 
    - Have the ability to copy and paste their GEOAnalytics API Token (found on the home page) into the notebook to demonstrate how to gain programmatic access to some systems/api’s.
    
## [1.4: Desktop VM’s](04-desktop-vm.ipynb)
- The basics of the Desktop VM interactions on GEOAnalytics Canada will be taught.
- Users will:
    - Be able to start a new remote desktop session and learn about the various options for machine type and GPU’s. 
    - Learn how to log into the remote desktop through the browser and through SSH. 
    - Be introduced to the remote desktop environment and where certain GEOAnalytics folder are (mounted buckets, personal NFS store).
    - Learn how to install new software into their desktop environments. 
    - Learn how they can check if they have GPU access (nvidia-smi).
    - Learn how to stop their remote desktop, with and without saving, and learn the tradeoffs. 
 
## [1.5: File Browser](05-file-browser.ipynb)
- A short notebook on navigating and interacting with the File Browser on GEOAnalytics Canada, where files can be downloaded from public and private buckets.

## [1.6: STAC](06-stac.ipynb)
- The SpatioTemporal Asset Catalog is introduced and users will be shown how to interact with the GEOAnalytics Canada STAC environment.  

## [1.7: Gitlab](07-gitlab.ipynb)
- In this notebook, users will be shown:
    - How to log in to git.geoanalytics and set up a project.
    - And how to set up with container registry and pushing/pulling images to the GEOAnalytics registry.

## [1.8: Pipelines](08-pipelines.ipynb)
- The basics of KubeFlow on GEOAnalytics Canada will be discussed. 
- User will learn how to create, compile, and run a pipeline within GEOAnalytics JupyterHub. 
- User will explore the features in the Kubeflow UI and review the built pipeline.