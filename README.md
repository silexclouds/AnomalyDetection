## OBJECTIVE: prediction of potential anomalies in crops

Procedure: 
1.	Training: 
      - a.	Extraction of Vegetation Indices on fields with a validated anomaly
      - b.	Extraction of Vegetation Indices on fields with not-detected anomalies
      - c.	Ingestion of datasets for training
      - d.	Training and selection of model
2.	Prediction:
      - a.	Extraction of Vegetation Indices on fields to detect status
      - b.	Prediction of status based on trained model

### Datasource: Copernicus Sentinel-2 satellite images
#### Vegetation Indices: SR, NDVI, GNDVI, NDRE, modNDRE, EVI, EVI2, PVR, GCI, RECI, TVI, MTCI, LCI, TCVI, GARVI, SIPI1, SIPI2, MCARI, ARVI,OSAVI.

Required applications:
+ Anaconda: Distribution of Python and R programming languages for scientific computing, suitable for Windows, Linux, and macOS, that aims to simplify package management and deployment (www.anaconda.com)
+ Python Notebook (https://jupyter.org/)
+ Orange Data Mining: Open source machine learning and data visualization application. License GPLv3 (www.orangedatamining.com)
+ Google Earth Engine account (https://earthengine.google.com/)

----

## Use cases:
### Case 1: Analysis of fields with potential PSA occurences (based on trained datasets)
Procedure:
  - a.	Extract field information (video)
  - b.	Launch Prediction Workflow (video)

### Case 2: Analysis of other potential anomalies. Required new trained datasets
Procedure: 
+ a.	Extract historical field information related to the crop anomaly and healthy fields (video)
+ b.	Launch Training Workflow (video)
+ c.	Extract Field information for new prediction (video)
+ d.	Launch Prediction Workflow (video)

----

Python Notebook required modules: geemap, ee, numpy, eemont, csv, os, io, requests, pandas, osgeo
| Module | Description |
| ------ | ------ |
| geemap | [is a Python package for interactive mapping with Google Earth Engine (www.geemap.org) To use geemap, is required a Google Earth Engine account (https://earthengine.google.com/) |
| ee | Google Earth Engine Python API package |
| eemont | This package extends Google Earth Engine with pre-processing and processing tools for the most used satellite platforms. (https://eemont.readthedocs.io/en/0.1.7/) |
| pandas | open source Python package most widely used for data science/data analysis and machine learning tasks (https://pandas.pydata.org/) |


#### Anaconda Environment: 
1)	Install Anaconda:
Install anaconda or miniconda: The geemap package has some optional dependencies, such as GeoPandas and localtileserver. 
2)	Create a new Environment
It is highly recommended to create a new conda environment to install geemap. Follow the commands below to set up a conda environment and install geemap and, which includes all the optional dependencies of geemap:

conda create -n [environment name]
To create an environment in which you can work and install the following packages you can use the file .condarc , and putting it into the user folder (naming it “.condarc”), and in the Anaconda prompt the following line “conda config”. 

Example of .condarc file:

    channels:
      - conda-forge
      - defaults
    create_default_packages:
      - python
      - ee_extra
      - eemont
      - geemap
      - google-cloud-sdk
      - pandas 
      - geopandas
      - numpy
      - spyder
      - spyder-kernels
      - jupyter
    ssl_verify: true

Note: On windows eliminate the “- google-cloud-sdk” package from the .condarc because is not provided on this channel of anaconda.

Earth Engine Authentication:
After the ee_extra and geemap installation, you can authenticate on GEE from command line in your environment anaconda prompt, and follow the instructions and enter with your google credentials:
   conda activate [environment name]
   earthengine authenticate

Google Earth Engine (GEE) packages in python (Anaconda environment)
- https://geemap.org/installation/
- https://github.com/r-earthengine/ee_extra
- https://eemont.readthedocs.io/en/latest/
- https://anaconda.org/conda-forge/google-cloud-sdk

Note: In case `ee_extra`, `eemont`, and `geemap` packages have not been installed in the initial Anaconda environment using the .condarc file, they can be installed later from the command line:

      Anaconda prompt -> 
      conda activate [environment name]
      conda install geemap -c conda-forge
      conda install ee_extra -c conda-forge
      conda install eemont -c conda-forge
      conda install geedim –c conda-forge

geemap is also available on PyPI. To install geemap, run "pip install geemap" and "pip install eemont" in the terminal.
                
Note: In case the command “earthengine authenticate” doesn’t work because the command ‘gcloud’ is not recognized, install the google-cloud-sdk package, close all Command Prompts and re-open the environment to proceed with the authentication. 
•	Linux: conda install -c conda-forge google-cloud-sdk
•	Windows: follow instructions described on: https://cloud.google.com/sdk/docs/install