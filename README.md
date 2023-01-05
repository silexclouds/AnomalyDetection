OBJECTIVE: prediction of potential anomalies in crops

Procedure: 
1.	Training: 
   - a.	Extraction of Vegetation Indices on fields with a validated anomaly
   - b.	Extraction of Vegetation Indices on fields with not-detected anomalies
   - c.	Ingestion of datasets for training
   - d.	Training and selection of model
2.	Prediction:
   - a.	Extraction of Vegetation Indices on fields to detect status
   - b.	Prediction of status based on trained model

Datasource: Copernicus Sentinel-2 satellite images
Vegetation Indices: SR, NDVI, GNDVI, NDRE, modNDRE, EVI, EVI2, PVR, GCI, RECI, TVI, MTCI, LCI, TCVI, GARVI, SIPI1, SIPI2, MCARI, ARVI,OSAVI.

Required applications:
 - o	Anaconda: Distribution of Python and R programming languages for scientific computing, suitable for Windows, Linux, and macOS, that aims to simplify package management and deployment. (www.anaconda.com)
 - o	Python Notebook
 - o	Orange Data Mining: Open source machine learning and data visualization application. License GPLv3. (www.orangedatamining.com)
 - o	Google Earth Engine account

Use cases:
Case 1: Analysis of fields with potential PSA occurences (based on trained datasets)
Procedure:
    - a.	Extract field information (video)
    - b.	Launch Prediction Workflow (video)

Case 2: Analysis of other potential anomalies. Required new trained datasets
Procedure: 
    - a.	Extract historical field information related to the crop anomaly and healthy fields (video)
    - b.	Launch Training Workflow (video)
    - c.	Extract Field information for new prediction (video)
    - d.	Launch Prediction Workflow (video)

Python Notebook required modules: geemap, ee, numpy, eemont, csv, os, io, requests, pandas, osgeo
•	geemap: is a Python package for interactive mapping with Google Earth Engine (www.geemap.org). To use geemap, is required a Google Earth Engine account (https://earthengine.google.com/)
•	ee: Google Earth Engine Python API package
•	eemont: This package extends Google Earth Engine with pre-processing and processing tools for the most used satellite platforms. (https://eemont.readthedocs.io/en/0.1.7/)
•	pandas: open source Python package most widely used for data science/data analysis and machine learning tasks (https://pandas.pydata.org/) 

