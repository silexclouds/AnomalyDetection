## OBJECTIVE: prediction of potential anomalies in crops

**Procedure to follow:**

**1.	Training:**
- a.	Extraction of Vegetation Indices on fields with a validated anomaly
- b.	Extraction of Vegetation Indices on fields with not-detected anomalies
- c.	Ingestion of datasets for training
- d.	Training and selection of model

**2.	Prediction:**

- a.	Extraction of Vegetation Indices on fields to detect status
- b.	Prediction of status based on trained model


***Note:** For the training procedure is important to underline as **critical** the ingestion of validated datasets regarding the occurrence of an anomaly.*

---
#### Use case 1: Analysis of fields with potential PSA occurences (based on trained datasets)
+ a.	Extract field information (*VI_extraction.ipynb*)
+ b.	Launch Prediction Workflow (*C_Workflow_prediction.ows*)

#### Use case 2: Analysis of other potential anomalies. Required new trained datasets
+ a.	Extract historical field information related to the crop anomaly and healthy fields (*VI_extraction.ipynb*)
+ b.  Launch Data Preparation Workflow (*A_Workflow_dataPreparation.ows*)
+ c.	Launch Training Workflow (*B_Workflow_training.ows*)
+ d.	Extract Field information for new prediction (*VI_extraction.ipynb*)
+ e.	Launch Prediction Workflow (*C_Workflow_prediction.ows*)

----
## Datasource: 
**Copernicus Sentinel-2 satellite images**
## Vegetation Indices: 
**SR (Rouse et al. 1973), NDVI (Eitel et al. 2011), GNDVI (Gitelson & Merzlyak 1998), NDRE  (Gitelson & Merzlyak 1994), modNDRE (Datt
1999), EVI (Huete et al. 1994), EVI2 (Huete et al. 1997), PVR (Metternicht 2003), GCI (Gitelson et al. 2003), RECI (Gitelson et al. 2003), TVI (Broge &
Leblanc, 2001), MTCI (Dash & Curran 2004), LCI, TCVI, GARVI, SIPI1, SIPI2, MCARI, ARVI (Kaufman & Tanre 1996), OSAVI (Rondeaux et al., 1996).**

---

#### Anaconda Environment: 
The specific environment to run the Python Notebook **VI_extraction.ipynb** can be installed with the **environment.yml** file attached in the PythonNotebook folder.

And from the Anaconda Prompt type:

`conda env create -f environment.yml`

Activate the new environment: 

`conda activate myenv`

Verify that the new environment was installed correctly:

`conda env list`

- more information regarding how to manage Anaconda environments at: https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html
---

