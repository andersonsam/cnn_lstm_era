# CNN-LSTMs for regional hydrological modelling

This repository contains the code used in the study:

Anderson, Sam and Radic, Valentina.  ["Evaluation and interpretation of convolutional long short-term memory neural networks for regional hydrological modelling"](https://hess.copernicus.org/articles/26/795/2022/). Hydrology and Earth System Sciences, 2022.  

Contact: Sam Anderson  
Email: sanderson@eoas.ubc.ca  
___  
## Overview

The code in this repository can reproduce all figures and findings in the study.  All data used is publicly accessable and details to download data are given below.  This repository contains the following files:

* main_publish.ipynb: Defines functions, loads preprocessed data, builds/trains CNN-LSTM model, evaluates performance, interprets model learning, creates figures
* preprocessing.ipynb: Loads raw data (temperature, precipitation, streamflow, and basin outlines) and preprocesses into format used in main.ipynb
* figure_study_region.ipynb: Creates Figure 1 (study region).
* era5_download_P_075grid.py: Connects to ERA5 API and downloads raw precipitation data
* era5_download_T2m_075grid.py: Connects to ERA5 API and downloads raw temperature data
* non_contributing_areas.ipynb: Calculates non-contributing areas of basins in the eastern cluster
* mini.ipynb: A miniature version of main_publish.ipynb, which loads and structures one year of input/target data, clusters stream gauge stations, makes heat maps, and perturbs input temperature 
___  
## Model structure  

We use a sequential CNN–LSTM model to map weather predictors to streamflow at multiple stream gauge stations simultaneously throughout the region of southwestern Canada. 

<img src= "https://github.com/andersonsam/cnn_lstm_era/blob/master/Figures/fig02.png" width=75% >

As input data, we use the past 365 days of weather, covering the whole study region, in order to predict the streamflow of the next day at N stream gauge stations. The CNN learns the spatial features in each day of the input, while the LSTM model learns the temporal relationship between these features in order to predict streamflow.  

<img src= "https://github.com/andersonsam/cnn_lstm_era/blob/master/Figures/fig03.png" width=75% >

___
## Model evaluation and interpretation  

The model performs well overall, with a median NSE of 0.68 across the region.

<img src= "https://github.com/andersonsam/cnn_lstm_era/blob/master/Figures/fig06.png" width=75% >  

### Interpretation test 1: Spatial sensitivity

Over the test period, the model is most sensitive to spatial perturbations typically near and within the basins where streamflow is being predicted.  Fine-tuning the model on each individual subregion improves the model's ability to focus on a the area of the subregion.  Notably, the the eastern, north-eastern, and north-western clusters, the model appears to have two sensitive areas.

<img src= "https://github.com/andersonsam/cnn_lstm_era/blob/master/Figures/fig07.png" width=60% >  

### Interpretation test 2: Response to warming/cooling temperatures

When the input data is made warmer or cooler overall, the model responds by adjusting the timing and magnitude of the spring freshet in accordance with the timing the transition to above-freezing temperatures.

<img src= "https://github.com/andersonsam/cnn_lstm_era/blob/master/Figures/fig09.png" width=60% >  

### Interpretation test 3: Summer streamflow sensitivity to temperature in glacierized basins

When August temperatures are made warmer (cooler), glacierized basins are predicted to have more (less) streamflow.  Streamflow in more highly glaciated basins is more sensitive to August temperature.

<img src= "https://github.com/andersonsam/cnn_lstm_era/blob/master/Figures/fig11.png" width=60% >  

___

## How to run code

Practically, main_publish.ipynb runs best on a GPU to train the models much faster.  It is set up to run in Google Colab.  Google Colab does not access locally saved files; rather, it can access those in Github and Google Drive.  So, main_publish.ipynb can be run on Colab via Github, and all outputs/required data can be saved/organized in Google Drive as outlined in the notebook.  The other files (preprocessing.ipynb, figure_study_region.ipynb, era5_download_P_075grid.py, era5_download_T2m_075grid.py, non_contributing_areas.ipynb) can be run locally.  Here we give instructions to replicate the results in Google Colab.

1. Download ERA5 data:

    * Locally, run era5_download_P_075grid.py and era5_download_T2M_075grid.py; save output files (ERA5_T_1979_2015_6hourly_075_grid_AB_BC.nc and ERA5_P_1979_2015_6hourly_075_grid_AB_BC.nc) locally in cnn_lstm_era/Data/ERA5/

2. Download streamflow data:

    * See [here](https://wateroffice.ec.gc.ca/mainmenu/tools_and_downloads_index_e.html) for instructions to download available data for all active and naturalized stream gauge stations in Alberta and BC.   ABActNatFlowAll.csv and BCActNatFlowAll.csv list the stations which should be downloaded.  Save streamflow data in ./Data/Flow/

3. Download basin outline data:

    * From [here](https://open.canada.ca/data/en/dataset/0c121878-ac23-46f5-95df-eb9960753375), download the folder WSC_Basins.gdb.  A direct download link and other information can be found [here](https://wiki.usask.ca/pages/viewpage.action?pageId=1766228079#HowtoloadandreprojectWaterSurveyofCanada%22WSC_Basins%22GeodatabaseinQGIS-Dataavailability)
Save this folder as ./Data/WSC_Basins.gdb/

4. Download provincial border shapefiles:

	* From [Statistic Canada](https://open.canada.ca/data/en/dataset/a883eb14-0c0e-45c4-b8c4-b54c4a819edb), download the "Provinces/Territories Cartographic Boundary File - 2016 Census" shapefile (SHP).  Note: This data is not necessary for the analysis, but it used for making maps.

5. Download glacier data:

	* Download the file 02_rgi60_WesternCanadaUS.shp by clicking 'Western Canada and USA' from the [Randolph Glacier Inventory V6.0](https://www.glims.org/RGI/rgi60_dl.html).  Save in Google Drive at './data/RGI/'.

6. Preprocess the raw ERA5, streamflow, and basin outline data using preprocessing.ipynb

7. Upload preprocessed files from Step 5 to Google Drive in folder './data/'.  Upload shapefiles from Step 4 to Google Drive in folder ./data/province_borders/ (e.g. ./data/province_borders/lpr_000b16a_e.shp)

8. Upload trained models (from './Models/') to Google Drive in folder './models/'.

9. Run main_publish.ipynb in Colab.

If interested in non-contributing areas in the eastern cluster:

10. Download non-contributing area data:
	* From [here](https://open.canada.ca/data/en/dataset/adb2e613-f193-42e2-987e-2cc9d90d2b7a), download the folder "HYD_AAFC_TOTAL_NON_CTRB_DRAIN.gdb" by clicking 'Pre-packaged FGDB files (Bilingual)' --> 'Access'.  Save this folder as './Data/HYD_AAFC_TOTAL_NON_CTRB_DRAIN.gdb/'.

11. Run non_contributing_areas.ipynb

If interested in the Reference Hydrometric Basin Network (RHBN) and how stations in the RHBN overlap with those in this study:

12. Download 'RHBN_Metadata.xlsx' from [Environment and Climate Change Canada](https://collaboration.cmc.ec.gc.ca/cmc/hydrometrics/www/RHBN/).  Save in Google Drive in './data/'.  This file is used in main_publish.ipynb.

___
## Miniature code

To reproduce some of the key results without downloading and structuring the whole datasets in Steps 1-12 above, you can use mini.ipynb.  This notebook loads enough preprocessed data to structure 1 year of climate reanalysis and streamflow data, load trained models, make sensitivity heat maps, and perturb input temperature data.  This notebook uses data saved in './Data/mini/' which can be uploaded to Google Drive (for access in Colab) in the folder './data_mini/'.  While mini.ipynb can be run locally, predicting streamflow under temperature perturbations (to identify freshet response) or spatial perturbations (to make heat maps) is much faster when predictions can be made in batches on a GPU (e.g. on Colab).

___
## File organization

Local organization:  

* cnn_lstm_era/  
  * main_publish.ipynb  
  * preprocessing.ipynb  
  * figure_study_region.ipynb  
  * era5_download_P_075grid.py  
  * era5_download_T2M_075grid.py  
  * Models/
  	* All trained bulk and fine-tuned models (.h5)
  * Data/  
  	* ERA5/  
		* ERA5_T_1979_2015_6hourly_075_grid_AB_BC.nc  
		* ERA5_P_1979_2015_6hourly_075_grid_AB_BC.nc  
	* Flow/
		* AB/  
			* ABActNatFlowAll.csv  
			* 05AA004_Daily_Flow_ts.csv  
			* ...  
			* 11AA026_Daily_Flow_ts.csv  
		* BC/  
		  	* BCActNatFlowAll.csv  
		  	* 07EA004_Daily_Flow_ts.csv  
		  	* ...
		  	* 10DA001_Daily_flow_ts.csv  
	* WSC_Basins.gdb/
		* ...
	* Mini/
		* x_intermediate_mini.pickle
		* y_mini.pickle
		* flowseason_norm.pickle
		* station_info.pickle
		* stationBasins.pickle

Google Drive organization (for Colab access)  

* My Drive/  
	* Colab Notebooks/  
		* cnn_lstm_era/   
			* models/  
			* output/  
			* heat_maps/ 
			* data/
				* province_borders/  
				* RGI/
			* heat_maps_mini/
			* data_mini/