Required applications:
+ Anaconda: Distribution of Python and R programming languages for scientific computing, suitable for Windows, Linux, and macOS, that aims to simplify package management and deployment (www.anaconda.com)
+ Python Notebook (https://jupyter.org/)
+ Orange Data Mining: Open source machine learning and data visualization application. License GPLv3 (www.orangedatamining.com)
+ Google Earth Engine account (https://earthengine.google.com/)

---

Python Notebook required modules: geemap, ee, numpy, eemont, csv, os, io, requests, pandas, osgeo
| Module | Description |
| ------ | ------ |
| geemap | [is a Python package for interactive mapping with Google Earth Engine (www.geemap.org) To use geemap, is required a Google Earth Engine account (https://earthengine.google.com/) |
| ee | Google Earth Engine Python API package |
| eemont | This package extends Google Earth Engine with pre-processing and processing tools for the most used satellite platforms. (https://eemont.readthedocs.io/en/0.1.7/) |
| pandas | open source Python package most widely used for data science/data analysis and machine learning tasks (https://pandas.pydata.org/) |


1)	Install anaconda or miniconda: The geemap package has some optional dependencies, such as GeoPandas and localtileserver. 
2)	Create a new Environment
It is highly recommended to create a new conda environment to install geemap. Follow the commands below to set up a conda environment and install geemap and, which includes all the optional dependencies of geemap:

conda create -n [environment name]
To create an environment in which you can work and install the following packages you can use the file .condarc , and putting it into the user folder (naming it “.condarc”), and in the Anaconda prompt the following line “conda config”. An example of a **.condarc** file is attched on PythonNotebook folder.

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

Note: In case **ee_extra**, **eemont**, and **geemap** packages have not been installed in the initial Anaconda environment using the **.condarc** file, they can be installed later from the command line:

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

---

## Description of Orange workflows used for data manipulation, training and prediction. 

[![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/orange_logo_hq.png)](https://orangedatamining.com/)

---


## Data Preparation Workflow

![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/DataPreparation_workflow.png)

### Procedure:
+ Step 1: Open Orange Data Mining application and load the workflow, or launch it via command line:

    > *C:\Program Files\Orange3>orange-canvas C:\AnomalyDetection\Orange_Workflows\A_Workflow_dataPreparation.ows*
+ Step 2: reload the files selected in **1**
#### Workflow description:
+ 1 - Upload of datasets (*.csv format*) generated with VI Python Notebook (**fields_anomaly_true.csv** & **fields_anomaly_false.csv files**)
+ 2 - Select particular rows (fields) if necessary
+ 3 - Concatenation of True/False datasets into a single File
+ 4 - Save final dataset for training **fields_for_training.csv** (*required input for the Training Workflow*)


---


## Training Workflow

![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Training_workflow.png)

### Procedure:
+ Step 1: Open Orange Data Mining application and load the workflow, or launch it via command line:

    > *C:\Program Files\Orange3>orange-canvas C:\AnomalyDetection\Orange_Workflows\B_Workflow_training.ows*
     
+ Step 2: reload the file selected in **1** (fields_for_training.csv)
+ Step 3: check the model with the best metrics from **7** (Test and Score)
+ Step 4: check the model with the best metrics from **11** (Predictions)
#### Workflow description:
- 1 - Upload of the file with the information extracted from Sentinel-2 vegetation indices (fields_for_training.csv).
     *File format: Comma Separated Values (.csv)*
     
     ![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Training_workflow0.png)
     
+ 2 - Selection of Columns & Rows of interest
+ 3 - Data visualization
+ 4 - Data Sampler (for instance a selection of 75% of cases for training and 25% for testing)

     ![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Training_workflow1.png)
     
+ 5 - Visualization of data for training
+ 6 - Definition of model parameters
+ 7 - Testing & Scoring evaluation of selected models 

     ![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Training_workflow2.png)
     
     - Area under ROC is the area under the receiver-operating curve.
     - Classification accuracy is the proportion of correctly classified examples.
     - F-1 is a weighted harmonic mean of precision and recall
     - Precision is the proportion of true positives among instances classified as positive, e.g. the proportion of anomaly cases correctly identified as anomalies.
     - Recall is the proportion of true positives among all positive instances in the data, e.g. the number of anomaly cases among all identified as anomalies.
     - Specificity is the proportion of true negatives among all negative instances, e.g. the number of non-anomaly cases among all identified as non-anomalies.
     
       *More info regarding the **Test & Score** widget at: https://orangedatamining.com/widget-catalog/evaluate/testandscore/*
        
+ 8 - Models trained are saved to local folder
+ 9 - Visualization of data for testing
+ 10 - Load of models saved on **step 8**
+ 11 - Predictions based on saved trained models with a subset selected for testing

     ![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Training_workflow3.png)
     
+ 12 - Selection of the model to be applied for the anomaly detection *(in the example for the demo data provided, the selected is Random Forest model)*.


---


## Prediction Workflow

![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Prediction_workflow.png)

### Procedure:
+ Step 1: Open Orange Data Mining application and load the workflow, or launch it via command line:

    > *C:\Program Files\Orange3>orange-canvas C:\AnomalyDetection\Orange_Workflows\C_Workflow_prediction.ows*
    
+ Step 2: reload the file selected in **1**
+ Step 3: check the selected model in **3** (*default Random Forest*)
+ Step 4: check the predictions save in **5**
#### Workflow description:
- 1 - Upload of new polygon/s dataset/s (*in .csv format*)

   ![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Prediction_workflow0.png)
  
- 2 - Remove Nan values from new dataset (*cloud pixel*)
- 3 - Load of the selected trained model (*example: **RandomForest_Model.pkcls***)
- 4 - Prediction process

   ![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Prediction_workflow1.png)
   
- 5 - Save prediction (*in .csv format*)

   ![](https://github.com/silexclouds/AnomalyDetection/blob/main/readme/images/Prediction_workflow2.png)
   

---
## License

MIT

---

### References:
   - Broge, Niels & Leblanc, E. (2001). Comparing prediction power and stability of broadband and hyperspectral vegetation indices for estimation of green leaf area index and canopy chlorophyll density. Remote Sensing of Environment. 76. 156-172. 10.1016/S0034-4257(00)00197-8.

   - Gitelson AA, Gritz Y, Merzlyak MN 2003. Relationships between leaf chlorophyll content and spectral reflectance and algorithm for non-destructive chlorophyll assessment in higher plant leaves. Journal of Plant Physiology 160: 271–282.
   
   - Gitelson AA, Merzlyak MN 1998. Remote sensing of chlorophyll concentration in higher plant leaves. Advances in Space Research 22: 689–692.

   - Gitelson AA, Merzlyak MN 1994. Spectral reflectance changes associated with autumn senescence of Aesculus Hippocastanum L. and Acer Platanoides L. leaves: spectral features and relation to chlorophyll estimation. Journal of Plant Physiology 143:286–292.

   - Dash J., Curran P. (2004). The MERIS terrestrial chlorophyll index. International Journal of Remote Sensing - INT J REMOTE SENS. 25. 10.1080/0143116042000274015. 

   - Datt B. A new reflectance index for remote sensing of chlorophyll content in higher plants: tests using eucalyptus leaves. J Plant Physiol. 1999;154(1):30–36. https://doi.org/10.1016/S0176-1617(99)80314-9.

   - Huete A, Justice C, Liu H 1994. Development of vegetation and soil indices for MODIS-EOS. Remote Sensing of Environment 49: 224–234.

   - Huete AR, Liu H, Batchily K, van Leeuwen W 1997. A comparison of vegetation indices over a global set of TM images for EOS-MODIS. Remote Sensing of Environment 59: 440–451.

   - Metternicht G.; (2003). Vegetation indices derived from high-resolution airborne videography for precision crop management. Int. J. Remote Sens. 24. 10.1080/01431160210163074. 

   - Rondeaux G., Steven M., Frederic B.; (1996). Optimization of Soil-Adjusted Vegetation Indices. Remote Sensing of Environment. 55. 95-107. 10.1016/0034-4257(95)00186-7.
   
   - Rouse JW, Haas RH, Schell JA, Deering DW 1973. Monitoring vegetation systems in the great plains with ERTS. Third ERTS Symposium, NASA SP- 351 I: 309–317.
